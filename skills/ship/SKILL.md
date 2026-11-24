# Ship It

## What shipping means here

- Treat "ship it" as one instruction covering the whole pipeline: update the narrative docs, commit, squash-merge, push, deploy, clean up old worktrees and branches, and leave the checkout current on main.

- Treat the invocation itself as the user's explicit approval for the commit, merge, push, and deploy of the work in scope. It grants no standing approval beyond that invocation.

- Ship only the work in scope: the repo and change set the conversation is about. Never sweep unrelated dirty changes, other repos, or someone else's worktree into a ship.

- Stop at the first failing step and report exactly where it stopped, leaving the tree in a recoverable state. Never force-push, never bypass a pre-commit hook, and never skip a failing check to reach green.

## Resolve the scope

- Identify the repo from the current conversation and the work just done. When two repos are plausibly in scope, ask once before touching either.

- Work where the repo lives: on this machine, or on the host where the repo lives if your repos are kept on another machine.

- Run a preflight in the repo: `git status`, current branch, `git worktree list`, unpushed commits, and how far main sits behind origin/main.

- Confirm the dirty set matches the intended change set. When it contains anything unexplained, stop and report the extra files instead of committing them.

## Document the arc before committing

- Run the `narrative-docs-update` skill for the work being shipped before anything is committed, so the record lands in the same commit and pull request as the code.

- Write the narrative doc into the shipped repo's own `docs/narrative/` folder, never anywhere else, so the record travels with the code it describes.

- Update the repo's CHANGES.md and any plan docs the work changed as part of the same pass.

- Skip the narrative doc only for one-line fixes. A CHANGES.md entry still applies when the repo keeps one for changes of that size.

## Commit and merge

- Follow the repo's own landing pattern. Where main is protected, work in an isolated worktree branched from origin/main, never commit on main in the main checkout, and land through a pull request.

- For repos whose established pattern is direct commits to main, such as small personal repos, commit on main directly and skip the pull request.

- Include the narrative doc, CHANGES.md entry, and plan-doc updates from the documentation step in the same commit or pull request as the code they describe.

- Run the repo's own checks before merging: its test suite, typecheck, or lint, whichever it defines. A red check stops the ship.

- Run an adversarial review before merging when the change touches a production gateway or service, a database migration, auth or secrets, a public surface, or the deploy path, following the `adversarial-review` skill in this repo. The user can waive it in the invocation.

- Squash-merge the pull request with `gh pr merge --squash` on the host where the repo lives. Keep the commit subject naming the work, and include any issue or ticket numbers when the work came from a tracker.

## Push and deploy

- Push after the merge and confirm origin/main carries the merge commit with `git ls-remote` or a fetch and compare.

- Deploy through the repo's official deploy path only, such as its `scripts/deploy.sh`, its documented deploy command, or its service restart. Never improvise a deploy.

- If deploys in your setup are serialized behind a shared lock, a silent deploy may be queued behind another one, so check what holds the lock before calling a deploy stalled.

- Verify the deploy live with one line of production evidence: a health endpoint response, a build-info or version read, a running-process check, or a smoke call against the new path. Merged and pushed is not shipped until that evidence exists.

- Report a failed or rolled-back deploy as not shipped, with the failing step named.

## Clean up and get current on main

- Remove the worktree the shipped work ran in and delete its branch locally and on the remote once the merge is confirmed.

- Sweep older worktrees and branches with the `worktree-janitor` skill in this repo: prune only worktrees whose work is provably in main, and never touch a worktree that is in use, dirty, or claimed by an active work session.

- Delete only branches fully merged into main, locally and on the remote, and prune stale remote-tracking refs with `git fetch --prune`.

- Finish in the main checkout: check out main, fast-forward to origin/main, and verify `git status` is clean and HEAD matches origin/main.

## Report

- Report one compact block per repo: what was committed, the pull request and merge commit, the deploy evidence line, what cleanup removed, and the final main state with its short hash.

- Name anything left behind plainly, such as an unmerged branch kept on purpose or a worktree skipped because it was dirty or in use.

## Adapting this to your setup

- Decide which of your repos land through pull requests and which commit directly to main, and note any exceptions next to the repo list you work from.
- Point the deploy step at each repo's real deploy path: a script, a documented command, or a service restart. If a repo has no deploy step, end the pipeline at the merge and say so in the report.
- Choose your production evidence in advance for each service you ship: a health endpoint, a version read, or a smoke call. If none exists yet, adding one is worth more than the rest of this pipeline.
- If you do not keep narrative docs or a CHANGES.md, drop the documentation step or shrink it to a short changelog entry, but keep some written record in the same commit as the code.
- Set your own bar for when the adversarial review is required. The list above is a starting point; match it to the changes that could actually hurt you.
