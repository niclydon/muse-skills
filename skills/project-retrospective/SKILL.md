# Project Retrospective

## Purpose
Close out a finished project by fusing every evidence surface into one durable record: what shipped (with ids), what slipped and why (reasons grounded in the record, never guessed), and what to change next time (concrete and actionable).

## Workflow
Run these steps in order. Git and file work happens in the project's repo checkout; ticket work goes through whatever tracker the project uses.

1. **Scope the project.** Identify the repo, the ticket set (by project label or tag, linked ticket ids, or the parent/epic ticket), and the time window. Confirm the repo name and window with the user only if genuinely ambiguous; everything else is resolvable from evidence.
2. **Pull the git evidence.** Read-only, in the repo checkout:
   - Commit log for the window: `git -C <repo> log --since=<start> --until=<end> --format="%h %ad %s" --date=short`
   - Merged PRs: `git -C <repo> log --merges --since=<start> --format="%h %ad %s" --date=short`
   - Files changed: `git -C <repo> diff --stat <first-commit>..<last-commit>`
3. **Pull the closed tickets.** From the project's tracker, list the tickets closed in the window for this project:
   - Tickets with status done (shipped), then again for dismissed and cancelled (dropped items), filtered by the project label/tag or a text search on the project name/slug, and the date range for the window. Page through all of them, not just the first page.
   - Open each key ticket for its resolution note and close date.
   - For each key ticket, find the commits linked to it (from the ticket's linked commits if the tracker records them, or by searching the git log for the ticket id). An empty result means no evidence was linked, not that no commits exist.
   - If the tracker or your records distinguish them, get the closed vs closed-and-verified split for the window: claimed and merged is not verified.
4. **Read the existing narrative docs.** Newest first in `<repo>/docs/narrative/` (match the project's voice and topic-slug pattern), plus the project's `CHANGES.md`. Also read the repo's own writing style guide if it keeps one, and follow the same documentary approach as your narrative-docs practice if you have one.
5. **Gather memory entries about the project.** Search your notes and memory files for the project name/slug, and any project record you keep. Keep only entries that add something the code and ticket records do not already show.
6. **Synthesize.** Classify every finding as observed (in the record with an id), inferred (you connected dots; mark it as such), proposed (a recommendation), or completed (production evidence exists, not just a merge). Write the retrospective doc and the CHANGES.md entry per the Output Contract. Then stop and report. Do not commit or push.

## Output Contract
- **Retrospective document** at `<repo>/docs/narrative/YYYY-MM-DD-retrospective-<project-slug>.md`, written in the project's documentary voice. It carries exactly three analytical sections:
  - **What shipped.** Each item with its evidence id: commit SHA, PR number, or ticket id. A phase counts as shipped only with production evidence (live verification, deploy record), never at merge.
  - **What slipped and why.** Each item with its reason grounded in a specific source: a ticket body or resolution note, a commit message, or a narrative doc. If no source states the reason, say so instead of guessing.
  - **What to change next time.** Each item concrete and actionable, tied to an observed failure from this project, not generic advice.
- **CHANGES.md entry** (30-150 lines): lead with the decision and the why, reference commits/PRs/ticket ids inline, link the narrative (`Full story: docs/narrative/YYYY-MM-DD-retrospective-<project-slug>.md`), end with what is unblocked or still pending.
- **Report to the user**: conclusions first, the three sections condensed with a link to the doc.

## Operating Rules
1. **Commit+push is gated on explicit ask, every time.** Write the files, then stop and report the diff. No exceptions.
2. Doc writes go directly into the repo's own `docs/narrative/` and `CHANGES.md`, because readers and indexes look for a project's narrative docs in that folder; docs written anywhere else are invisible to them. If any code touch becomes necessary, it follows worktree discipline: isolated worktree branch plus PR plus squash merge, never a commit on the main branch in the canonical checkout.
3. Distinguish observed, inferred, proposed, and completed on every claim. Merged is not deployed, deployed is not live-verified: a phase is shipped only with production evidence.
4. Every shipped claim carries an id (commit SHA, PR number, ticket id). Every slipped reason is grounded in a named source (ticket resolution note, commit message, narrative doc), never guessed.
5. Negative claims need exhaustive evidence. Never assert "nothing else shipped" without checking the git log, the merged-PR log, the closed ticket list, and the narrative docs together; label any conclusion the check cannot verify as unverified.
6. Memory entries that only restate what the git log, ticket records, or narratives already show are dropped, not duplicated.
7. Style: conclusions first; no em dashes; no bold inside prose paragraphs; tables only where they make compact facts clearer.

## Adapting this to your setup

- Point step 3 at your own tracker (GitHub Issues, Linear, Jira, a local backlog): what matters is done vs dismissed vs cancelled, resolution notes, and linked commits, whatever your tracker calls them.
- If your projects do not keep `docs/narrative/` or a `CHANGES.md`, pick one home for retrospective docs and use it consistently; the three-section output contract stays the same.
- If a project has no tickets at all, work from git history and docs alone, and say so in the report so the missing surface is on the record.
- Drop step 5 if you do not keep notes or memory files about projects; the git and ticket evidence carries the retrospective on its own.
