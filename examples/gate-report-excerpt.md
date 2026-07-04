# Exhibit — adversarial gate report (excerpt)

From the delivery-area gate report (`design/qa/A7-qa-report-2026-06-12.md`), the review that ran *after* the area's decisions were ratified and its design doc derived, and *before* the human's forest check. The reviewer is a different, fresh-context agent whose contract says: you are not the author; re-derive every load-bearing claim against its oracle; give **each** finding severity + location + quote-level specificity + a concrete fix; adjudicate the author's flags decision-style. Findings here fed register entry **D054**.

**The per-item contract, visible in the artifact:** findings are numbered (F1–F14) so none can silently vanish; each names its oracle; adjudications end in a verdict; and verified-*correct* claims are enumerated too ("Solid") — the absence of a finding is itself an auditable output, not an implication.

---

## Header (scope + oracle sanction)

> Independent adversarial review; verified against docs.github.com, docs.terragrunt.com, cloud.google.com. 14 findings (2H/7M/5L) + 4 adjudications. Resolutions → D054.

## HIGH — executability bugs in composition, not ratified substance

> **F1** — §2 deployment branch policies BLOCK every §3.1 PR plan job: `pull_request` runs carry `GITHUB_REF=refs/pull/N/merge`, matching no listed pattern → protection gate rejects before token minting. Fix: add `refs/pull/*/merge` rule to each environment policy (GitHub's documented pattern) + honesty sentence (fork walls unaffected; same-repo insider surface = accepted A4 §4 risk + CODEOWNERS) + §12 checklist item.

*Anatomy: the claim was re-derived (what ref does a PR run actually carry?), not reviewed for plausibility. The finding names the failure point (before token minting), the documented fix, and — characteristic of this gate — demands an "honesty sentence" about what the fix does not protect against.*

## MEDIUM (one of seven)

> **F6** — `GITHUB_SHA` on annotated tag pushes = tag object SHA, not commit SHA — derive via `git rev-parse HEAD` post-checkout on prod lane (+ freshness MUST-verify).

*Anatomy: a one-line time bomb — prod release provenance would have recorded the wrong SHA on every annotated tag. Claim type "behavioral" → oracle is vendor documentation; the fix carries a freshness obligation forward so the workaround gets re-verified before it's relied on.*

## Adjudication 4 — the author's model verified CORRECT

> **4** — OIDC VERIFIED CORRECT (caller permissions binding; called side can only downgrade; environment subject mints in reusable workflow w/ caller's numeric IDs; PR events yield environment subject when env referenced) + two pin-notes: reusable workflows carry NO `permissions` block (comment names caller as binding surface); `{id-token: write, contents: read}` on EVERY OIDC-minting caller job (7 callers + infra-pr plans + drift).

*Anatomy: adversarial ≠ fault-finding. The reviewer re-derived the entire OIDC permission chain and certified it — and the act of verification still produced output: two pin-notes that hardened the design. A gate that returns "looks fine" has produced nothing checkable; this one returned the chain it walked.*

## Solid (verified) — the enumerated all-clear

> Plan-tier table exact (required reviewers private = Enterprise-only; tag rulesets on Team); queue:max GA 2026-05-07 exact; … fork three-walls hold; carriage verbatim; all 12 handoff rows land; no ratified decision contradicted.

*Anatomy: exhaustiveness contract closing out. "All 12 handoff rows land" is a completeness sweep against an enumerated inventory — if row 9 hadn't landed, its absence would be syntactically visible here. "No ratified decision contradicted" is the register-conformance verdict every gate must return.*
