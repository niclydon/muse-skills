# Worktree Janitor

Prune git worktrees whose work is safely merged into the default branch, and report what was reclaimed. The janitor removes only what it can prove is safe; anything uncertain stays.

## What you need

- A helper script that lists the worktrees of the target repos, classifies each one, and removes the proven ones. Write a small script of your own or adapt one: it needs a `--dry-run` mode that only classifies, and an apply mode that removes. The proof rules below are the contract the script must enforce.
- A protect list: the names of worktrees that must never be removed because an active work session is using them. Build it from whatever tracks your active sessions or claims (a shared task list, a claims file, a lock directory), and check it before touching anything. If you have no such tracking, protect at least any worktree modified in the last day and any worktree you cannot account for.

## Build the protect list

- Before touching anything, work out which worktrees are in use right now: check for active work sessions or claims on each worktree, and add every claimed name to the protect list.

- Treat an empty protect list as an empty protect list and proceed, because a missing protect list is not a reason to stop a routine cleanup. When in doubt about a specific worktree, protect that one.

## Classify and prune in one run

- Run the helper script with `--dry-run` first in the same run to classify every worktree in the target repos, which default to the repos you name in your setup, with extra repos passed as arguments for a one-off wider sweep.

- Remove a worktree in the same run with the apply mode when every proof below holds, with no separate approval step, because routine reversible cleanup inside these bounds is pre-authorized.

- Require the worktree to be clean, off the protect list, outside the main checkout, and outside any tool-managed worktree path (such as an agent harness's own worktree directory) before removal.

- Prove the work is merged with a merged pull request for its branch or with its HEAD as an ancestor of the default branch, and check the pull request proof first because squash merges do not leave branch commits as ancestors of the default branch.

- Keep every worktree whose merge status is uncertain and flag it in the report, because an uncertain proof is a keep, never a removal.

- Keep dirty worktrees untouched and name them in the report with their paths, because hand inspection of uncommitted work belongs to a person or a later decided pass, not to the janitor.

- Remove each proven worktree with `git worktree remove --force` through the script, prune its merged branch, and finish with `git worktree prune`.

- Never commit, push, or merge from this skill, because it only removes proven-safe leftovers.

## Report the result

- Report the number pruned with the merge proof for each one, the number kept with the reason for each keep, and the megabytes of disk reclaimed.

- Escalate only when a removal proof fails in a new way, when the script errors, or when kept dirty or unmerged worktrees are blocking disk recovery, and name that blocker plainly in the report.

## Adapting this to your setup

- Write the helper script for your own environment: it can be a short shell script around `git worktree list`, `git status`, and your pull request tooling. Keep the dry-run-first habit.
- Set your default repo list to the checkouts that actually accumulate worktrees on your machine, and pass extra repos as arguments for one-off sweeps instead of changing the defaults.
- Decide how your protect list is built: a claims file, a session tracker, or simply "anything touched recently." The rule that matters is that an in-use worktree is never removed.
- Run the janitor as part of a disk-pressure response before investigating other consumers, because merged worktree leftovers are usually the cheapest disk to reclaim.
- If your team reviews deletions, keep the dry-run report as the review artifact and run the apply step only after a person has seen it.
