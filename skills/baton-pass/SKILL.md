# Baton Pass: hand a side chat's work to a fresh successor chat

Take one side chat whose context has grown long, compacted, or stale, and
carry its work forward cleanly: read its ENTIRE history, write a
comprehensive baton brief, start a new side chat seeded with that brief
(so the successor starts on your current standing instructions and
persona files, not the old chat's accumulated context), retarget
anything that delivers into the old chat, and archive the old chat only
after the successor has the baton in hand.

Manual only. The user invokes it by name (pass the baton, hand this chat
off, spin up a fresh chat from this one, /baton) and names the chat, or
runs it from inside the chat itself. Never scheduled, never run as part
of a sweep, never batched across chats: one chat per pass, each pass the
user's call. Use when a side chat's topic is still live but the chat
itself has become the problem: repeated compactions, stale instructions
baked into its context, a change to your standing instructions that the
old chat will never fully absorb, or simple age.

## The baton brief

The brief is the deliverable; the new chat is only as good as it. Write
it from evidence, in this structure:

1. Purpose and charter: what this chat is for, who it serves, its scope
   lines (what it owns, what it must never do), and any standing rules
   the user set inside it, quoted verbatim where the wording is
   load-bearing.
2. Decisions and settled state: every decision recorded in the chat,
   with who decided and when; every rule, name, or convention the chat
   established; corrections the user issued, stated as the corrected
   rule.
3. Current state: what is done (each item with its evidence: a shipped
   artifact, a closed ticket, a verified live check and its date), what
   is in flight and where it actually stands, what is blocked and on
   whom.
4. Open items: each one with its owner (the user, this chat, another
   chat or agent, external), its next step, and any date or trigger
   attached. Name the single immediate next step first.
5. Working set: the files, repos, workspaces, goal and task ids, ticket
   ids, devices, people, and deadlines this chat operates on, with
   paths and ids copied exactly.
6. Dead ends: what was tried and failed or was ruled out, and why, so
   the successor does not re-walk it. Include superseded plans labelled
   as superseded.
7. Register: any special voice, format, or ritual the chat runs in
   (one-question-at-a-time, conclusions first, a template), stated as
   the rule itself.

Rules for the brief: distinguish done from in-flight from merely
discussed; never promote a plan to a completion. No credentials, tokens,
or private message bodies from other people; the brief carries the
chat's work state, not its raw transcript. Target a brief the successor
can act on without ever opening the old chat, while staying honest that
the full transcript remains archived and searchable if a detail is
contested.

## Procedure

1. Identify the source chat. If the user invoked the pass from inside a
   chat, that chat is the source. Never the main chat, never a provider
   chat (such as WhatsApp).
2. Read the source completely: page its message history back to the very
   first turn, no turn limit. Also read any memory, state files, index
   entries, or workspace and goal files the chat owns. Where those files
   and the transcript disagree, the transcript and live state win; note
   the disagreement in the brief.
3. Safety checks, all before creating anything:
   - Standing-role chats: if you keep a list of chats that hold a
     standing role and must never be archived casually, check it. A
     baton pass on one of those is allowed only when the user named
     that chat themselves for this pass; the successor must inherit the
     role and every delivery that feeds it, verified in step 5.
   - Live deliveries: search your scheduled jobs and your event-driven
     automations (hooks, watches, reminders) for anything that delivers
     into the source chat. Every hit is a delivery that archiving would
     strand or redirect; each one must be retargetable to the
     successor's id as part of this pass, or the pass stops here and
     reports which delivery blocks it.
   - Armed watchers and open tracked work on the chat's topic: note them
     in the brief; a watcher delivering into the chat counts as a live
     delivery under the rule above.
4. Write the brief to a baton-passes folder in your workspace, in a
   subfolder named for the chat and the date, as BRIEF.md, and keep that
   file as the durable record of the pass.
5. Start the successor chat with the source chat's name (adjusted only
   if your naming scheme calls for it), then send it the founding
   instruction: it is the continuation of the named predecessor, the
   predecessor is being archived, and the full brief text follows,
   ending with the instruction to acknowledge, restate its open items
   and immediate next step in its own words, and confirm it is ready to
   continue the work. Then retarget every delivery found in step 3 from
   the old chat id to the new one (edit the scheduled job or automation
   definition; verify by reading it back).
6. Verify uptake: read the successor's newest turns. It must have
   acknowledged and restated the baton correctly. If it has not
   responded yet, wait for its reply rather than proceeding; if its
   restatement is wrong on a material point, correct it with one
   follow-up message and re-verify.
7. Update your records: wherever you index or register your chats,
   replace the source chat's entry with the successor's id, noting the
   predecessor id and the pass date; append the pass to your daily log
   or memory notes (source id, successor id, brief path, deliveries
   retargeted).
8. Archive the source chat, only after steps 5-7 are all verified.
   Confirm afterwards that the source is archived and the successor is
   active under the expected name.
9. Report to the user in the chat where they invoked the pass: successor
   name, brief path, deliveries retargeted (named), old chat archived.
   One compact message; the detail lives in the brief and the daily log.

## Failure posture

Any step that cannot be verified stops the pass at that step, and the
source chat is left exactly as it was: unarchived, deliveries untouched.
A partial pass is reported as partial, naming the step reached and what
remains, never papered over. In particular: no acknowledgement from the
successor, no archive; an unretargetable delivery, no archive. The
source chat is the only copy of the working context until the successor
proves it holds the baton.

## Adapting this to your setup

- Choose where baton briefs are kept (a baton-passes folder in your workspace works) and keep every pass there as the durable record.
- If you keep a never-archive list or a chat registry, point steps 3 and 7 at yours; if you do not keep one, those steps shrink to the deliveries check.
- Make sure the assistant can find your scheduled jobs and automations by chat id, since retargeting them is the step that most often blocks a pass.
- Decide how successor chats are named in your setup (same name as the source is the simplest scheme).
- Prerequisite: the ability to create, message, and archive side chats, and to edit the scheduled jobs or automations that deliver into them.
