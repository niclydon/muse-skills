# Verify background and delegated results

## Sort the claims in the report

- Split every background task handoff, delegated agent report and subagent completion into its separate claims before using any of them.

- Mark each claim as an aggregate or count, a state claim such as merged, deployed, closed or live, or a completion claim that work finished end to end.

- Treat every marked claim as unverified until a foreground check in this session reproduces it.

## Spot check aggregates before any write

- Reproduce an aggregate or count with a fast foreground check, such as a set-based join or a small per-id read, before repeating the number or basing any write on it.

- Run a pilot on a small slice before a batch write whenever the batch rests on a background-delivered aggregate.

- Stop the batch when the pilot or spot check cannot reproduce the aggregate, and re-derive the true number from the live source before doing anything else.

## Verify state and completion against the source of truth

- Check a merged, deployed or closed claim against the live system that owns it, such as the GitHub state of the pull request, the deployed build stamp and health response, or the current database row.

- Require one line of production evidence that the new path was actually invoked before calling a change shipped or a fix live.

- Never let the report itself, an earlier stale result, or a summary of the report stand in for that live check.

## Report what the check found

- Say plainly when a foreground check contradicts a background report, discard the contradicted claim, and state the verified value with its source.

- Label a claim unverified and name the check that could not run when no live check is possible, instead of repeating the claim as fact.

- Carry only the verified claims forward into decisions, writes and anything said to the user.

## Alternatives for better results

- Shrink the first check to the cheapest read that can contradict the claim, such as one id, one row or one status call, and widen only when it agrees.

- Prefer a second independent source for high-stakes claims, such as a deploy stamp plus a health response, over a deeper read of one source.

## Adapting this to your setup

- Name the source of truth for each kind of claim you receive often (code host for merges, deploy system for releases, database or admin UI for record state) so the check is a lookup, not a search.
- Decide which claim types always need a foreground check in your environment; counts and completion claims are the two that fail most often.
- Works with any delegation pattern: background shell tasks, subagents, or another assistant's handoff report.
- No special tools are required beyond read access to the live systems your background work reports on.
