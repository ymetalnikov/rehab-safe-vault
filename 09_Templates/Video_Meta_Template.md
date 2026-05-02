---
exercise:           # wikilink to exercise file, e.g. "[[Supine_Shoulder_Flexion_with_Dowel]]"
session:            # wikilink to that day's session.md, e.g. "[[07_Session_Log/2026-04-30/session]]"
recorded_at:        # ISO datetime, e.g. 2026-04-30T10:15
take:               # 1, 2, 3 — chronological order if multiple takes of the same exercise
video_file:         # filename, e.g. "pendulum_01.mov"
reference_url:      # copied from exercise frontmatter at session time, so analysis is reproducible later
status: pending     # pending | processed | rejected
reject_reason:      # one of: framing | movement_partial | no_full_rep | unstable | lighting | clothing | wrong_angle | low_res | pattern_mismatch | other
analyzed_by:        # "gemini" once processed; empty otherwise
analyzed_at:        # ISO datetime when analysis was written
---

# {{video_file}}

> While `status: pending`: this body is empty. Gemini fills it during the **Video Queue** workflow.
> While `status: rejected`: only the **Fix instruction** section is filled. No technique analysis.
> While `status: processed`: all sections below are filled.

## Quality gate

Result of `Video_Quality_Checklist`:

- framing:           # pass | fail
- movement_visible:  # pass | fail
- reps:              # pass | fail
- stability:         # pass | fail
- lighting:          # pass | fail
- clothing:          # pass | fail
- angle:             # pass | fail
- resolution:        # pass | fail
- pattern_match:     # pass | fail

## Fix instruction (rejected only)

Concrete, actionable. Example: "Shoot from the side, ~2m back. Start recording before you raise the arm."

-

## Technique analysis (processed only)

### Cues — followed / broken

Per cue from the exercise's `cues` list, mark followed or broken with one short observation.

- 
- 
- 

### Compensations observed

- torso:        # none / mild / noticeable / strong
- scapula:      # none / mild / noticeable / strong
- neck/trap:    # none / mild / noticeable / strong
- other:

### Range of motion

- Compared to reference video: less / similar / more
- Compared to user's previous take of this exercise: less / similar / more / no_prior

### Errors

What went wrong, in order of importance:

- 
- 

### Verdict

One sentence: is this take usable, what's the single most important thing to fix next time.
