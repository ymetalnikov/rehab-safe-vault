---
date:               # YYYY-MM-DD
cycle_number:
duration_min:
agent:              # "claude" | "gemini" | "gpt" | other — which agent led this session
videos_total:       # how many video files in this session folder
videos_pending:     # videos with status: pending in their meta
red_flags: []       # list of rule IDs from 06_Metrics_and_Monitoring/Red_Flags.md that fired
---

# {{date}} Session

> Subjective layer of the session. Written incrementally during a **Live Session** by whatever agent is leading.
> Technique analysis lives in per-video `*.meta.md` files in this same folder, written only by Gemini.

## Red flags surfaced today

> Filled by the agent if any rule from `06_Metrics_and_Monitoring/Red_Flags.md` fired. Empty otherwise. Patterns flagged here are signals worth bringing up with the user's physiotherapist — they are not a diagnosis or an instruction.

<!-- Example:
- `pain_post_higher_than_pre` (Theraband ER/IR): pain after has been ≥2 above pain before for 2 sessions in a row. May be worth pausing this exercise until the next PT visit.
-->

## Context

- General state before:
- Sleep / fatigue / stress:
- Today's restrictions or warnings:

## Plan & checklist

For each exercise: ☑ done with video / ⊡ done without video / ☐ skipped.

- [ ] {{exercise_1}}    — videos:
- [ ] {{exercise_2}}    — videos:
- [ ] {{exercise_3}}    — videos:

## Per exercise

### {{exercise_1}}

Link: [[]]

- pain before (0–10):
- pain after (0–10):
- subjective difficulty (1–5):
- range of motion: less / same / more / can't_assess
- painful arc (optional, e.g. `90–130°`):
- arc quality (optional, e.g. `sharp at 110°, eases above 130°`):
- fear of movement (0–5):
- video takes: 0 / 1 / 2+ — filenames if any
- how it felt:
- what was hard:

#### Pain events (optional)

> Fill only if pain was noticeable enough to describe (intensity ≥ 3, or any time you use a specific sensation word). Use the controlled vocabulary in `02_Conventions/Glossary_and_Scales.md`. Skip the whole block if nothing notable happened.

<!-- Example:
- position: 90° abduction with external rotation
  quality: sharp
  intensity: 5
  pattern: end-range
  notes: eased when arm returned below 60°
-->

### {{exercise_2}}

Link: [[]]

- pain before:
- pain after:
- subjective difficulty:
- range of motion:
- painful arc (optional):
- arc quality (optional):
- fear:
- video takes:
- how it felt:
- what was hard:

#### Pain events (optional)

### {{exercise_3}}

Link: [[]]

- pain before:
- pain after:
- subjective difficulty:
- range of motion:
- painful arc (optional):
- arc quality (optional):
- fear:
- video takes:
- how it felt:
- what was hard:

#### Pain events (optional)

## Overall

- What went well:
- What was hard / what bothered me:
- Recurring signals (pain, fear, fatigue patterns):
- New symptoms today (burning / numbness / tingling / weakness / instability / swelling — if any):
- Questions for the specialist:
- What to check next time:

## Video queue note

If `videos_pending > 0` and the agent leading this session was not Gemini: the videos in this folder are queued for technique analysis. Open Gemini and run **Video Queue** to process them.

## Tags

#session #rehab
