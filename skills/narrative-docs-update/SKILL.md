# Narrative Docs Update

## What this produces

- Write the documentary record of a completed arc of work into the work's own repo. The primary artifact is a long-form narrative in `docs/narrative/` carrying the why and the how.

- Treat `CHANGES.md` as the secondary record: a tighter chronological entry that points at the narrative. Never write only the CHANGES.md entry and skip the narrative, because an index without the content is the most common failure of this task.

- Treat documentary thoroughness as policy, not debt. Do not apologize for length and do not offer skip-ahead shortcuts.

- Work where the repo lives: on this machine, or on the host where the repo lives if your repos are kept on another machine. The layout rules are the same either way.

## The documentary style

- Write in plain, concrete prose that a reader who was not there can follow a year later. Decisions and reasons carry the story; the diff is supporting evidence, not the story itself.
- Open with the decision and the stake, not with a list of files changed.
- Record specifics a future reader needs to reconstruct the moment: paths, numbers, error messages, and the alternatives that were rejected and why.
- Keep one topic per document, use headings to break a long arc into chapters, and let tables carry compact before-and-after facts where they help.

## Read before writing

- Read the project's own newest narrative docs to match its established voice and topic-slug pattern, plus its `CHANGES.md`, any contributor or agent notes file, and any `docs/plans/*.md` or `docs/roadmap.md` the work touched.

- Inventory the arc before drafting: `git log` since the last narrative doc, `git status`, and `git diff`. Reconstruct the story, not just the diff: what triggered the work, what was tried, what was rejected, what numbers were observed, and what is now unblocked.

- Skip this skill entirely when the ask was only a one-line README tweak or a single-line fix with no arc behind it.

## Write the narrative

- Create the narrative at `<repo>/docs/narrative/YYYY-MM-DD-<topic-slug>.md`, creating `docs/narrative/` when it does not exist. The slug is short and content-bearing, such as `model-onboarding-marathon` or `apns-cert-recovery`, never `phase-26` or `update-2026-05-09`.

- Keep one topic per narrative doc. CHANGES.md may group two arcs under one date heading, but the narratives stay separate.

- Open with a topic sentence that names the decision and the stake, in the shape of "X had been doing Y, which broke Z, so we did W." Never open with "This change adds X."

- Write 200 to 500 lines in documentary voice, with H2 and H3 headings breaking the arc into chapters. Prose carries the decisions, and tables are fine for compact facts such as before-and-after state or options compared.

- Include everything a future reader needs to reconstruct the moment: file paths, migration numbers, table names, ports, model ids, row counts, latencies, costs, error messages verbatim, commit hashes when available, and the alternatives rejected with the reasons.

- Close the narrative by citing the secondary record: "See `CHANGES.md` for the chronological diff summary."

## Update CHANGES.md and the plan docs

- Append a CHANGES.md entry of 30 to 150 lines: lead with the decision and the why, reference commits, pull requests, and issue numbers inline, link the narrative by path, and end with what is unblocked or still pending. Create CHANGES.md with a top-level header when the repo has none.

- Update each touched plan doc: mark completed items with the date and a one-line outcome rather than a bare checkmark, move items between Now, Next, and Later to match what actually happened, add new items that emerged, and record why superseded items were dropped instead of silently deleting them.

- Fold in, never rewrite, when editing an existing narrative, CHANGES.md, or plan doc: preserve every existing heading verbatim and in order, extend rather than replace, and merge lists as a union. A fresh per-session narrative is a clean write and is exempt.

- When the work touches a second repo, add a pointer in that repo's CHANGES.md, plus a narrative there too when the change is non-trivial, and note any contract or interface change both sides need to know about. For multi-repo work, write every repo's docs before committing any of them, to avoid a half-synced state.

## Guardrails

- Never paste a secret value into a narrative or CHANGES.md entry. Reference the fact that a key was added or rotated, never its bytes.

- Never put private hostnames, internal addresses, or personal paths in the docs. Use public hostnames, example values, or `localhost`.

- Check your project notes and memory for facts the work just made stale and update or remove them. Save new notes only for surprises not derivable from the current code state.

- Write month-end rollups named `YYYY-MM-project-snapshot.md` only at month end and only when asked, never per session.

## Commit posture

- This skill writes files only. Commit and push happen only when the user asks, or when this skill runs as the documentation step of the `ship` skill, where the ship invocation carries the approval and the docs land in the same commit as the code.

- Stage the narrative, CHANGES.md, and plan changes together. Never commit a CHANGES.md entry that points at an unstaged narrative doc.

## Adapting this to your setup

- Set the length bar to fit your projects: 200 to 500 lines suits a substantial arc; scale down for small repos rather than padding a short story.
- If your repo keeps its changelog in a different file or format, point the secondary record there instead of CHANGES.md, and keep the rule that the short entry links to the long narrative.
- Decide where plan docs live in your repos (`docs/plans/`, a roadmap file, or an issue tracker) and update those in the same pass.
- If you work across several repos, keep the rule that every affected repo gets its docs written before any of them is committed.
- Add your own style notes to the documentary style section as your voice settles: the first few narratives set the pattern the rest will follow.
