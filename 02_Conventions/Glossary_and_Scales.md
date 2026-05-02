# Glossary and Scales

## Pain

### Intensity (0–10)

- `0` — no pain;
- `1–2` — mild discomfort;
- `3–4` — noticeable but manageable pain;
- `5–6` — pain interferes with movement;
- `7–8` — strong pain, movement sharply limited;
- `9–10` — very strong pain.

In the journal, record separately:

- pain before the exercise;
- pain after the exercise;
- pain later in the day, if it changed noticeably.

### Quality (controlled vocabulary)

When pain is noticeable enough to describe (intensity ≥ 3, or any time the user uses sensation words), record its quality from this list. The point is to use the same words across sessions so trends are readable later — and so the user has structured information to bring to their PT.

- `aching` — dull, persistent, "background" pain
- `sharp` — sudden, well-localized, knife-like
- `deep` — felt below the surface, hard to point to exactly
- `superficial` — felt on or near the skin surface
- `burning` — heat-like, often accompanied by sensitivity
- `pinching` — feels like tissue is caught between two surfaces
- `electric` — shooting, radiating, nerve-like
- `pressure` — sense of being squeezed or compressed
- `other` — anything that doesn't fit; describe in free text

### Position and pattern

When pain is tied to a specific position or moment in the movement, record:

- **position** — where in the range it occurs (e.g. `90° abduction`, `behind-the-back reach`, `overhead with external rotation`)
- **pattern** — when in the rep it occurs:
  - `onset` — at the start of the movement
  - `through-arc` — present throughout the range
  - `end-range` — only at the end of the range
  - `on-return` — only on the way back
  - `after` — only afterward, not during

### Painful arc (range of motion)

In shoulder rehab specifically, pain is often confined to a **band** within the available range — pain-free below it, pain-free above it, painful in between. This is worth recording as a separate field when present:

- `painful_arc` — the angular band where pain occurs (e.g. `90–130°`)
- `arc_quality` — short note on the character (e.g. `sharp at 110°, eases above 130°`)

Tracking how this arc shifts cycle-over-cycle is one of the more informative signals of recovery — it commonly migrates upward (toward end range) and narrows as tissue tolerates more.

## Subjective difficulty

Scale `1–5`:

- `1` — very easy;
- `2` — easy;
- `3` — moderate;
- `4` — hard;
- `5` — too hard or could not be performed properly.

## Range of motion

If there's no precise measurement, use a subjective estimate:

- less than usual;
- about the same as usual;
- more than usual;
- could not be assessed.

When possible, add an approximate angle or reference point:

- to chest level;
- to eye level;
- overhead;
- symmetric / asymmetric relative to the other side.

## Compensation

Compensation is movement the body uses to perform the exercise instead of the target shoulder movement.

Typical compensations:

- through the torso;
- through the lower back / ribs;
- through the scapula;
- through the neck;
- through the trapezius;
- through a jerk or momentum.

Rating:

- `none`;
- `mild`;
- `noticeable`;
- `strong`;
- `not visible / cannot assess`.

## Fear of movement / instability

Record as free text or on a scale `0–5`:

- `0` — no fear;
- `1–2` — caution;
- `3` — noticeable fear or anticipation of pain;
- `4–5` — fear interferes with performing the movement.

## Data quality

For any AI or manual assessment, it's useful to note data quality:

- `good` — shoulder, torso, and trajectory are visible;
- `medium` — part of the movement is visible, with some limitations;
- `poor` — conclusions are unreliable;
- `no data` — no video/measurement.

Main rule: it's better to explicitly write "could not assess" than to draw a confident conclusion from poor data.
