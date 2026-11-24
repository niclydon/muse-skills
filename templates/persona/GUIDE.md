# Persona Files: A Guide

Three small files that tell the assistant who it is, how to behave, and
how work gets done in your setup. They are the cheapest upgrade available
to any long-running assistant arrangement: instead of re-explaining
yourself in every chat, you write it down once, and every session starts
from the same understanding.

## What each file is for

- **IDENTITY.md** — the fixed facts. The assistant's name, pronouns,
  character, vibe, and signature. Facts change rarely, so they live in
  their own file where they can be stated once and stay stable.
- **SOUL.md** — the behavior. Voice, format habits, how recommendations
  are made, how much initiative to take, how mistakes and corrections are
  handled, and the hard boundaries. This is the file that shapes every
  reply, so it is the one worth the most care.
- **AGENTS.md** — the operating manual. Conventions for the workspace and
  the lessons learned from real work: tool quirks, environment facts, and
  rules you have set and do not want re-litigated. It is about the work,
  not about either of you.

The split matters. When behavior drifts, you edit SOUL.md. When a fact
about the setup changes, you edit IDENTITY.md. When a tool teaches you
something the hard way, it goes in AGENTS.md. One file trying to do all
three jobs becomes a document nobody can maintain.

## How Muse uses them

Muse loads these files as standing context at the start of a session, so
their contents are present before your first message. That is the whole
mechanism: anything in these files is something you never have to type
again, and anything absent is something the assistant will guess at, with
mixed results. Write them for the assistant that wakes up fresh, because
that is who reads them: plain statements, no assumed context, no
references to conversations it cannot see.

## How to start, and how they grow

Start small. A ten-line SOUL.md with your three strongest preferences
beats a blank file waiting for a perfect draft. The templates in this
folder are skeletons with guidance in brackets; fill in what you know,
delete the rest, and let the files grow by correction:

- When the assistant does something you dislike, ask whether a line in
  SOUL.md would have prevented it. If yes, add the line, stated as the
  rule it should have followed.
- When you make a durable decision about how work is done, record it in
  AGENTS.md with its date, so future sessions treat it as settled.
- When a lesson comes out of a failure, write it down the same day, while
  the details are still exact.

Review the files occasionally and prune. A rule that no longer matches
how you work is worse than no rule, because the assistant will follow it
faithfully.

## The one hard rule

These files describe the working relationship. They are never a place for
secrets. No credentials, tokens, passwords, or private keys go in them,
ever — standing context is read into every session and may be quoted back,
so treat everything in these files as readable by anyone the assistant
talks to. If a fact is sensitive, leave it out; the assistant can be told
in the moment, in the chat that needs it.
