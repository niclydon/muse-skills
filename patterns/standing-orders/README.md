# Pattern: Standing Orders

Assistants learn lessons constantly, and then do not change, as if the
lesson had never been written down. The reason is usually placement: a
lesson recorded in a log that the assistant never opens at the moment of
decision is functionally unwritten. Behavior changes only when the lesson
is compiled into a surface the assistant actually reads before acting.

Standing orders are that surface. They are the small set of durable,
approved rules the assistant works under every day, kept in one file, read
at the start of every session, and maintained through an open approval
process instead of silent edits. This pattern pairs naturally with the
twice-a-day journal pattern: the journal is where lessons surface, and
standing orders are where the lessons that generalize end up.

## The mechanics

1. **Lessons are proposed, not adopted.** When a journal entry (or any
   review of the work) produces a lesson that generalizes beyond the day
   it happened on, the assistant writes it into a candidate queue: a
   separate file that holds proposed orders waiting for a decision.
   Nothing enters the active file from the queue on the assistant's own
   say-so.
2. **The user approves as a batch.** Periodically the candidate queue is
   presented to the user, who approves or strikes each item. Approval is
   the user's; that is what makes an order an order rather than a habit
   the assistant picked up.
3. **Approved orders live in one active file.** Exactly one file holds the
   orders in force. The assistant reads it at the start of every session,
   alongside whatever morning summary the setup uses. If a rule is not in
   that file, it is not a standing order.
4. **Each order is one line, imperative, and checkable.** An order states
   an action that can be obeyed or visibly disobeyed, not a value or an
   aspiration. Each one carries its approval date and where it came from
   (which journal entry or incident produced it), so its origin can be
   re-examined later.
5. **Strikes happen in the open.** An order that proves wrong is struck in
   the active file itself, with the date and the reason, never silently
   deleted. The file keeps its own history, so a future reader can see
   what was tried and why it stopped.
6. **The day is graded against the orders.** The night journal entry
   checks the day against the active orders. An ignored order surfaces as
   a mark in that entry rather than expiring quietly, which is how the
   user finds out the system is drifting before the drift becomes damage.

## What makes a good order

- **General:** it applies to a class of situations, not one incident. The
  incident belongs in the provenance note, not in the wording.
- **Checkable:** a reader can look at a finished piece of work and say
  whether the order was followed.
- **Small:** the active list stays short enough to actually read at the
  start of a session. A file with eighty orders has no orders. When the
  list grows, merge near-duplicates and strike what practice has made
  automatic.

## Sample orders

These are examples of the form, written generically. Adopt the shape, not
necessarily the content; your orders should come from your own lessons.

- Verify a claim against the live source before reporting it as fact.
  (Sample — approved 2026-01-01; from a night entry where a stale summary
  was reported as current state.)
- After two identical failures, check the setup and tooling before
  changing the content again. (Sample — approved 2026-01-01; from a night
  entry where the same fix was retried three times.)
- Report a fix as proven only for the path actually tested; name every
  untested path as untested in the same sentence. (Sample — approved
  2026-01-01; from a night entry where a one-direction test was reported
  as a complete fix.)
- Read a document back in full after writing it and before sharing it; a
  truncated file is a failed document, not a shipped one. (Sample —
  approved 2026-01-01; from a night entry where a partial file went out.)
- Create anything new in its private or draft state by default; making it
  public is the user's decision, never an assumption. (Sample — approved
  2026-01-01; from a night entry where a default setting exposed work
  early.)

## Adapting this to your setup

- Keep three files: the candidate queue, the active orders file, and
  whatever journal or review process feeds the queue. Plain text is fine.
- Decide the approval rhythm: a batch review once a week, or whenever the
  queue reaches a handful of items, both work. What matters is that
  approval is a real event, not a rubber stamp.
- Wire the active file into session start. If the assistant does not read
  it before its first action of the day, the pattern is decoration.
