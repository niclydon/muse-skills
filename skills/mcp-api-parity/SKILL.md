# MCP API Parity

A reusable playbook for any MCP server repo: inventory the tools the server already has, compare them against the upstream API's official docs and live endpoints, and find the drift, the missing endpoints, and the multi-endpoint composite tools worth building.

**You run the discovery yourself.** Do not stop at a vague summary. Do not implement product code unless the user also asks for implementation in this same session. Default end state of this skill:

1. A parity report with a prioritized table of recommended work.
2. If the user wants it, one issue per recommended item in their tracker, written so another session can implement it without redoing the discovery.
3. A copy-paste kickoff prompt for a follow-up session that implements the work.

## Auth

- Locate credentials **without printing secrets**: `.env` / `.env.example` keys (`*_API_KEY`, `*_BASE_URL`, `*_TOKEN`), README setup docs, `client.py` / `config.py` header names (`X-API-Key`, `Authorization`, …).
- **Hard stop** if live discovery needs credentials and none are available: still complete the **docs-only** inventory and tool inventory, then ask only for the missing environment variables (never ask the user to paste keys into chat if a login tool exists).
- Do not over-gate the read path: a credential check protects the user's own secrets, not service credentials the pipeline already manages.

## Workflow

### Phase 0 — Preconditions

1. **Target repo.** The user may name one, or you are already inside its workspace. Resolve the repo's absolute path before starting.
2. **Git hygiene.** `git status`, current branch, and whether a production deploy path exists. Do not invent deploy paths.
3. **Worktree discipline.** Discovery is read-only and can run anywhere. Any repo mutation (probe scripts excluded) goes in an isolated worktree on a branch with a PR — never commit on the main branch in the canonical checkout.

### Phase 1 — Orient the MCP repo

Build a **tool inventory** from code, not README marketing.

| Look for | Why |
|---|---|
| `**/server.py`, `main.py`, `TOOL_COUNT` | Registration surface |
| `**/tools/*.py`, `@mcp.tool` | Tool list |
| `**/client.py`, `get_client`, `unwrap` | HTTP envelope + base URL |
| `**/models.py` | Documented response shapes (often stale) |
| `*_api_findings.md`, `docs/**` | Prior discovery notes |
| `scripts/test_*.py`, `scripts/*probe*` | How this repo tests |

Extract every upstream path the MCP already calls (adapt the search to the repo's language):

```bash
rg -n 'client\.(get|post|put|patch|delete)\(|"[/][a-z]' <package>/tools <package>/client.py
```

Produce: **MCP tool → HTTP method + path → params → notes**. Note design patterns already in the repo (composite tools, summary modes, rate governors, storage capture, insight envelopes) — these are the **templates** for new multi-endpoint tools. Do not invent a second style.

**Deliverable:** a short orientation card (paths, tool count, auth scheme, base URL pattern, prior findings docs).

### Phase 2 — Discover docs + live API

**Official docs surface.** Probe common locations (adapt the host from the README / env): agent skill pages the vendor publishes, `/openapi.json` / `/v1/openapi.json` / `/docs` / `/swagger`, the developer portal (`…/api-keys`, `…/docs/api`, `llms.txt`), in-repo `docs/` / `openapi.yaml` / Postman collections. Record a **docs content hash** (sha256 prefix) when the content is stable, for drift detection on later runs.

**Separate surfaces (critical anti-pattern):**

| Surface | Treat as |
|---|---|
| **Developer / Open API** (API keys, MCP-facing) | Source of truth for tools |
| **App / web-app backend** (session cookies, mobile) | Out of scope unless documented for API keys |
| **Frontend JS path strings** | Hints only — verify on the Open base URL |

Never recommend MCP tools for private app routes that 404 under the API key.

**Live probe (with credentials):**

1. Confirm auth with a cheap endpoint (`/me`, `/health`, `/v1/user`, …).
2. Hit **every documented path** with minimal valid params.
3. Probe **candidates** (documented paths ± related names), classifying carefully:
   - **200 + success envelope** → real route
   - **404** → not on this surface
   - **401/403** → exists or gateway; note the auth gap
   - **422 / validation error** → inspect `loc`:
     - `["path", "id"]` with input = path segment → **path-param capture**, not a new route
     - `["query", "field"]` → real route, bad/missing params
4. Stay under rate limits (serial or low concurrency; sleep between bursts).
5. **Never log or commit API keys, JWTs, or presigned URLs.**

**Shape + enum drift** (even when paths are unchanged): for each live payload vs `models.py` / tool validators, note nested vs top-level fields, enum case, allowed filter values the API rejects, new optional fields tools ignore, and type-vocab expansion.

**Deliverable:** a filled parity matrix (see the template at the end of this skill): orientation, existing tools → HTTP, live/docs path matrix, contract drift, false positives.

### Phase 3 — Diff vs MCP toolset + recommend work

**Classification of gaps:**

| Bucket | Meaning | Typical work type |
|---|---|---|
| **A. Missing endpoint tool** | Documented/live path with no MCP tool | feature |
| **B. Contract drift / bug** | Tool exists but wrong params/shape | fix |
| **C. Docs-only lag** | Runtime OK; models/README stale | docs |
| **D. Composite opportunity** | Multi-endpoint workflow users ask for | feature |
| **E. Harness** | Re-probe / docs-hash script | automation |
| **F. Out of scope** | Private API, auth surface, write ops the user didn't ask for | note only |

**Composite tools (required brainstorm).** Do **not** stop at 1:1 endpoint mirrors. Explicitly propose multi-endpoint tools inspired by patterns already in the repo: day rollup, search + enrich, diff two layers, harvest structure, clustering related records, capture-before-expiry, realtime + detail lookups. For each: name, endpoints used, default windows/caps, token-cost controls (`mode=index|summary|full`), rate-limit strategy.

**User-facing output.** Present a short summary first, then the detailed prioritized table:

| Pri | Title | Bucket | Why | Suggested tools/files | Effort |
|---|---|---|---|---|---|
| P0 | … | B | hard 422 / broken behavior | … | S |
| P1 | … | A/D | … | … | M |

Rules: P0 = user-visible breakage or hard API rejection on current tools; prefer small, safe, self-contained slices over mega-PRs; cap speculative composites if the rate limit is tight and note the tradeoffs; explicitly list **rejected** ideas (app-only routes, etc.).

Wait for the user's go-ahead **only if** they asked for a report first. If they already said "file the issues" or "write the kickoff prompt", proceed to Phase 4 without re-asking.

### Phase 4 — Turn findings into trackable work (optional)

Only when the user wants the findings filed:

1. **One issue per recommended item** in the user's tracker, each implementation-ready. Every issue body carries: the problem, the evidence from this run (live status, drift details, matrix rows), the endpoints and files involved, the suggested approach, and acceptance criteria a reviewer can check.
2. **Group them under one label or parent issue** named for the run (for example `<repo> API parity — YYYY-MM`), with the discovery date and the docs URL + hash in the parent, so a later run can diff against this one.
3. **Priority mapping:** carry the table's P0/P1/P2 bands into the tracker's priority field or labels; keep the ordering from the report.
4. Mark an item as safe for an unattended session only if it truly is: no secrets, no product decisions, no irreversible actions.

**Deliverable:** the list of issue references, grouped under the run's label or parent issue.

### Phase 5 — Kickoff prompt for the implementation session

Do **not** auto-start implementation unless the user explicitly asks in the same turn. Always **print** the best kickoff prompt, filled in from this run: the repo path, the ordered issue list (P0 first), the branch policy (isolated worktree + PR), the deploy path if one exists, and the closing steps (merge, push, deploy if applicable, a live smoke check, and a short write-up of what changed in the repo's docs).

### Phase 6 — Closeout (after implementation)

When the implementation is done (this session or a later one), write a short record of the discovery → implementation arc in the repo's own docs (a dated note under `docs/` plus a changelog entry if the repo keeps one). The record must land in the work's own repo, never elsewhere.

## Parity matrix template

Fill during Phases 2–3. Do not put secrets in it.

**Orientation**

| Field | Value |
|---|---|
| Repo path | |
| Package / tool count | |
| Auth scheme | |
| Base URL env var | |
| Official docs URL(s) | |
| Docs content hash (prefix) | |
| Discovery date | |

**Existing tools → HTTP**

| MCP tool | Method | Path | Params | Notes |
|---|---|---|---|---|
| | | | | |

**Live / docs path matrix**

| Path | In docs? | Live status | Envelope | In MCP? | Classification |
|---|---|---|---|---|---|
| | | 200/404/422/… | | Y/N | A–F or false positive |

**Contract drift**

| Area | Docs/models say | Live says | Tool impact |
|---|---|---|---|
| | | | |

**Recommended work**

| Pri | Title | Bucket | Issue filed |
|---|---|---|---|
| P0 | | | |
| P1 | | | |

**Rejected**

| Idea | Why |
|---|---|
| | |

## Output contract

A completed run ends with:

1. The parity report (summary + priority table), seen by the user.
2. If requested, one issue per recommended item in the user's tracker, grouped under one label or parent issue.
3. A kickoff prompt for the implementation session, printed, not auto-started.
4. Closeout write-up instructed or executed after implementation ships.

## Operating rules

- Never commit on the main branch; repo work means an isolated worktree + branch + PR.
- Filing one mega-issue for the whole run is a failure: implementers want small, self-contained slices.
- Mark work safe for an unattended session only when it needs no secrets, no manual clicking through product UIs, and no product decisions.
- Secrets (API keys, JWTs, presigned URLs) never go in issues, docs, logs, or chat.
- Do not invent deploy paths, merge ambiguous identities, or recommend app/private-API tools for API-key auth. Do not skip the composite brainstorm.
- Session checklist: Phase 1 tool inventory + path map from code → Phase 2 docs hash + live matrix + false positives classified → Phase 3 priority table including ≥1 composite opportunity (or an explicit "none") → Phase 4 issues filed if requested → Phase 5 kickoff prompt printed → Phase 6 closeout instructed or executed after implementation.

## Adapting this to your setup

- Works for any MCP server repo, in any language: adjust the Phase 1 file patterns and the Phase 2 path-extraction search to your stack.
- Phase 4 is written for any issue tracker (GitHub Issues, Linear, a local backlog file); if you have none, skip it and keep the report plus kickoff prompt as the deliverables.
- If the upstream API has no live test credentials available, run Phases 1–3 in docs-only mode and label every live-status claim as unverified.
- Add your own repo's design patterns to the Phase 1 pattern list, so composite proposals match the style already in the codebase.
