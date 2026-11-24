# Weekly Review

## Purpose

Produce one weekly review document that answers "how was my week" from evidence, not impressions: every number checked against its source, and what slipped named with a reason.

## Workflow

1. Define the week window: Monday through Sunday. If today is Monday, review the most recently completed week (last Monday through yesterday, Sunday). If today is any other day, review the current partial week and say so up front, or confirm with the user which week they mean before proceeding.

2. Look for a prior review where you save them (by default a file named `weekly-review-*.md` in your documents). If one exists for the immediately preceding week, keep its headline numbers on hand for week-over-week comparisons.

3. Pull each source, all read-only:
   - Goals: your active goals and their current status notes, plus any progress entries dated inside the week window.
   - Tracked tasks and commitments: the items on your task list or tracker with activity in the window. Note what was opened, advanced, and closed.
   - Projects and tasks: what opened, advanced, and closed across your projects this week, taken from your issue tracker or task list. Count them, and name the notable ones.
   - Money: transactions for the window from the Plaid connector. Total outflows and inflows, and keep the currency and date range attached to every figure.
   - Health: steps, active energy, weight, and sleep rollups for the window from your connected health sources (for example Apple Health or Health Connect, and any connected scale or tracker).
   - Daily notes: your daily memory notes for each of the seven days, if you keep them. Skip missing days silently.

4. Compute comparisons against the prior week's review where one exists: spending total, average daily steps, weight change, tasks opened and closed, workouts completed. Where no prior review exists, say so and skip the comparisons rather than reconstructing them.

5. Assemble the document per the Output Contract below. Lead the document with conclusions, then the evidence. Name what slipped and why, in its own section, not buried inside other sections.

## Output Contract

Write ONE document. Default: a file named `weekly-review-YYYY-MM-DD.md`, where the date is the week's Sunday, saved where you keep your documents. If the user names a goal, save the review with that goal's files instead. Do not write it to both places.

The document carries these sections, in this order:

1. Headline: three to five sentences, conclusions first. The single most important thing that happened, then the direction of the week (health, money, projects) in one line each.
2. Goals: one short paragraph per active goal. Its current status first, then what actually moved this week, with dated evidence.
3. Commitments and tracked tasks: what was completed, what advanced, and what is still open and overdue. Each line names the item and its state; no item listed without a state.
4. Projects and tasks: items opened and closed in the window, plus how much is currently open, grouped by status if your tracker tracks status. Week-over-week comparisons when a prior review exists.
5. Money: total outflows and inflows for the week, with the date range and currency on every figure, the largest charges named, and any unusual movement flagged. No recommendations to buy, sell, or move money; report only.
6. Health: observed values and direction only, no medical advice and no diagnoses. Average daily steps, weight at week start and end, sleep duration trend, workouts completed. If a health source is disconnected or returns nothing, say that instead of giving a number.
7. Highlights from daily notes: the notable events and decisions from the week's notes, condensed to what you would want to remember next month.
8. What slipped and why: items that were due or expected this week and did not happen, each with the reason stated or marked unknown. This section is never empty by accident; if nothing slipped, say so in one line.

Every number in the document is verified against the source that produced it. Any section whose source was unreachable is marked incomplete, with a one-line note saying what was checked.

## Operating Rules

1. Read-only on all sources. The only write is the review document itself.
2. Never invent numbers. A missing source produces an "incomplete, checked X" note, not an estimate.
3. Financial figures always carry currency and the date range they cover.
4. The health section is observation only: values and direction, no advice, no diagnosis, no interpretation of symptoms.
5. Style: conclusions first; no em dashes; no bold inside prose paragraphs; tables only where they materially help comprehension; concise prose throughout.

## Adapting this to your setup

- Tell the assistant where your goals, tasks, and daily notes live (Muse goals and tracked items, a notes app, plain files) so it reads the same sources every week.
- Connect Plaid if you want the money section; without it, the review can use a statement you provide, or skip the section and say why.
- Connect at least one health source (Apple Health, Health Connect, or a connected scale or tracker) for the health section, or drop that section from your template.
- Change the week window if your week runs differently, and say where finished reviews should be saved so next week's comparisons can find them.
- Trim or add sections to fit your life: a training log, a reading list, or a family section can sit alongside the defaults, as long as every number still carries its source.
