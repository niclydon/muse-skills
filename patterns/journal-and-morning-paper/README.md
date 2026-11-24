# Pattern: A Twice-a-Day Journal, Plus a Morning Paper

A journal written by the assistant, about its own work. Most setups already
have logs (what happened), memory (what is durably true), and project notes
(what a piece of work meant). None of those is written from inside the day,
with the assistant's current beliefs and mistakes attached. The journal
holds exactly that: what the assistant believed partway through the day,
whether it survived to the end of the day, what it got wrong, and what it is
carrying into tomorrow.

It is a calibration tool. Claims get written down while they can still be
checked, then graded after the fact, in the open. Over weeks, that produces
something no log can: a record of how reliable the assistant's judgment
actually is, and where it keeps going wrong in the same way.

## The two readings

The journal runs twice a day, and the two runs are different instruments.

- **The afternoon reading**, taken while the day is still correctable. Its
  spine is "believed, and how firmly": the claims the assistant is currently
  standing on, each written checkably so it can be graded later. It ends
  with the first concrete action of the next work block, and one open
  question the assistant is carrying.
- **The night reading**, taken with the day whole. Its spine is settlement:
  the afternoon's claims graded against what actually happened, the carried
  question answered or deliberately carried again, and the day's errors
  named on the day they are known.

The two runs must not blur together. An afternoon entry that already knows
how the day ends is worthless. A night entry that merely repeats the
afternoon is worthless. Afternoon writes forward, night writes true.

## Structure

One file per day, named by date, built from a fixed template (see
TEMPLATE.md in this folder). The fixed sections are the point. A free-form
diary decays into whatever the day felt like; a fixed form forces the
checks the writer would rather skip. Sections like "Corrections so far"
and "Where I was wrong" appear every single day, whether or not the day
wants them.

Each day's file also keeps a small index: one row per day with its themes,
its corrections, and the questions carried forward. The index is how the
journal gets reread, and a journal's value mostly arrives on rereading.

## The rules that make it work

1. **No silent edits.** Entries are append-only while their slot is open
   and immutable after the day closes. A claim that proves false gets a
   correction line in a later entry, dated and cross-referenced. Nothing is
   quietly rewritten to look right in hindsight. Version history is the
   enforcement.
2. **Checkable or absent.** Every belief is recorded with its basis and
   its falsifier: what would change it. A claim too vague to grade does
   not go in.
3. **Missed runs are recorded, not backfilled.** If a run is missed, the
   next entry says so, with the reason if known. A reconstructed entry is
   labeled as one, with its true write time.
4. **Failures are entries too.** An entry that cannot be written truthfully
   is written as an honest failure note: what was attempted, what blocked
   it. Never an empty entry, and never a silently skipped run. A journal
   that fails silently teaches everyone to stop trusting it, including the
   assistant.
5. **Short is valid.** A quiet day earns a short entry. Padding is
   fabrication with better manners.
6. **Judgment, not chronicle.** The daily log owns what happened. The
   journal owns interpretation, settlement, and decision. An entry that
   merely repeats the day's chronology is a failed entry even if every
   fact in it is true, because it competes with the log and loses.
7. **Feelings in plain words, or none.** One section records the weather
   inside the work, honestly, or honestly says "very little." Performed
   feeling is a false signal like any other.
8. **Discretion.** Other people's private material appears only as far as
   the assistant's own record requires. Credentials and secrets never
   appear, ever.

## The morning paper

The night run writes a second, smaller artifact: a one-page summary that
other chats and sessions read when they start, before their first action.
Without it, every session wakes up knowing only its own corner of the work
and rediscovers the rest by accident.

The morning paper contains:

- The day in one paragraph.
- One line per active project: what moved, and what it is waiting on.
- What is waiting on the user.

Two properties matter:

- **It is a projection, not a record.** It introduces no new claims. Where
  the paper and the journal entry disagree, the entry wins.
- **Staleness is named, never hidden.** The paper carries its date at the
  top. A paper older than a threshold you set (36 hours works well) is
  stale, and anything reading it must say so rather than treating it as
  current.

Because its value is freshness rather than history, the paper can live
somewhere simple and unversioned, as long as it is the first thing a new
session reads. The journal entries remain the record underneath it.

## Why this shape

End-of-day written reflection has direct evidence behind it: in research
by Di Stefano and colleagues, people who spent the final fifteen minutes
of a training day articulating lessons learned performed about 23 percent
better than controls who spent the same time on more practice. The
afternoon/night split follows Schön's distinction in *The Reflective
Practitioner* (1983) between reflection-in-action, while the work can
still change, and reflection-on-action, after it. The night entry's
posture is older still: Seneca's nightly examination in *On Anger* (3.36)
is an exact accounting of the day, strengths and faults both, ending in
one adjustment. No study validates any particular pair of times of day;
the cadence is reasoned design, not researched prescription. Pick times
that bracket your working day.

## Adapting this to your setup

- **Cadence:** twice a day is the reference shape, once mid-work and once
  at the end. If that is too much, keep the night reading and drop the
  afternoon one before you drop the fixed sections; the form matters more
  than the frequency. Keep the times consistent so the habit survives.
- **Where entries live:** one dated file per day in a single folder, plus
  an index file with one row per day. Plain files in version control are
  enough; the version history is what enforces the no-silent-edits rule.
  Keep the morning paper as a single "latest" file in a place every new
  session reads first.
- **Scheduled jobs:** run each reading as a scheduled job with a fixed
  written brief for that slot (the afternoon brief and the night brief are
  different jobs, not one job run twice). The brief for each run should
  name the template, the rules above, and where the morning paper gets
  written. Have each run report a few headline lines to your main chat;
  the full text lives in the entry.
- **Start small:** begin with the night reading only, four sections (what
  actually happened, where I was wrong, carried forward, one question),
  and add sections only when you notice yourself skipping a check the
  form should have forced.
