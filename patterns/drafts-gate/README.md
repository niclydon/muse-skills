# Pattern: The Drafts Gate

How to let an assistant take part in public rooms — a Discord server, a
forum, a social account — without ever giving the assistant the ability to
post.

This is a design write-up, not code. The working implementation this
pattern was distilled from is a Discord bot built as a third-party open
project; the code is that project's to share, under its own license. What
follows is the architecture, which is platform-independent and yours to
implement.

## The split

Two processes, with different powers:

- **The monitor** is a small, dumb, always-on process. It is the only
  thing that holds the platform token. It watches the rooms, collects the
  items that need a reply, hands them to the assistant, and sends whatever
  comes back through the gate described below. It does no writing of its
  own.
- **The assistant** is the smart part, woken on demand. It receives a
  batch of items, drafts a reply to each, and writes the drafts to an
  **outbox** — a file or queue the monitor reads. The assistant never
  holds the token, never calls the platform's send API, and cannot post
  anything itself, no matter what it decides to write.

The assistant can only reply to items it was handed, in the channel those
items came from. It cannot start a thread in a room nobody asked about,
cannot wander into a new channel, and cannot reply twice to the same item.
Each draft carries a reference to the one item it answers, and the monitor
enforces the match.

## The gate

Not every draft goes straight out. The monitor checks each outgoing draft
against a hold list — phrases or topics the owner wants to see first.
A draft that matches is diverted: instead of posting, the monitor sends
the draft and its destination to the owner privately, with two choices:

- **Send anyway** — the draft posts as the original reply.
- **Discard** — the draft is dropped and the item is settled as skipped.

A hold nobody answers within 24 hours is discarded. Silence never becomes
a post. Drafts that match nothing on the hold list send normally, so the
gate adds review exactly where the owner asked for it and nowhere else.

## Why the split matters

An assistant that writes public posts can go wrong in ordinary ways: it
can misread a room, be talked into something by a stranger's message, or
confidently say a thing that is not true. If that same process also holds
the send credential, every one of those failures is a published post, and
the only defense is hoping the writing stays good.

With the split, the writing process can be compromised, manipulated, or
simply wrong, and it still cannot publish a word. The worst it can do is
put a bad draft in an outbox nobody is obliged to send. Send capability
lives in a process too simple to be talked into anything: it holds no
opinions, follows a short list of mechanical rules, and its entire
judgment is "does this draft match the hold list, and was it drafted in
reply to an item I handed out?" That is a security boundary, not a style
preference, and it also makes the system auditable: every post that ever
went out passed through one small program that can log all of them.

A side benefit: the hold list becomes a precise record of the owner's
actual sensitivities, learned from real near-misses, instead of a long
prompt asking the assistant to please be careful about everything.

## Adapting it to other platforms

The pattern needs only three things from a platform: a way to read new
items, a way to post, and a private channel to the owner. It ports
directly:

- **Slack or Discord bots:** the monitor is your bot process with the bot
  token; the outbox can be a local file or a small queue table; holds are
  delivered as a direct message with button or reaction choices.
- **Forums and comment sections:** the monitor polls for new comments or
  uses a webhook; drafts wait in the outbox until the gate clears them;
  the posting step uses the account credential, which lives only in the
  monitor.
- **Social posting:** the assistant drafts posts and threads into the
  outbox on a schedule; the monitor is the only process with the posting
  credential. A common variation here is to hold everything by default
  and let the owner release posts in a batch, since there is no incoming
  item each draft is answering.

Two implementation notes:

- **Keep the monitor genuinely dumb.** Every feature added to it (smart
  filtering, auto-replies, formatting intelligence) erodes the boundary.
  Intelligence belongs on the drafting side, where it cannot send.
- **Log at the monitor.** Because every send passes through it, its log
  is the complete record of what was published, when, and in reply to
  what. Keep it.
