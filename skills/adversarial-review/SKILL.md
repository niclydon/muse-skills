# Adversarial Review

## Purpose

A review whose goal is to **break the change, not bless it**. Reviewers assume the change is wrong and hunt for the specific way it fails, instead of reading sympathetically and approving. Run it pre-merge over the committed diff, then triage every finding: fix, defer (to a ticket opened in your tracker), or decline (with a one-line reason). Keep the full policy in a doc in your repo and point this skill at it: that doc is the canonical home for the full doctrine, trigger tiers, and triage contract.

## Workflow

1. **Gate.** Run AR when the change touches any of:
   - **Irreversible / high-blast-radius:** production gateway/service, LLM router, DB migrations and backfills, identity-graph merges, schema changes to load-bearing tables, the deploy path.
   - **Security-sensitive:** authentication, secrets handling, tool grants, DB-role isolation, anything on a public surface.
   - **Correctness-critical:** subtle ordering, concurrency, races, or reward / financial / scoring semantics where a wrong-but-plausible result is hard to spot later.

   Skip AR for docs, surgical single-file fixes, config tweaks, additive non-load-bearing features, and test-only changes.

   **Hard gate** (migrations, auth/secrets/tool-grants, public surface, prod gateway, deploy path): the change does NOT merge until AR has run and every finding is triaged, with the triage recorded in the PR body or close evidence. **Strong default** (everything else in the list above): AR is expected; skipping requires a one-line written justification recorded in the PR/close evidence (e.g. "skipped AR: additive read-only endpoint, no shared state"). Silent skipping is not allowed.

2. **Commit first.** AR runs over the committed diff, never uncommitted working-tree state. Commit the change on the branch, then snapshot the pre-review fingerprint: `git rev-parse HEAD` and `git status --porcelain=v1 -uall`. Review the base as `origin/main...HEAD` (origin/main, not local main: local main may be stale and inflate the diff).

3. **Choose lenses.** 2-4 lenses matched to the change (default: correctness, data-flow, edge-cases, blast-radius). `blast-radius` deliberately asks the scale/limits question a green test suite never asks. For a concurrency-heavy change use concurrency / data-flow / the-math; for a security change use authz / input-handling / secret-exposure.

   **Security lens pack for API and MCP diffs** (harvested from grim-prompter, jmanico/grim-prompter @ 2a94457, MIT: `quick/api.md` checks 1-8 and `reference/tool-credential-boundaries.md`): whenever the diff touches an API surface, an MCP server/tool, or a connector sharing a service identity, add the named pack `api-and-credential-boundaries` as one of the lenses (it replaces the generic security lens for that surface, it does not add a fifth reviewer). The reviewer works these checks as coverage lenses, not mandatory defects, and preserves protective observations:
   - **API boundaries:** (1) follow caller identity and intended audience through gateway admission to backend authorization; distinguish client, resource owner, and delegated actor, and preserve issuer/subject through any identity translation (recognition is not permission). (2) Trace a changed object identifier through individual reads, updates, bulk selection, asynchronous export execution, and result retrieval; authority is bound to owner, action, and principal at each decision point, including what is rechecked when work is delayed, ownership changes, or access is revoked (a passing GET proves nothing about export or download). (3) Separate object authority, function authority, and property authority; follow caller-supplied and backend-returned fields to their actual use (an allowed read authorizes neither a privileged operation nor disclosure of every returned property). (4) Trace replay, duplicates, concurrent requests, and operation order against the explicit business invariant, its commitment point, and retry after an unknown outcome (a unique request id alone establishes neither safe sequencing nor exactly-once). (5) Trace a caller-controlled callback destination from registration through storage, outbound-worker authority, destination checks, and redirects. (6) Follow the notification payload out and response fields back: separate the data authorized to leave from the authority granted to returned fields; an acknowledgment must not acquire business-state authority. (7) Review bounded use and recovery of bulk/callback work: cancellation, delayed jobs, retained results, retries, and access after revocation. (8) In mixed API/federation scope, trace issuer and subject to the global principal, then to resource ownership and asynchronous authority; audience, scope, and authenticated identity are distinct from the ownership decision.
   - **Tool-credential boundaries (MCP / agent tools / connectors):** build a per-tool effective-authority map: server and executor, intended read/write/admin operations and resource/tenant scope, the inbound client-to-server identity separately from the downstream execution principal and its credential reference, which tools share that credential, and the credential's effective downstream grants. Compare each tool's required authority with the shared credential's effective grants: one server credential accumulates the union of its tools' permissions, so name the shared-token blast radius, cross-tenant reach, confused-deputy use without token theft, credential disclosure and direct backend use, loss of audit attribution, and the shared failure domain (rotation, revocation, or compromise hits every consumer). A tool named "read" or annotated read-only is a claim until its implementation and enforcement support it; different key strings with the same broad grants, or separate processes that can both obtain the write credential through environment, secret store, or fallback identity, are not separation. Also treat tool descriptions as an input surface: inventory the metadata source/controller, the model-visible definition and revision, and post-approval description changes (tool-description poisoning, shadowing of another server's tool, redirection through the poisoned tool's own arguments), tracing metadata -> model-visible context -> selected tool and arguments -> approval -> execution identity -> effect or disclosure. Pinning, signing, scanning, or approval UI alone does not prove backend enforcement.

4. **Fan out.** Dispatch one independent reviewer agent per lens, in parallel. Each reviewer gets this prompt, with the lens substituted in:

   > You are an ADVERSARIAL reviewer. Your job is to BREAK this change, not bless it. Assume the change is wrong and hunt for the specific way it fails. Default to "broken" when uncertain; over-flagging is expected, a triage step removes false positives.
   >
   > REPO/WORKTREE: `<path>`. Read the diff: `git -C <path> diff origin/main...HEAD`, then open and read the files it touches; the diff alone is not enough. CHANGE CONTEXT: `<one or two sentences: what the change does and where to look>`.
   >
   > YOUR LENS — `<key>`: `<focus>`.
   >
   > A finding is NOT "this seems risky." It is the exact input or sequence that breaks it, the file, and a concrete suggested fix, so it can be acted on or dismissed on evidence. Severity: blocker (must fix before merge) / major (real defect, fix or consciously accept) / minor / nit. Severity carries no numeric score: do not attach a number (no CVSS, no DREAD, no x/10) unless it is computed from a named, verified rubric you cite; blocker/major is the gating signal.
   >
   > Label every finding with an evidence type (harvested from grim-prompter, jmanico/grim-prompter @ 2a94457, MIT: artifact contract and stage 02): `evidenced-defect` or `design-hypothesis`. An `evidenced-defect` requires an implementation or execution observation at HEAD: code you read at HEAD that does the wrong thing, or a run whose output shows it. A description, a design doc, a comment, or a plausible failure chain alone grounds only a `design-hypothesis`, stated with its prerequisites; a hypothesis is not a confirmed defect and does not become one by being repeated. Record `confidence` (low / medium / high / unknown) and a `confidence_rationale` next to severity for every finding: what supports it, what argues against it, and what observation would decide it. Confidence is separate from severity; never express it as a percentage or fold it into a score.
   >
   > Before you strengthen a finding toward blocker, seek the protective counterexample: the guard, check, caller constraint, or upstream enforcement that would defeat your breaking sequence if it exists. Name what you looked for and where. If you find it, scope or drop the finding; if nothing rules it out and nothing confirms its absence either, keep the gap explicit in the finding instead of asserting certainty.
   >
   > Ground every finding in committed HEAD. Re-open the cited file with `git show HEAD:<file>` before returning it, and include a bounded excerpt that exists at HEAD (`head_excerpt`). Do not cite uncommitted working-tree state, including code you just injected.
   >
   > Return a coverage entry with your findings (see step 5): every file you actually read with its extent (full read, or the exact ranges / partial extent and what was unread), any in-scope area you skipped with the reason, whether each item was read fresh in this review or carried from a named earlier source, and any boundary you could not map (a caller, store, external service, or async handoff whose other side you did not inspect). A search hit, a directory listing, or a filename is not a read.
   >
   > Report ONLY real defects in `findings`. If you find nothing under this lens, return empty findings and say what you actually checked.
   >
   > HARD RULES — read-only review, no live writes:
   > - NEVER execute a migration file or run INSERT/UPDATE/DELETE/DDL against a live database. If a lens needs live DB state, use a read-only database connection (SELECT only). A database URL in the environment may be the production write role; using it during a review is a defect.
   > - NEVER edit, create, delete, checkout, reset, clean, or stash files in the reviewed worktree; never `git add` or `git commit` there. Injecting experiments into that tree and reporting them as blockers is a defect. If you must run a mutating experiment, copy the files into a temp dir first and leave the reviewed worktree untouched.

   Require each reviewer to return structured findings: `severity`, `evidence_type` (`evidenced-defect` / `design-hypothesis`), `confidence`, `confidence_rationale`, `title`, `file`, `detail` (the exact input/sequence that breaks it), `suggested_fix`, `head_excerpt`, plus an `overall` verdict and its coverage entry. If a lens returns placeholder or content-empty output, retry it once; if it still returns garbage, record it as a failed lens. Drop any finding whose `head_excerpt` is missing or does not match committed HEAD. A finding returned without an evidence type or without confidence and its rationale is incomplete: send it back or downgrade it to `design-hypothesis` with confidence `unknown` and the gap recorded, do not silently upgrade it.

5. **Verify the review was clean, consolidate coverage, and run the counterevidence step.** After fan-out, compare `git rev-parse HEAD` and `git status --porcelain=v1 -uall` against the pre-review fingerprint; if they differ, the review is invalid.

   Consolidate the reviewers' coverage entries into one **inspection coverage ledger** for the review (harvested from grim-prompter, jmanico/grim-prompter @ 2a94457, MIT: runbook Step 2 inspection ledger): files read with extent (full, or partial with the unread part stated), areas skipped with reasons (size, binary, access, context budget, out of scope, labeled as such), fresh vs carried for each item (carried names its source; reading a report that cites a file is not a fresh read of that file), and unmapped boundaries (crossings whose other side was not inspected, with what would settle them). Group skipped trees by directory or pattern without implying every member was enumerated, never open secrets or expand scope to fill the ledger, and never equate file counts with exhaustive coverage. The ledger goes in the review output (see Output Contract); a review with no ledger is incomplete, and a lens's bare "looks good" with no ledger behind it is not a review.

   **Counterevidence step** (harvested from grim-prompter, jmanico/grim-prompter @ 2a94457, MIT: stage 03 and `helpers/model-challenger.md`): before any blocker is final, the coordinator names the protective counterexample that would defeat it (the guard, validation, caller constraint, platform enforcement, or earlier check that would stop the breaking sequence) and the evidence that rules it out at HEAD. If the counterexample is found and not ruled out, the finding is downgraded or declined on that evidence. If nothing rules it out and its absence is also unproven, the finding still ships, but with the gap stated explicitly in the finding and in the triage record; it is never silently presented as fully proven. Majors get the same step when triage time allows; blockers get it without exception.

   Optional, if your repo has a check for untracked durable docs (for example a script that lists untracked files under `docs/**` or a changes directory), run it from the reviewed worktree before treating AR as finished: if it reports anything, block, commit or delete the named files, and do not merge or dispose of the worktree until the list is empty. Skip this step when your repo has no such check. Never auto-commit.

6. **Triage.** The review SURFACES; you DECIDE. Disposition **every** finding:
   - **Fix** — change the code before merge, re-run the relevant tests. Every Fix entry carries a verification shape (harvested from grim-prompter, jmanico/grim-prompter @ 2a94457, MIT: stage 04 treatment and validation): the re-check to run, with distinct **pass** criteria (the observable result that proves the breaking sequence no longer works and the legitimate path still does), **fail** criteria (the observable result that shows the defect, or a regression of the legitimate path, is still present), and **inconclusive** criteria (what missing evidence, unavailable environment, or unobservable effect would leave the fix unverified, and what remains unknown then). Criteria must be falsifiable observations, not "retest it." A Fix entry without all three is incomplete; the verification starts as not run and is never recorded as passed without execution evidence.
   - **Defer** — real but out of scope: open a ticket in your tracker and reference it. Never "noted in passing."
   - **Decline** — not a real defect, or cost outweighs value: record the one-line reason. A false positive is the system working, not a failure. Where the counterevidence step defeated a blocker, decline (or downgrade) on that counterexample and cite it.

   Record the triage in the PR body or close evidence. For hard-gate-tier changes that record is mandatory; without it the merge is not allowed.

## Design Review Path (investigation dispositions)

When the AR target is an investigation's conclusion rather than a code diff, use this path instead of the diff-based workflow above. The goal is the same — break the conclusion, not bless it — but the target is reasoning, not code.

**Target.** The investigation report in your tracker: the evidence gathered, the reasoning chain, and the proposed disposition (close it, approve it for implementation, convert it into a decision for the user, open follow-up tickets, and so on).

**Lenses.** 2-4 matched to the investigation (default: evidence-completeness, reasoning-validity, alternative-hypotheses, actionability).
- `evidence-completeness`: Did the investigation consult all relevant sources? Are there obvious evidence gaps?
- `reasoning-validity`: Is the logical chain from evidence to conclusion sound? Are there leaps, circular reasoning, or unsupported inferences?
- `alternative-hypotheses`: Were plausible alternative explanations considered and ruled out with evidence, not just dismissed?
- `actionability`: Does the disposition follow from the findings? Is a "safe to implement as proposed" verdict actually safe, or does it need more work?

**Fan out.** Dispatch one independent reviewer agent per lens, in parallel. Each reviewer gets:

> You are an ADVERSARIAL reviewer. Your job is to BREAK this investigation's conclusion, not bless it. Assume the disposition is wrong and hunt for the specific way the reasoning fails. Default to "broken" when uncertain; over-flagging is expected, a triage step removes false positives.
>
> Ticket: `<id>`. Read the ticket body, then the investigation report attached to it. Follow the evidence references to their sources (files, queries, linked records, and so on) and verify the key claims yourself; the investigation's summary alone is not enough.
>
> YOUR LENS — `<key>`: `<focus>`.
>
> A finding is NOT "this seems thin." It is the specific evidence gap, reasoning flaw, or unconsidered alternative that breaks the conclusion, with a concrete suggestion for what would fix it. Severity: blocker (disposition is unsafe/wrong, must re-investigate) / major (real gap, fix or consciously accept) / minor / nit. Label it `evidenced-defect` or `design-hypothesis` and give `confidence` with a `confidence_rationale`; attach no numeric score.
>
> Ground every finding in the actual investigation record and its cited sources. Quote the specific claim you're challenging.
>
> Report ONLY real defects in `findings`. If you find nothing under this lens, return empty findings and say what you actually checked.
>
> HARD RULES — read-only review:
> - NEVER write to the ticket, modify the investigation record, or take action on the disposition. You are reviewing, not re-investigating.
> - NEVER treat the investigator's conclusions as ground truth; verify the key evidence yourself.

Apply the same evidence discipline as the code path, adapted to reasoning: label each finding `evidenced-defect` (the record or a cited source you re-read contradicts the conclusion) or `design-hypothesis` (a plausible gap or alternative the record does not settle), with `confidence` and a `confidence_rationale` (supports / argues against / deciding observation) next to severity, and no numeric scores. The `alternative-hypotheses` lens is this path's counterevidence step: before a blocker on the disposition is final, name the protective counterexample (the evidence or source that would make the disposition safe) and what rules it out, or ship the finding with that gap explicit. Each reviewer's return also feeds the coverage ledger: sources actually re-read with extent, sources skipped with reasons, fresh vs carried, and boundaries (source families, systems) not consulted.

**Triage.** Same as the code path: fix (re-investigate and update the record), defer (open a follow-up ticket), or decline (with one-line reason). A fix entry carries the same pass / fail / inconclusive verification criteria for the re-investigation. Record the triage on the ticket. The disposition does not proceed until blockers are resolved.

## Output Contract

- Findings grouped by severity: `blockers` / `majors` / `minors` / `nits`, plus `triage_required` (blockers + majors), each finding carrying severity, evidence_type (`evidenced-defect` / `design-hypothesis`), confidence, confidence_rationale, title, file, detail, suggested_fix, and head_excerpt. No finding carries a numeric severity score unless the score, the named rubric it was computed from, and that rubric's verification are all recorded; otherwise the finding is unrated by default and blocker/major remains the gating signal (harvested from grim-prompter, jmanico/grim-prompter @ 2a94457, MIT: risk contract and the CVSS/DREAD methods, which refuse a number without verified rubric inputs).
- An **inspection coverage ledger** section, before or alongside the findings: files read with extent, areas skipped with reasons, fresh vs carried per item, and unmapped boundaries with what would settle them.
- For every blocker, a **counterevidence** line: the protective counterexample named, and either the evidence at HEAD ruling it out or the explicit gap that remains.
- One-line `overall` verdict per lens; if nothing was found, it names what was actually checked under that lens (a bare "looks good" is not a review).
- A triage record for every finding: fix / defer (with the ticket reference) / decline (with the one-line reason). Every fix entry in that record carries its pass / fail / inconclusive verification criteria.
- For hard-gate changes, the triage record lands in the PR body or close evidence; no record means no merge.

## Fix loop (who does what after the verdict)

The review ends at triage; the fix loop starts there and it is owned, not hoped for.

- Verdict `block` routes the work back to the implementation owner with `required_fixes` as the spec, findings attached to the ticket. The ticket stays in a review-fixes state; it is not left in triaged limbo.
- The worker addresses every required fix in an isolated worktree, opens a fix PR, and requests a new review on the same ticket. The re-review judges the fix against the triage entry's own pass / fail / inconclusive criteria: pass merges the point, fail reopens it, inconclusive leaves it open with the missing evidence named, never waved through. No merge until a review returns `approve`.
- Findings declined with a recorded reason need no re-review; findings fixed or deferred-with-ticket do.
- Reviews are picked up from the review queue only; implementation work must not grab review tasks.

## Operating Rules

1. AR complements tests, build, and typecheck; it never replaces them. Those run first and independently.
2. Over-flagging is expected. Refute by default: "try to break this; if uncertain, mark it broken."
3. Findings must be grounded in committed HEAD. Never in a reviewer-injected experiment, never in uncommitted state.
4. Reviewers are read-only against the worktree AND the live database. Any mutation is a defect in the review, not a finding against the change.
5. Triage every finding. AR without triage is noise.
6. The hard gate is real: migrations, auth/secrets/tool-grants, public surface, prod gateway, and the deploy path do not merge until AR has run and every finding is dispositioned with the record to prove it.
7. For the heaviest changes, a separate, more expensive multi-agent review pass that only the user can launch is a distinct option an agent cannot start itself; it does not replace the in-session run.
8. Every finding is typed and confidence-rated: `evidenced-defect` only on an implementation or execution observation at HEAD, otherwise `design-hypothesis`; confidence and its rationale sit next to severity, separate from it.
9. Unrated by default: no numeric severity scores without a named, verified rubric. Blocker/major is the gating signal; an invented number is a defect in the review.
10. No blocker is final without the counterevidence step, and no review is complete without its coverage ledger. A gap left by either is stated in the output, never smoothed over.
11. No fix is verified without its pass / fail / inconclusive criteria being run against evidence; inconclusive is a real outcome, not a pass.

## Adapting this to your setup

- Point the Purpose section at your own policy doc if you keep one, or delete that sentence and let this skill be the whole policy.
- Adjust the gate list to your stack: name the surfaces in your project that count as high-blast-radius (payments, migrations, auth) and the ones that never need this review.
- Decide where triage records live in your workflow (PR body, ticket comments, or close evidence) and say so in step 6.
- If your assistant cannot dispatch parallel reviewer agents, run the lenses one at a time in separate sessions; independence between lenses matters more than simultaneity.
- Keep the read-only database rule in whatever form fits your tooling: reviewers get a SELECT-only connection or none at all, never the production write credentials.
