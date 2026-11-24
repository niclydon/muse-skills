# muse-skills

Skills, patterns, and templates for [Muse](https://muse.ai), built by one user for a real, daily setup and generalized so yours can use them too.

Most of these exist because something went wrong once and the fix was worth writing down as a procedure instead of a hope: a background report that turned out to be invented, a chat that went stale for a month, a sign-in that failed three times because nobody checked how the site actually logs you in. If you run Muse as more than a chat window, you have probably met the same problems.

## The fastest way in

Point your own Muse at this repo and say:

> Read ADOPT.md and catalog.yaml in this repo, show me the menu, and install what I pick.

Your Muse will walk the catalog with you, group by group, note what each item needs, and install only what you choose, adapting each one to your setup as it goes. The recommended starter set (no connectors needed, useful in the first week) is: where-were-we, blockers, verify-background-results, check-the-record, do-it-yourself-first, and the persona file templates.

You can also install anything by hand: copy a folder from `skills/` into your `~/workspace/custom-skills/` and read the "Adapting this to your setup" section at the end of its SKILL.md.

## What's here

**Getting oriented** — [where-were-we](skills/where-were-we) (re-ground a stale chat), [whats-next](skills/whats-next) (the same for a code project), [blockers](skills/blockers) (prove each blocker real, then clear them one by one)

**Running the chat list** — [chat-tidy](skills/chat-tidy) (a family + status emoji naming system for chats, with an approval-gated archive pass), [baton-pass](skills/baton-pass) (hand a long chat to a fresh successor), [bounded-fanout](skills/bounded-fanout) (message many chats in small, verified batches)

**Shipping code** — [ship](skills/ship) (the whole pipeline as one instruction), [narrative-docs-update](skills/narrative-docs-update) (document the work inside its own repo), [adversarial-review](skills/adversarial-review) (reviewers who try to break the change before it merges), [worktree-janitor](skills/worktree-janitor), [project-retrospective](skills/project-retrospective), [repo-health-scan](skills/repo-health-scan), [upgrades-scan](skills/upgrades-scan), [mcp-api-parity](skills/mcp-api-parity)

**Checking your work** — [verify-background-results](skills/verify-background-results), [check-the-record](skills/check-the-record), [do-it-yourself-first](skills/do-it-yourself-first), [verify-signin-route](skills/verify-signin-route), [gui-input-verification](skills/gui-input-verification), [verification-shape-parity](skills/verification-shape-parity), [pipeline-coverage-freshness](skills/pipeline-coverage-freshness), [gold-source-coverage](skills/gold-source-coverage)

**Money and life admin** — [receipt-matcher](skills/receipt-matcher) (match statement charges to receipt emails; needs the Gmail and Plaid connectors), [weekly-review](skills/weekly-review), [trip-debrief](skills/trip-debrief)

**Patterns** (ways of working, written up rather than copied) — [journal and morning paper](patterns/journal-and-morning-paper) (your Muse keeps a twice-a-day journal and writes a one-page brief your other chats read at wake), [standing orders](patterns/standing-orders) (lessons become an approved list your Muse actually reads), [drafts gate](patterns/drafts-gate) (let your Muse draft public posts without ever holding the send token)

**Templates** — [persona files](templates/persona) (blank SOUL.md, IDENTITY.md, and AGENTS.md templates with a guide, the standing files that give a Muse a stable character and memory of how you work)

## Related

The [muse-mcp](https://github.com/niclydon/muse-mcp) repo has more Muse tooling, including a [voice targeting doc](https://github.com/niclydon/muse-mcp/blob/main/docs/voice-targeting.md) for gadget voice control.

## Notes on the writing

Everything here is plain language and self-contained: a skill should make sense to someone who has never seen the setup it came from. Where a skill builds on third-party work, the credit is kept in the file (the adversarial review skill, for example, carries material from [grim-prompter](https://github.com/jmanico/grim-prompter), MIT licensed).

## License

MIT. See [LICENSE](LICENSE). Use these, change them, ship your versions; a link back is appreciated.
