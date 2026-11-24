# Chat Tidy: an emoji naming system for side chats

Rename every side chat so its name answers two questions at a glance: what
family does it belong to, and does it need anything right now? Use when the
user says tidy the chats, and as a separate archive pass when they ask
which chats can be archived.

## The naming format

`<family emoji> <core name> <status emoji>`

For example: `🧳 Lisbon trip ⏳`, `💰 Tax paperwork 🙋`.

- Exactly one family emoji, at the start. At most one status emoji, at the end.
- The core name is the chat's plain human name; keep it stable between sweeps.
- Strip emoji from an earlier sweep before recomputing, so a second sweep
  changes nothing.
- Never rename the main chat. Renaming is safe mid-work. Keep names under
  120 characters.

## Family emoji

The family is a category the user defines for their own chats: one emoji
per family, in a short list the sweep works from. Examples:

- 💼 Work projects
- 👨‍👩‍👧 Family
- 🧳 Travel
- 💰 Money
- 🏃 Health
- 🎨 Creative
- 🗂️ Admin
- 🔎 Research

Keep a record of which chat belongs to which family. A chat that fits no
family gets the closest one, noted in the report; propose new families
rather than inventing them silently.

## Status emoji

At most one, and only when the evidence supports it:

- 🙋 Needs you: a question, decision, or approval is waiting on the user,
  or the next step is a physical action only they can do.
- ⏳ Waiting: blocked on something that is not the user: another person, a
  background job, a timer, a future date. Nothing for the user to do here.
- ✅ Done: the purpose is complete and verified, nothing open. The sweep
  marks done chats; it never archives them.
- 💤 Parked: the user explicitly set the topic aside.
- No status emoji: work is in flight, or the chat is ordinarily idle.

Precedence: 🙋 over ⏳ over ✅ or 💤. When the evidence is ambiguous, use no
status rather than a wrong one: a false 🙋 is worse than a missing one,
because 🙋 means "your move."

## The sweep

1. List the chats. Skip the main chat and any already archived.
2. Read each chat's newest few turns and classify its status from that
   evidence, not from its age.
3. Compute the correct name; rename only when it differs from the current one.
4. Record any chat not seen before in your family record.
5. Report: how many were renamed, the full 🙋 list (the user's open moves),
   and any new family proposed. If nothing changed, there is nothing to report.

Between sweeps, a chat can update its own status when its state changes;
the next sweep corrects any mark the newest evidence disagrees with.

## The archive pass

A separate mode, run only when the user asks. It never archives anything on
its own.

1. Candidates: chats marked ✅, plus long-quiet chats whose purpose looks
   complete. The ✅ mark alone is not enough.
2. Verify completion from evidence: read the newest turns, and for short
   chats read back to the start. Confirm the deliverable landed, nothing is
   open, and no question is unanswered.
3. Safety checks: hold a chat off the list, with the reason reported, if it
   holds a standing role (a recurring job, regular delivery, or ongoing
   responsibility arriving in that chat), if scheduled results still arrive
   there, or if work on its topic is still tracked. Never archive one.
4. Present one batch: each recommended chat with one line of completion
   evidence, plus the held list with reasons. End by asking for approval.
5. Archive only the chats the user approves, exactly those; a partial
   approval archives only the named subset. Archiving is reversible.

## Adapting this to your setup

- Define your own families and emoji before the first sweep; 6 to 10
  families cover most people.
- Keep the family record somewhere your assistant can read and update
  during a sweep.
- List standing-role chats as never-archive before any archive pass.
- If your chat tool cannot rename chats, keep the same system as a written
  index: one line per chat, kept current by the sweep.
