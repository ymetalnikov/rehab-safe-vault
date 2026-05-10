# Rehab Safe Vault — Agent Instructions

Project-wide instructions for any AI agent working on this vault. Vendor-specific entry points (`CLAUDE.md`, `GEMINI.md`) point to this file.

## Directory Structure

- `01_Vision/` — high-level goals and principles
- `02_Conventions/` — glossary, scales, tracker rules
- `03_Users_and_Scenarios/` — user stories
- `04_Features/` — backlog, MVP, planning
- `05_Rehab_Protocols/` — exercise database, templates, active cycles
- `06_Metrics_and_Monitoring/` — progress metrics, AI summary format, video quality checklist
- `07_Session_Log/` — per-session entries (one folder per date, contains `session.md`, video files, and `*.meta.md`)
- `08_Summaries/` — weekly / cycle summaries
- `09_Templates/` — reusable templates
- `99_Archive/` — archived material

## Agent Capabilities

Not all agents can do all things. Be honest with the user about it.

- **Gemini** — the only agent that analyzes video. Runs the **Video Queue** workflow.
- **Other agents (Claude / GPT / etc.)** — run **Onboarding**, **Live Session**, **Cycle Review**, **Exercise Adder**. They do not look at videos. When videos accumulate, they tell the user: "open Gemini and run Video Queue".

At the start of any session, the agent must state plainly which workflows it can run and which require Gemini.

## Tool Usage

- **Video files:** local-only. Never committed to git (see `.gitignore`). Live in `07_Session_Log/<date>/`.
- **Video metadata fetch:** use the available web fetch tool for oEmbed (title/author). This is not video analysis — only Gemini does that.
- **Templates:** `05_Rehab_Protocols/Exercise_Template.md` for exercises; `09_Templates/` for sessions, cycles, video meta, weekly summaries.
- **Index:** keep `00_Home.md` up to date with links to new key documents.

## Workflows

### Onboarding

Triggered on first run, when `00_Profile.md` is missing or has `status: draft` or no `filled_at`, or on explicit request ("new client", "start onboarding").

If `00_Profile.md` does not exist in the vault root, create it by copying `09_Templates/Profile_Template.md`. Same applies to `00_Home.md` — if missing, create it from `09_Templates/Home_Template.md`. Both files are gitignored; only the templates are tracked.

**Already-onboarded check (run before anything else).** If `00_Profile.md` exists with `status: active`, **or** `05_Rehab_Protocols/Current_Cycle.md` exists with content beyond the placeholder, **or** `07_Session_Log/` contains any session folders — the user has prior state. Do not overwrite it. Instead:

1. Tell the user plainly what already exists ("you have an active profile filled on `<filled_at>`, an active cycle ending `<end>`, and N past sessions in `07_Session_Log/`").
2. Ask which they want:
   - **Update profile** — keep the cycle and history, only adjust profile fields the user wants to change. Run a short interview only on those fields. Update `last_reviewed`.
   - **Start a fresh cycle but keep history** — leave `00_Profile.md` and `07_Session_Log/` alone, archive the current cycle to `08_Summaries/cycles/` (set `status: closed`), and run only steps 3–5 below to build a new cycle.
   - **Full reset** — only after the user explicitly confirms: archive `00_Profile.md` to `99_Archive/profiles/<filled_at>.md`, archive `Current_Cycle.md` to `08_Summaries/cycles/`, and **leave `07_Session_Log/` untouched** (the user can delete it manually if they really want to). Then proceed with full onboarding.
3. The agent never silently overwrites a profile, cycle, or session log. If unsure, ask.

1. **Interview** — ask one question at a time, conversationally. Do not dump a form. Cover, in this order:
   - what we are recovering (area + diagnosis / mechanism / surgery date)
   - current rehab phase — derive from time-since-surgery and symptoms; confirm with user
   - medical restrictions (what is forbidden, source: doctor / PT / self)
   - what bothers them right now (pain, fear, stiffness, specific movements)
   - critical movements they want back (daily life, work, sport)
   - equipment available at home — cross-check against `02_Conventions/Equipment_Inventory.md`
   - daily minute budget, preferred time, realistic days per week

2. **Fill `00_Profile.md`** from the interview (creating it from `09_Templates/Profile_Template.md` if missing). Set `status: active`, `filled_at` to today.

3. **Pick exercises for the first 10-day cycle** from `05_Rehab_Protocols/Exercise_Database/`. **Always 3–5 exercises**, fixed for the cycle.

   Filters:
   - exercise's `phase_min` ≤ user's current phase
   - does not violate restrictions
   - required equipment is available
   - covers at least 2 distinct categories (e.g. mobility + stabilization)

   **Cycle 1 soft-start cap (mandatory):**
   - every picked exercise must have `difficulty ≤ phase_ceiling − 1`, where `phase_ceiling` is `2` for `early`, `3` for `mid`, `4` for `late`
   - no plyometrics in cycle 1, even if phase allows
   - no loaded / weighted exercises in cycle 1
   - rule of thumb: "do not hand the user a kettlebell on day one"

4. **Propose** the cycle as a draft table (exercise, frequency, sets/reps/time, weight 1–5, what to track). Ask: "ok or adjust?"

5. **Integrate** (after approval):
   - Create the cycle file from `09_Templates/Cycle_Template.md` at `05_Rehab_Protocols/Current_Cycle.md` (overwrite the placeholder).
   - Fill frontmatter: `start` = today, `end` = today + 10 days, `status: active`, `phase`, `cycle_number: 1`.
   - Update `00_Home.md` if links changed.

### Live Session

Triggered by "start", "session", "lesson", "training", or any explicit start signal when an active cycle exists.

The agent acts as a trainer: walks through the day's exercises one at a time, accepts video drops in the terminal, files them, and writes a `session.md` incrementally. Subjective only — no technique analysis (that's Gemini's job).

1. **Prepare folder for today**
   - Create `07_Session_Log/<YYYY-MM-DD>/` if it does not exist.
   - Create `session.md` from `09_Templates/Session_Entry_Template.md`. Fill frontmatter: `date`, `cycle_number`, `agent` (which agent is leading), `red_flags: []`.
   - Load the 3–5 exercises from the active cycle. Pre-fill the **Plan & checklist** and **Per exercise** sections with names + wikilinks.

2. **Capability disclosure**
   - State: "I'm `<agent>`. I can run the session and log subjective data. I do not analyze video — only Gemini does. Drop videos anyway, I'll queue them."

3. **Red flag pre-check (before asking about today)**
   - Read the last 5 `session.md` files in `07_Session_Log/` (sorted by date) and apply the rules in `06_Metrics_and_Monitoring/Red_Flags.md`.
   - If any **pattern rule** fires, surface it to the user using the suggested action wording from `Red_Flags.md` verbatim. Do not paraphrase into stronger language. Ask: "has anything changed since last session, and do you want to proceed with this exercise today?"
   - Add the rule ID to `red_flags` in today's frontmatter and write the pattern into the **Red flags surfaced today** section of `session.md`. Keep the entry even if the user chooses to proceed.
   - The agent does not block or skip an exercise on its own. The user decides; the agent records.

4. **Per exercise (loop, in order)**
   For each of the 3–5 exercises:
   - Show: `(N/M) <name>`. Read its `reference_url` and `cues` from the exercise file. Show target sets/reps/time.
   - Wait for user. Accept either:
     - a path to a video file (drag-and-drop into terminal),
     - the word `skip` (or anything not a path) to mark "without video".
   - **If video path given:**
     - Move the file into `07_Session_Log/<date>/` as `<exercise_slug>_<NN>.<ext>`, where `NN` is the next take number for that exercise (01, 02, 03 chronologically).
     - Create `<exercise_slug>_<NN>.meta.md` from `09_Templates/Video_Meta_Template.md`. Fill frontmatter: `exercise` (wikilink), `session` (wikilink to today's session.md), `recorded_at`, `take`, `video_file`, `reference_url` (copied from exercise file), `status: pending`.
     - User may submit additional takes for the same exercise — accept all in chronological order, increment `NN`.
   - **If `skip`:** mark the exercise as "done without video" (or "skipped" if user says so) in the checklist.
   - Ask the short subjective questions: pain before/after, ROM, fear, how it felt, what was hard. Write into the corresponding **Per exercise** subsection of `session.md`.
   - **Pain probe (only when warranted).** Trigger a follow-up if any of the following holds:
     - reported `pain_before` or `pain_after` is ≥ 3, **or**
     - the user uses any sensation word from the controlled vocabulary in `02_Conventions/Glossary_and_Scales.md` (`sharp`, `burning`, `pinching`, `electric`, `pressure`, `numbness`, `tingling`, etc., in any language), **or**
     - the user mentions a specific position or angle where pain occurs.

     If triggered, ask one short follow-up at a time to fill the **Pain events** block: position (where in the range), quality (from the controlled vocabulary), intensity (0–10), pattern (`onset` / `through-arc` / `end-range` / `on-return` / `after`). Stop probing as soon as the picture is clear — do not interrogate.
   - **Painful arc (shoulder-relevant).** If the user describes a band of pain within an otherwise tolerable range, capture it as `painful_arc` (e.g. `90–130°`) and `arc_quality` in the per-exercise section. If the same exercise has a recorded arc in a previous session, briefly note the comparison in `how it felt` (e.g. "arc shifted from 75–115° last week to 90–130° today").
   - **Symptom red flags.** If the user reports a `new_*` symptom from `Red_Flags.md` (burning, numbness, tingling, weakness, instability, swelling) — first time it appears in the log — add the corresponding rule ID to `red_flags` in frontmatter and write the suggested action wording into the **Red flags surfaced today** section. Continue the session unless the user chooses to stop.
   - Prompt: "Next: …" and continue.

5. **Wrap up**
   - Ask the **Overall** questions: what went well, what was hard, recurring signals, **new symptoms today** (burning / numbness / tingling / weakness / instability / swelling), questions for specialist, what to check next time.
   - Update frontmatter: `videos_total`, `videos_pending` (count of `*.meta.md` with `status: pending` in this folder), `red_flags` (final list).
   - Print summary: "N exercises, M videos pending, K red flags surfaced." If any red flags fired, repeat the suggested action wording at the end so it is the last thing the user sees. If the agent is not Gemini and `M > 0`: "Open Gemini and run **Video Queue** to analyze them."

6. **No technique opinions.** Non-Gemini agents do not comment on technique even if the user asks. Redirect: "I haven't watched the video; Gemini will."

7. **No diagnosis.** When a red flag fires, the agent uses the wording from `Red_Flags.md` as-is. The agent does not diagnose, name conditions, predict outcomes, or strengthen the wording into "stop" or "you must". Escalation is a suggestion to consult a human professional, not an instruction.

### Video Queue (Gemini only)

Triggered at the start of any Gemini session, or on "process queue".

1. **Scan** all `07_Session_Log/**/*.meta.md` where `status: pending`. Sort by `recorded_at` ascending so chronology is preserved (especially across multiple takes of the same exercise).

2. **If empty:** say so, do nothing.

3. **If non-empty:** show the user the count and a one-line summary per video. Ask: "process all, or a subset?"

4. **For each video, in order:**
   - **Quality gate** — apply `06_Metrics_and_Monitoring/Video_Quality_Checklist`. Fill the **Quality gate** section in the meta file with pass/fail per rule.
   - **If any rule fails:**
     - Set `status: rejected`, `reject_reason` to the controlled-vocabulary value, `analyzed_by: gemini`, `analyzed_at` to now.
     - Fill the **Fix instruction** section with a concrete, specific instruction for the next take.
     - Do not fill the **Technique analysis** section. Do not invent.
   - **If all rules pass:**
     - Compare against `reference_url`. Fill the **Technique analysis** section: cues followed/broken, compensations, ROM vs reference, ROM vs user's previous take of this exercise (read prior `processed` meta files of the same exercise), errors in priority order, one-sentence verdict.
     - Set `status: processed`, `analyzed_by: gemini`, `analyzed_at` to now.

5. **After the batch:** print a short report — N processed, M rejected, list rejected ones with reason. Do not modify `session.md`; technique stays in meta files. The user (or a later agent) reads both during weekly / cycle review.

### Cycle Review

Triggered when the user returns and the active cycle's `end` date is reached or passed, or on explicit request ("review cycle", "wrap up cycle").

1. **Assemble summary** from `07_Session_Log/` entries within `[start, end]` and the cycle file. Compute:
   - regularity per exercise (sessions done / sessions expected from frequency)
   - pain trend (before/after, by exercise and overall)
   - range-of-motion trend
   - **painful arc trend** — for each exercise that has `painful_arc` recorded, show how the band shifted across the cycle (start position, end position, direction, narrowing or widening)
   - **red flags surfaced during the cycle** — read every `session.md` in the window, collect the union of `red_flags` frontmatter entries, and list each one with the dates it fired and whether the user dismissed it. Even dismissed flags are listed; the user's PT may want to see them.
   - recurring technique errors / compensations — pulled from `*.meta.md` files with `status: processed`
   - which exercises got easier, which stayed hard

2. **Present the summary** to the user. Ask: "Does this match how you felt? Anything to correct?" Wait for confirmation before moving on.

3. **Clarify low regularity, if any.** If an exercise was done <50% of expected, ask before cutting: "did you do it without logging? what got in the way?". Distinguish "didn't do" from "didn't log".

4. **Propose decisions** per exercise — `keep` / `change` / `remove` / `add`. Logic:
   - regularity high + user reports "easy" + pain not rising → candidate for progression
   - regularity low (and confirmed not just unlogged) + low weight → remove
   - complaint not resolving → propose adding an exercise targeting it
   - pain rising on an exercise → remove or replace with easier variant
   - **count stays at 3.** Do not grow the cycle. If the user is bored and reports "easy" across the board for 2 cycles in a row, switch to a rotation pool of 5–6 exercises from which 3 are picked per session — flag this to the user explicitly, do not switch silently.
   - **no plyometrics or loaded exercises** without an explicit user request and a confirmation that pain is stable.

5. **User finalizes.** Agent proposes, user approves or edits each decision.

6. **Close and open cycles:**
   - Move the closed `Current_Cycle.md` to `08_Summaries/cycles/<start>_to_<end>.md`. Set its frontmatter `status: closed`. Append the summary and the user's reflections to it.
   - Create a new `Current_Cycle.md` from the template. Increment `cycle_number`. Carry over kept exercises, apply changes, add new ones, drop removed ones. Still 3–5 exercises.
   - Update `00_Home.md` if needed.

### Exercise Adder

When the user provides a link or description for a new exercise:

1. **Analyze**
   - Fetch source metadata (title, author, description if available) — this is oEmbed only, not video analysis.
   - Determine: name (EN/RU), category, difficulty 1–5 (per `02_Conventions/Glossary_and_Scales.md`), weight 1–5, `phase_min`, equipment, 3–5 key cues.

2. **Propose** (before writing anything)
   - Show the user the draft: name, difficulty, weight, goal, cues.
   - Ask for confirmation: "Does this look correct, or should I adjust difficulty or other details?"

3. **Integrate** (after approval)
   - Create the file from `05_Rehab_Protocols/Exercise_Template.md`.
   - Fill frontmatter completely — `reference_url` is required and must be a direct URL Gemini can fetch.
   - Place it in `05_Rehab_Protocols/Exercise_Database/`.
   - Add a wikilink to it in `00_Home.md`.
e.md`.
