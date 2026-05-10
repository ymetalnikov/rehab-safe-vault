# Vault Structure

## Canonical root

The canonical root of the knowledge base is the folder containing `README.md` and `00_Home.md`.

All working notes, templates, protocols, journals, and summaries are stored relative to this root.

## Sections

- `00_Home.md` — main project map and entry point.
- `01_Vision/` — why the tracker exists and what principles matter.
- `02_Conventions/` — guidelines, glossary, scales, structure.
- `03_Users_and_Scenarios/` — usage scenarios.
- `04_Features/` — MVP, backlog, product decisions.
- `05_Rehab_Protocols/` — exercises, protocols, current cycles.
- `06_Metrics_and_Monitoring/` — metrics, data quality, summary format.
- `07_Session_Log/` — actual session entries.
- `08_Summaries/` — weekly, 10-day, and monthly reviews.
- `09_Templates/` — templates for new entries.
- `99_Archive/` — outdated or irrelevant materials.

## Naming

### Session entry

Format:

`YYYY-MM-DD Session.md`

Example:

`2026-04-30 Session.md`

### Weekly summary

Format:

`YYYY-WW Weekly Summary.md`

Example:

`2026-W18 Weekly Summary.md`

### Cycle

Format:

`YYYY-MM-DD — YYYY-MM-DD Cycle.md`

Example:

`2026-04-30 — 2026-05-10 Cycle.md`

## Source-of-truth rule

- An exercise is described once in the exercise database.
- A protocol or cycle references the exercise via a link.
- A session entry records the fact of execution and sensations on a specific date.
- A summary doesn't replace the journal — it aggregates the trend over a period.

## Video and attachments

By default, store links to videos or paths to local files inside notes.

If video is stored locally, it's better to keep it outside the main note vault or in a separate attachments folder, so text entries and heavy files don't get mixed.
