# Exhibit — register slice

Four real entries from the source project's DECISIONS.md (genericized: app names/domains replaced, the human = "the operator"; IDs, dates, and technical content are as recorded). Chosen to show the register's four load-bearing entry species: a **meta-decision** (the process governing itself), a **design decision carrying an override**, a **QA-gate resolution block**, and the **freeze**.

## Legend

- **ID** — immutable; docs cite it forever. Change = a new entry that supersedes (see D002's status below — the register's own first process rule was corrected this way, never edited).
- **Area** — `meta` entries are process rules; they bind agents exactly like design entries bind docs.
- **Decision** — carries the resolution *and* the reasoning, including overridden recommendations and rejected runners-up, so design docs stay clean and the road-not-taken stays citable (e.g., "D038 precedent").
- **Status** — `ratified` | `superseded-by-D###`. Nothing is ever deleted.
- **Sources** — where authority came from: the interview, a decision doc's question, a QA report, the operator verbatim.

---

| ID | Area | Decision | Status | Sources | Date |
|---|---|---|---|---|---|
| D013 | meta | Supersedes D002: every area decision (detail or load-bearing) is presented in a comprehensive decision document — full corpus evidence with context, cases for/against, pros/cons, risks, best-practice findings, recommendation + why, runner-up + why — BEFORE any question is asked. The operator decides after reading. | ratified | operator 2026-06-11 | 2026-06-11 |
| D006 | tooling | **Terraform ratified (operator override of OpenTofu recommendation)** — parity research verified tofu 1.11+ has ephemeral/write-only, but the operator weighted ecosystem majority + zero feature lag; engine pinned TF ≥1.11 (corpus assumption); D027 configs stay engine-neutral so reversal = pin change | ratified | A2 doc Q9 + research | 2026-06-11 |
| D054 | delivery | A7 QA resolutions (report design/qa/A7-qa-report-2026-06-12.md; no design reopens): F1 environments gain refs/pull/\*/merge branch rule (PR plans mint again); F2 infra-dev.yml gains workflow_dispatch+edge input; F3 reviewer-gates-everything documented, {scope}-plan companion env = register-gated option; F4 CI scopes = {dev,stg,prod,dns}, bootstrap excluded by construction (genesis-unit changes planned human-impersonated; bootstrap state outside drift — named residual); F5 dns-records name sweep (A5/A4); F6 prod SHA via git rev-parse HEAD post-checkout (annotated-tag GITHUB_SHA hazard) + freshness verify; F7 main-branch ruleset requiring code-owner review added to template checklist; F8 cross-lane displacement documented, queue:max default for prod/dns; adjudications: 11-file prose fix; A4 §9 dns footnote; A5 rider MUST+sweep; OIDC pin-notes (no permissions block in reusable workflows; id-token pair on every minting caller). Propagation: A7 v2, A2 v2.7, A4 v5, A5 v2.1, A6 v2.1, A3 v2.4 as needed | ratified | QA gate 2026-06-12 | 2026-06-12 |
| D064 | freeze | **GOLDEN DESIGN v1.0.0 FROZEN (2026-06-12)** — see FREEZE.md. Gates passed: D062 zoom-out (invariants), D063 freshness (12 confirmed/4 changed/0 unresolved). Corpus: A1 v13, A2 v2.11, A3 v2.7, A4 v8, A5 v2.4, A6 v2.5, A7 v2.2, A8 v2.2, A9 v2.2 + PROCESS + 63 register entries + 95-file archive + 10 QA reports. Register-gated catalog: 16 items consolidated in FREEZE.md §3. Change policy: register supersession only (D-entry + version bump). Next: phase ③ ops guides (C2-runbook-inventory), then implementation. The operator to tag v1.0.0 | ratified | FREEZE.md | 2026-06-12 |

---

## What each entry demonstrates

- **D013** — self-hosted process. The operator's feedback ("short pre-answered briefs don't work — I can't evaluate them") superseded the day-old D002 and became a rule every later agent reads from disk. Two later entries (D023, D030) tightened it further when backsliding was caught.
- **D006** — the override as first-class record. The recommendation, the human's contrary reasoning, *and* the designed escape hatch (engine-neutral configs; reversal = pin change) live in one immutable row. Nobody later has to wonder whether OpenTofu was considered.
- **D054** — a QA-resolution block: one entry closes an entire adversarial gate report, per-finding (F1–F8), with the adjudications and the cross-document propagation set (six docs, each with its version bump) named inside the entry. The gate report itself stays frozen; this entry is its executable summary.
- **D064** — the freeze as a register event with named preconditions (two passed gates cited by ID), a pinned manifest, and the change policy that makes "frozen" mean something: supersession only.
