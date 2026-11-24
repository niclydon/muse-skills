# Bounded Fanout

## Scope the fanout

- Count the exact targets before sending anything and record the target list, the batch size and the total batch count in the run notes.

- Exclude chats with a standing role, parked chats and chats that already received the same message in the current campaign, unless the user explicitly named them.

- Split any dispatch over 20 targets into batches of at most 10, and never run two batches from the same campaign at the same time.

## Pilot and pace

- Send one pilot batch first and wait for its delivery results before starting the next batch.

- Continue only when the pilot batch shows deliveries landing without database timeouts, pool timeouts or repeated tool failures.

- Pace later batches with a pause between them, and stop the campaign when the pause no longer clears the errors.

## Handle pressure and retries

- Treat a database pool timeout, or a batch where most deliveries fail, as a capacity signal, not as a reason to resend the whole campaign at once.

- Retry only the targets with no confirmed delivery, one smaller batch at a time, and keep a delivered list so no target gets the same message twice.

- Report the campaign as paused on capacity when two consecutive batches hit the same timeout shape, and name the first failing batch and the count still unsent.

## Verify and report

- Verify delivery per batch from the delivery results themselves, not from the send call returning without an error.

- Close the campaign with counts that add up, covering targets, confirmed deliveries, retries, duplicates avoided and targets left unsent.

- Name any batch that timed out by its position in the campaign, so a later run can resume from the delivered list instead of starting over.

## Alternatives for better results

- Narrow the campaign to the chats that are actually stale or actually need the instruction, since most mass dispatches shrink sharply once standing and parked chats are removed.

- Replace a mass dispatch with a single notice in the coordinating chat when the recipients only need to read the information rather than act in their own chats.

## Adapting this to your setup

- Set your own batch ceiling and pause length to match the limits of the system you are sending through; 10 per batch is a starting point, not a law.
- Keep the delivered list somewhere durable (a file or note tied to the campaign) so a resumed run never double-sends.
- Decide which targets are always excluded in your setup, such as standing-role chats or archived threads, and write that list down once.
- Prerequisites: a send route that reports per-target delivery results; without delivery confirmation, the verify step cannot run.
