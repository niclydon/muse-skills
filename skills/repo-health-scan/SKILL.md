# Repo Health Scan

## Purpose

One job: run a maintenance pass over your repos and turn hygiene findings into well-scoped issues in your tracker. Diagnose and file; never fix. The fixes happen later, as their own pieces of work, once someone has decided they are worth doing.

This skill is read-only on the repos. It changes nothing in any checkout.

## Workflow

1. **Scope the pass.** Default target is every git repo in the projects folder you name in your setup (check each candidate with `git -C <path> rev-parse --is-inside-work-tree`). Exclude read-only registries and asset stores. Accept a narrower scope (one repo, a list) when the caller gives one.

2. **Apply the cooldown.** Skip any repo scanned in the last 168 hours (7 days). Track last-scan state in the scan report, not in a separate state file, unless you need to resume across restarts.

3. **Gather signals** (deterministic first; no judgment calls on detection):
   - Forgotten `TODO` / `FIXME` / `XXX` / `HACK` in source, older than about 30 days per `git blame`. Skip lines already tracked with a marker like `(#123)`.
   - Stale plan, project, and narrative docs (`docs/plans/`, `docs/projects/`, `docs/narrative/`, `PLAN.md`, `ROADMAP.md`, `TODO.md`): older than 60 days (modification time or last commit, whichever is newer), not marked done, shipped, closed, or complete, and containing a trigger phrase ("deferred", "follow-up", "didn't ship", "future work", "TBD", "out of scope") with a concrete next action that was never taken.
   - `CHANGES.md` follow-ups: the same trigger phrases over the most recent ~50 entries.
   - Buried bug patterns (high-precision only): silently swallowed `catch` blocks, `@ts-ignore` without a tracking reference, stray `console.log` in non-test source, linter-disable comments without an explanation, hardcoded `localhost:<port>` outside config files. If a pattern exceeds 20 hits in one repo, file one summary finding for that repo and pattern instead of individual ones.
   - Orphaned schema, config, and handlers: migrations referencing tables absent from current source, job handlers missing from the routing table (and vice versa). These are heuristic; file them as needs-investigation, not as fixes.

4. **Dedup before filing.** For each signal, search your existing issues for the same repo, file path, and finding. The dedup key is (repo, file path, line). Re-scans must not re-file what is already tracked.

5. **File one issue per finding** (or one summary issue per bulk pattern) in GitHub Issues or your own tracker. Each issue carries:
   - A title leading with the repo and the category, such as "myrepo: forgotten TODO in handlers/foo.ts:42".
   - A problem paragraph: what was found, the exact file and line, the `git blame` age and commit subject if revealing, and why it matters here.
   - A solution paragraph: a concrete suggested fix, or the next investigation step when the diagnosis is still open.
   - A category label from a small fixed set: forgotten-todo, stale-plan, changes-followup, buried-bug, orphan.
   - A priority: low by default; higher only for likely real bugs in production code paths.
   - A readiness tag: mark an issue safe to hand off without further decisions only when the fix is fully specified, reversible, and testable, and needs no decision only you can make. Mark it needs-investigation when the diagnosis is still open. Every issue gets one of the two.

6. **Report.** A short per-repo summary: repos swept, findings filed (with issue links or numbers), duplicates skipped, and repos skipped on cooldown.

## Output contract

- One issue per distinct finding, or one summary issue per bulk pattern.
- A short per-repo sweep summary (swept / filed / duplicates / skipped on cooldown).
- Nothing else: no code edits, no commits, no pushes, no pull requests. The pass is read-only on the repos.

## Operating rules

1. Read-only on the repos: searches, `git blame`, `git log`, and file reads only. If a check needs more than read access, file the finding and move on.
2. A "safe to hand off" tag is a classification, not a safety grant. Tag an issue that way only when the next step is mechanical and verified by the repo's own tests or build. Uncertainty about how to implement a reversible, testable maintenance fix is not a decision for the user; a genuine product, policy, money, authority, or destructive-data choice is.
3. For multi-repo sweeps, work one repo at a time or split repos across parallel helpers instead of running long sequential searches; keep the results in the same report either way.
4. Keep the signal catalog, the cooldown, and the dedup key stable between runs. Changing them every pass is what turns a maintenance sweep into a source of duplicate issues.

## Adapting this to your setup

- Name your projects folder and your exclusions (asset stores, vendored code, read-only mirrors) before the first run, and keep that scope in the skill so every pass sweeps the same ground.
- Point filing at your own tracker: GitHub Issues works well; any tracker with searchable titles and labels can carry the dedup and category scheme.
- Tune the signal thresholds to your repos: the TODO age, the stale-doc age, and the bulk-pattern cap of 20 are starting points, not laws.
- Set the cooldown to match how often you actually sweep. Seven days suits a weekly pass; a monthly pass can drop it.
- Decide who picks up the filed issues and how. This skill only diagnoses and files; pairing it with a regular triage habit is what keeps the list from becoming its own kind of clutter.
