# Red Flags

Rules the agent uses to surface patterns in your log that may warrant pausing and consulting your physiotherapist or doctor before continuing. This is not a diagnostic tool — it does not decide anything. It only highlights signals that, in your accumulated log, are worth a human's attention.

## Important framing

- **Not medical advice.** These rules look at your own self-reported numbers and notes. They do not see your tissue, your imaging, or your movement in person. The agent flags the pattern; the decision is yours and your PT's.
- **Conservative by design.** Better to surface a false alarm than miss a real one. If a rule fires and the situation is benign, you confirm with your PT and dismiss the flag — that is the intended workflow.
- **The agent escalates, it does not command.** Wording is always "this pattern may be worth bringing up with your PT", never "stop" or "you must".

## How the agent uses these rules

The agent checks against these rules at two moments:

1. **At the start of every Live Session** — before asking about today, the agent looks at the last 5 sessions and checks each rule. If a rule fires, the agent surfaces the pattern, asks if anything has changed, and lets the user decide whether to proceed.
2. **During Cycle Review** — every fired rule from the cycle window is included in the summary, even if the user dismissed it day-to-day.

When a rule fires, the agent records it in the session.md frontmatter as `red_flags: [<rule_id>, ...]` and writes a short note in the session body. Dismissed flags are kept in the log — they are not erased.

## Pattern rules (signals across sessions)

These rules look at trends across your last few sessions.

| ID | Pattern | Window | Suggested action wording |
|---|---|---|---|
| `pain_post_higher_than_pre` | `pain_after − pain_before ≥ 2` on the same exercise | 2 consecutive sessions | "This exercise has been leaving you in more pain than it found you in, two sessions in a row. Worth checking in with your PT before the next attempt." |
| `pain_pre_rising` | `pain_before` rises by ≥1 on the same exercise | 3 consecutive sessions | "Baseline pain on this exercise is creeping up across sessions. May be worth pausing it and asking your PT whether to swap or scale back." |
| `rom_regressing` | `rom` direction is `less than usual` on the same exercise | 5 consecutive sessions or 7-day window | "Range of motion on this exercise has been trending the wrong way for a while. Worth bringing up at your next PT visit, or sooner if it bothers you." |
| `painful_arc_widening` | painful arc range grows by ≥20° between sessions on the same exercise | 2 consecutive measurements | "The painful arc on this exercise has widened. This may be a signal worth discussing with your PT." |

## Symptom rules (signals on a single session)

These rules look at what the user reports today, regardless of history.

| ID | Symptom (reported by user, in any phrasing) | Suggested action wording |
|---|---|---|
| `new_burning` | First mention of burning sensation in or around the joint | "Burning is a new symptom in your log. Worth a message to your PT before the next session." |
| `new_numbness_or_tingling` | First mention of numbness, tingling, or pins-and-needles in the arm or hand | "Numbness or tingling has not been mentioned before. This is the kind of signal worth bringing up with your PT promptly, especially if it persists." |
| `new_weakness` | First mention of unexpected weakness — arm "giving way", inability to hold a position previously held | "A new sense of weakness has appeared in your log. Worth contacting your PT before continuing the program." |
| `new_instability` | First mention of the joint slipping, popping with pain, "coming out", catching | "Instability or catching has been mentioned today. Pausing the program and reaching out to your PT or surgeon (if relevant) is the conservative call here." |
| `new_swelling` | First mention of visible swelling around the joint | "Swelling is a new entry in your log. Worth pausing the program and asking your PT whether to continue." |
| `pain_severity_high` | `pain_after ≥ 7` on any exercise, even once | "Pain at this level is unusual relative to your baseline. Worth checking in with your PT before next session." |

## What the agent does NOT do

- The agent does not interpret these patterns as a diagnosis.
- The agent does not change the cycle on its own when a flag fires.
- The agent does not delete or hide a flag — only the user can mark a flag as `dismissed` after consulting their PT, and the original record stays in the log.
- The agent does not add new rules silently. New rules are added to this file with their reasoning, and only after that does the agent start applying them.

## Glossary of "first mention"

A symptom counts as `new_*` if no prior session in `07_Session_Log/` mentions it in free-text notes or in the `pain_event.quality` field. The agent searches case-insensitively across `session.md` files and pain event records. A symptom that was mentioned, dismissed by the user as "fine, ignore", and now reappears still counts as worth flagging again.
