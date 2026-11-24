# Upgrades Scan

## The prompt

- Run this prompt, with the surface filled in: "Run an upgrades scan of [surface]. Read-only: change nothing. First establish the current state from live evidence, and verify any self-report against an independent source before treating it as a finding. Then inventory the opportunities in four classes: updates, fixes, upgrades, and enhancements. Dedupe every candidate against work already tracked or already decided. Rank what survives into one list, each item with its evidence, its class, its value, its effort and risk, and a recommendation. End with the items you would do first and why. File nothing and build nothing until I dispose the list."

## Scope the scan

- Name the surface precisely before starting: a repo, a service, a set of hosts, a database, a set of tools, a chat surface. When the ask names no surface, ask once rather than guessing.

- Bound the scan to that surface. Adjacent findings get one line in the report as leads, never a silent scope expansion.

## Establish ground truth first

- Read the live state of the surface before forming any candidate: versions, configs, queues, logs, and inventories as they are now, not as any doc or prior scan left them.

- Verify every self-report against independent evidence before it becomes a finding. A job's own log line, a service's own health claim, and a dashboard's green are claims, not proof. A real example of the trap: two discovery passes once carried a component's own error text as ground truth and reported a misplacement that never existed.

- Treat aggregate numbers from background runs as unverified until a fast foreground spot-check reproduces them.

- Label anything that could not be verified as unverified, in the item itself, never buried.

## Inventory the four classes

- Updates: versions behind current, end-of-life runtimes, pending packages, stale lockfiles, and anything whose upgrade path is unowned or unmonitored.

- Fixes: defects, drift between desired and actual state, dead weight with no owner or activity, broken or misleading signals, and hygiene debt such as forgotten TODOs and stale plan docs.

- Upgrades: capability gaps measured against the upstream project, the reference implementation, or the official API: endpoints, features, and tools that exist upstream and are not yet adopted locally.

- Enhancements: quality-of-life and usability improvements, including improvements that help agents use the surface well. A good inventory often earns a "build all of these" on the items that prove out, so do not self-censor plausible enhancements for taste.

## Dedupe before ranking

- Check every candidate against your existing backlog or issue tracker, prior scans of the same surface, and decisions you have already recorded. Work already tracked is referenced, not refound.

- Never resurface an item you already rejected or parked unless new evidence changes its case, and say what the new evidence is when you do.

## Rank and present

- Deliver one ranked list. Each item carries: the opportunity in one line, its class, the evidence behind it, the value if done, the effort and risk, and a recommendation.

- Save the full scan as a dated report file in the workspace area for that surface, and present the ranked list in chat as a disposition list the user can rule on item by item.

- Name the top items you would do first and why. A flat inventory with no recommendation is a non-answer.

## Guardrails

- The scan is read-only. It changes nothing, files nothing, and builds nothing. Filing backlog items and starting work happen only after the user disposes the list, under the usual approval gates.

- Discovery outpacing repair is a known and accepted dynamic. The scan's job is a trustworthy inventory, not a smaller one.

## Adapting this to your setup

- Name the surfaces you scan most (repos, services, a database, your tool set) if you want the assistant to suggest one instead of asking.
- Point the dedupe step at your actual backlog or issue tracker and wherever prior scan reports are saved.
- Choose where dated scan reports should be written (a folder per surface works well).
- Set your own ranking weights if they differ: value first is the default here; you may prefer lowest risk or lowest effort first.
- No prerequisites beyond read access to the surface being scanned.
