# Blockers

## The prompt this skill runs

- Run this prompt in the chat it is posted in: "Name every blocker in this thread and prove each one is real against current state. For each one: what it stops, whose move unblocks it, what has already been tried, and the way through. Then work them one by one in dependency order: clear the ones clearable under your existing authority, and bring me the ones that need my decision singly, evidence first and my question last."

- Know the difference from `where-were-we`: that skill orients and reports what is open. This skill works. It ends with blockers cleared or precisely held, not with an inventory.

## Find the real blockers

- Read the thread back to its start and extract every claimed blocker: the dependency that never landed, the credential that died, the decision never given, the path that failed, the dispatch that stalled.

- Verify each claim against current state before treating it as a blocker: check the dependency's live status, the credential, the service, the queue. Stale threads carry blockers that already dissolved, and working one of those wastes the session.

- Report dissolved blockers as dissolved, with the evidence, and drop them.

- Reclassify what is not a blocker. Work the assistant simply has not done yet is a task, so do it or route it rather than listing it. An item the user parked by their own ruling is a decision that stands, not a blocker, unless they ask to revisit it. A slow process is not a blocker unless something is actually stopped behind it.

## Build the ledger

- State each surviving blocker compactly: what work is stopped behind it, its kind (decision, credential or access, external dependency, technical failure, capacity), whose move unblocks it, what has already been tried, and the proposed way through with its tradeoff.

- Order the ledger by gating: a blocker that holds other blockers goes first, then by how much work each one releases when cleared.

## Work them one by one

- Start at the top of the ledger and finish each blocker before opening the next. One by one means one at a time, never a bundle of decisions presented together.

- When a blocker is clearable under existing authority, clear it in the moment, verify the clearance live, report the outcome in one or two lines, and move to the next.

- When a blocker needs the user's decision, present that one alone: the evidence first, then the question at the end with tap options where the chat supports them. Resolve it from their answer and move on.

- When a blocker is genuinely external, file or track it properly, name what will notice its change, and move to the next blocker that can move, so one external wait does not hold the whole session hostage.

- Re-check the ledger after each clearance. Clearing one blocker often dissolves the next or changes its shape, and the order should follow the new reality, not the original list.

## Stop conditions and guardrails

- Stop when every blocker is cleared, and close by naming the work that can now proceed. Or stop when the remainder are all genuinely external or waiting on the user, and name what each one waits on.

- Never route around a gate to clear a blocker. An approval gate is not an obstacle to bypass; it is a decision to bring to the user.

- Never manufacture a blocker from speculation. A blocker exists only when named work is demonstrably stopped behind a named cause.

## Adapting this to your setup

- Define what "existing authority" means for your assistant: which fixes it may apply on its own and which actions always need your approval (for example anything destructive, financial, or outward-facing).
- Point the skill at wherever external blockers should be tracked in your setup (an issue tracker, a task list, a note) so a genuinely external wait gets filed instead of just mentioned.
- If your chat surface does not support tap options, present each decision question as a short numbered list instead.
- No prerequisites; the skill works on any thread with claimed blockers in it.
