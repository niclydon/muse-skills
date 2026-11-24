# What's Next

Project status briefing on demand. This replaces the 5 to 10 minute session-start discovery loop of re-reading a project to work out where things stand.

This is the repo companion to the where-were-we skill: that skill re-grounds a stale conversation, and this one re-grounds a code project from its plan docs, commits, and TODOs.

## Purpose

Produce a tight "where we left off / single recommended next step" briefing for one project repo. Do the discovery; do not narrate it.

## Workflow

1. **Identify the project.** If the user named it, use it. Otherwise ask which project. Work wherever the repo lives: on this machine, or on another host you can reach. Resolve the repo's actual location before reading anything.

2. **Read the recent changelog.** If the repo keeps a `CHANGES.md` or similar changelog, its most recent entry is the "where we left off" anchor. Note the date and the last completed items.

3. **Read plan docs**, in priority order:
   - `docs/plans/*.md`, `docs/roadmap.md`, `BACKLOG.md`, `TODO.md`
   - Any project policy or conventions doc the repo points to from its README or plans
   - Architecture decision records, if the repo keeps them (often in `docs/adrs/` or similar)

4. **Cross-check git state.**
   - `git log --oneline -20`: what has actually been committed since the last changelog entry.
   - `git status`: uncommitted work in progress.
   - `git stash list`: anything parked.

5. **Cross-check active work.** Look for background jobs, scheduled tasks, or deployed services relevant to in-flight work, if the project has them. Check recent error logs only if the project owns a deployed service.

6. **Look for inline markers.** Search the code for `TODO`, `FIXME`, and `XXX` comments, excluding dependency and virtual-environment folders. Flag only ones added recently or tied to the active plan.

Skip this skill if the user has already named a specific task. Just do that task.

## Output Contract

```
## Where we left off
<1-3 sentences anchored to the most recent changelog entry + last commit>

## Open items (priority order)
1. <item> — <why it's next>
2. <item> — <why>
3. <item>

## Blockers / context
<anything that would derail the next step>

## Recommended next step
<ONE concrete action, not a menu>
```

If nothing is actually open, say so plainly: "Nothing pending here. Last work: <summary>."

## Operating Rules

- **One recommendation, not a menu.** Scope expansion is a known problem here.
- Do not pad the briefing with adjacent candidates the user did not ask about.
- Do not apologize for the brief being short. Short is correct here.
- Do not recommend work that `git status` shows as already in progress without flagging that it is mid-flight.
- Do not list every TODO in the codebase, only ones tied to the active plan.
- If multiple plans conflict, name the conflict and ask one targeted question.
- If your saved memory has stale facts about this project, prefer the current code and git state, and update the memory.

## Adapting this to your setup

- List your main project repos and where each one lives (this machine, a server, a specific folder) so the assistant goes straight to the right place.
- If your repos use different names for the changelog or plan docs, name them here so they are read first.
- If a project tracks work in an issue tracker as well as in plan docs, name the tracker so open issues are cross-checked against the docs.
- Keep the one-recommendation rule unless you genuinely prefer a short menu; the single next step is the point of the briefing.
