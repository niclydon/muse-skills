# Where Were We

## The question this skill answers

- Answer this prompt, in whatever chat it is posted in: "Re-ground this thread from current state, not from its own last word. What was it for, what is already done, what is still open and who owns each item, what is blocked and on what, and what is the single next step?"

- Use this skill for a conversation thread gone stale. For a repo's plan docs, commits, and TODOs, use a project-status skill instead.

## Short form

- Saying /www in a chat runs this same skill in compressed form: read the thread back to its start, verify every candidate open item against current state before reporting it, and reply with the conclusion first, the open items compactly with an owner each, and one recommended next step at the end. Same method, shorter output.

## Reconstruct the thread

- Read this chat's history back to its start, paging past the recent turns and past any compaction summary when the early context matters. Compaction summaries omit detail, so recover missing decisions from the original messages before concluding anything.

- Identify the thread's purpose, the decisions recorded in it, what was delivered, and every commitment left hanging: questions put to the user, approvals pending, work dispatched, follow-ups the assistant owed, and items parked by the user's own ruling.

## Verify against current state

- Treat the thread's own account of what is open as a claim, not a fact. A stale thread predates work that may have finished elsewhere.

- Verify each candidate open item against current state before reporting it: search memory, check the live source the item lives in, such as the ticket, the repo, the calendar, or the service itself, and check sibling chats when the work may have moved there.

- Report an item the thread left open but another venue closed as closed, naming where it closed. Flag any claim in the thread that current state contradicts.

- Label anything you could not verify as unverified rather than presenting it as open or closed.

## Answer

- Lead with the conclusion: how many items are genuinely open, and the one that matters most.

- List each open item compactly with its owner: waiting on the user for a decision, input, or physical action, in flight with the assistant, blocked on a named external dependency, or parked by the user's ruling with the ruling named.

- End with a single recommended next step, the highest-leverage move from the open items, stated as a recommendation with its tradeoff rather than a menu. When that step needs the user's choice, put the question last with tap options.

- When verification shows nothing genuinely open, say the thread is complete in one line, name the evidence behind that, and note the chat is safe to archive. Never declare a thread complete from its own text alone.

## Guardrails

- This skill reports and recommends. It does not restart work, message other chats, or act on any item it surfaces unless the user directs that in reply.

- Do not re-ask the user a question the thread already answered or that current state settles. The point of the skill is recovering ground, not spending their attention twice.

## Adapting this to your setup

- Decide which live sources count as current state in your environment (your issue tracker, repos, calendar, connected services) and name them for the assistant if they are not obvious.
- If you use side chats or multiple threads per project, tell the assistant where sibling threads live so it can check whether work moved there.
- Adjust the owner labels to your situation (for example, just "you" and "the assistant" if you work alone).
- No prerequisites beyond a chat with enough history to be worth re-grounding.
