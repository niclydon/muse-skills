# Check pipeline stage coverage freshness

## Measure coverage, not activity

- Start by naming each stage in the chain from raw landing to the surface the user sees, and write down what current data each stage should carry.

- Check the newest content date each stage holds, using the date of the event or message inside the row rather than the row creation time or the last run time.

- Compare that newest content date against the newest content date in the stage upstream, and report the gap in days for every stage.

- Treat row counts, run counts, an active schedule and a fresh status stamp as no evidence of coverage, because a stage can keep writing rows over old data while covering nothing current.

- Flag a stage as stale when its upstream carries newer content that never appears downstream within the stage's normal lag, even when the stage ran on schedule.

## Trace a golden fixture end to end

- Pick one recent real item whose correct outcome is known, such as a named plan with dates and a companion, or an urgent family event from the last few days.

- Follow that one item through every stage by its own identifiers, and record the last stage where it still appears in the expected form.

- Name the break by what is missing downstream, such as no episode, no candidate, no typed event or no surfacing, rather than by which job failed.

- Add each traced fixture to a small standing set, and replay the whole set after any fix so a repair for one break cannot hide another.

## Report and file the break

- Report coverage as a per-stage table in prose, giving each stage its newest content date, upstream gap and one fixture result.

- Open one ticket in your tracker per distinct break, with the fixture as the acceptance test and a freshness guard as part of the fix rather than a follow-up.

- Ask for a guard that alarms on content date lag, so the next silent stop shows up as a staleness breach instead of impersonating a healthy stage.

- Never declare a pipeline healthy from a single stage check, and label any stage you could not read as unverified.

## Alternatives for better results

- Replay fixtures from different source families, such as messages, mail and calendar, because a stage can cover one family and silently drop another.

- Check a quiet period against the source itself before calling it a break, because a genuinely quiet source and a stalled stage look alike from downstream alone.

## Adapting this to your setup

- Draw your own chain first: name each stage from the raw source to what the user sees, and what "current" means for each one.
- Pick your golden fixtures from your own data: recent, real items whose correct end state you can state without checking.
- Set a normal lag per stage (minutes, hours, a day) so "stale" is a number, not a feeling.
- Prerequisites: read access to each stage's store, and a tracker where the break tickets can be opened.
- The fixture set is the asset here; keep it small, dated, and replayable after every fix.
