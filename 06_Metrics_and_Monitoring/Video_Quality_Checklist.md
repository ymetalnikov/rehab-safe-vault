# Video Quality Checklist

Binary pass/fail rules for the **Gemini quality gate**. Used before any technique analysis on a video. If any rule fails → `status: rejected`, `reject_reason` set, no analysis.

The point is determinism: the gate must be a yes/no for each rule, not a vibe.

## Pass/fail rules

A video must pass **all** of the following:

1. **`framing`** — the working joint and the body parts required to judge the movement are fully in frame from start to end. (No half-cropped shoulders, no head-out-of-frame for an overhead movement.)
2. **`movement_visible`** — the full movement is captured: clear start position, full range, clear end position. No mid-rep cuts.
3. **`reps`** — at least 1 complete repetition is visible (≥ 2 for cyclic exercises like pendulum / theraband sets).
4. **`stability`** — camera is stationary or near-stationary. No handheld walking, no panning during the rep.
5. **`lighting`** — body silhouette and joint positions are clearly readable. Not backlit into a black blob, not blown out into white.
6. **`clothing`** — clothing does not hide the working joint or relevant body landmarks (e.g. scapula visibility for shoulder work).
7. **`angle`** — angle matches what the exercise needs (side / front / 45°). If the exercise template specifies a required angle and this video doesn't match, this rule fails.
8. **`resolution`** — at least 720p. Lower is fine if everything else is unambiguously readable, but default to fail.
9. **`pattern_match`** — the visible movement is plausibly the exercise it claims to be (per `meta.md → exercise`). If it's clearly a different movement, fail with `pattern_mismatch`.

## Reject reasons (controlled vocabulary)

Use these as `reject_reason` values in `meta.md`:

- `framing` — required body parts cut off
- `movement_partial` — movement starts/ends out of frame, or mid-rep cut
- `no_full_rep` — fewer than required reps captured
- `unstable` — camera shake / panning makes the rep unreadable
- `lighting` — too dark / too bright to read joint positions
- `clothing` — joints / landmarks hidden by clothing
- `wrong_angle` — angle doesn't match what the exercise needs
- `low_res` — below 720p and unreadable
- `pattern_mismatch` — visible movement is not the claimed exercise
- `other` — anything else; explain in body

## Fix instruction

Whenever a video is rejected, the meta file must contain a **specific** instruction for the next take. Not "shoot better" — concrete, e.g. "shoot from the side, ~2m back, no zoom; start camera before you raise the arm".

## Hard rule

If the gate fails, the agent must not produce technique analysis. No "let me try anyway". Set `status: rejected`, fill `reject_reason`, write the fix instruction, move on.
