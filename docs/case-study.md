# Case Study — a year of research into a frozen golden design

The exhibits in [`examples/`](../examples/) are lightly genericized artifacts from a real project: consolidating ~1 year of GCP/Terraform infrastructure research into a frozen, QA-gated reference architecture ("the golden design"), with a SaaS app — here called **exampleapp** — as instance #1. App names and domains are genericized; decision IDs, dates, findings, and technical content are real.

**The exhibits, and where this narrative touches them:**

| Exhibit | What it shows | Referenced in |
|---|---|---|
| [register-slice.md](../examples/register-slice.md) | four entry species: meta (D013), override (D006), QA-resolution (D054), freeze (D064) | §2, §3, §5 |
| [decision-doc-section.md](../examples/decision-doc-section.md) | one complete question (A8·S1) at the completed-staff-work bar | §3 step 2 |
| [gate-report-excerpt.md](../examples/gate-report-excerpt.md) | the per-item finding contract + an adjudication that verified the author *correct* | §4.3 |
| [amendment-ledger.md](../examples/amendment-ledger.md) | an executed cross-doc ledger with grep post-conditions, and the one that slipped | §4.4 |

## 1. The starting state

By mid-2026 the operator had accumulated **95 source files across seven provenance groups**, dated 2025-11 through 2026-02:

| Group | What it was | Why it conflicted |
|---|---|---|
| g1 | deep-research library (LB, logging, OTel, Cloud Run…) | thorough, but pre-Terragrunt-1.0 |
| g2 | multi-model plan bakeoff | plans disagreed with each other by design |
| g3 | the app spec | app-shaped, not template-shaped |
| g4 | as-built docs of a working v1 deployment | plain Terraform, central state, script-driven secrets |
| g5 | Terraform notes | fragmentary |
| g6 | adjacent project docs (app internals, CI deep-dives) | different repo conventions |
| g7 | a season of ChatGPT exports | long, contradictory, undated positions inside |

The material was good — and it disagreed with itself across *generations*:

- The **as-built v1** (g4) transited secret values through tfvars into apply.
- The **deep-research generation** (DR-series), in one case written *one day later*, condemned exactly that mechanism — calling write-only arguments "the intended solution" to the tfvars transit "the thing it exists to kill."
- Every position predated **Terragrunt 1.0** (stacks GA, 2026-03), which changed what was even possible.

Any single agent asked to "consolidate this" would have blended the generations into something plausible and quietly wrong. The failure mode wasn't missing information; it was that nobody could say, for any given claim, *which generation's position it was and whether it survived*.

## 2. Seeding — and the human's feedback becoming rules

Work began 2026-06-11 with a **goals/process interview**, producing PROCESS.md and the first register entries — meta-decisions before any design decision:

- **D001**: golden design is a reusable template; exampleapp is instance #1 via a thin instance layer.
- **D003**: corpus-first distillation; one global freshness refresh at the end (sole exception: the tooling fork, which everything downstream depends on, got a fresh check immediately).
- **D016**: evidence ordered chronologically; the latest position is presumptively evolved thinking, overridable with reason.

The self-hosting property showed up immediately, because the first process design was *wrong* and the register recorded its correction rather than anyone's memory of it:

- **D002** ratified a "mixed" Q&A mode — small decisions arrive as short pre-answered briefs. In practice the short briefs failed: the operator was being asked to ratify things he couldn't evaluate from a paragraph. His feedback became **D013**, superseding D002: *every* decision — detail or load-bearing — is presented in a comprehensive decision document (cold-start background, chronological corpus evidence, options, risks, recommendation + why, runner-up + why) **before any question is asked**.
- Backsliding was caught twice more, and each catch became citable law: **D023** (the iron law — questions are asked ONLY from the document; chat-only question lists violate the process) and **D030** (if any evidence leg is thin, research it *before* presenting; the in-question research option exists for the human's judgment calls, not thin preparation).
- Most tellingly, **the adversarial gate itself was a mid-process amendment**: **D021** — an independent-agent QA review before each new area — was added *after* Area 1 closed, at the operator's demand. The pattern's signature mechanism entered the project as a register entry, not a founding assumption. Its retroactive run on Area 1 promptly paid: 16 findings, three HIGH, including a GCS backend example silently written in S3 syntax (D022).
- **D015** amended the oracle list itself mid-flight: official Terragrunt docs became a sanctioned reference for all areas, because the corpus predated TG 1.0 — a register entry deciding what counts as truth.

"Short briefs don't work" stopped being feedback and became an enforced rule every subsequent agent read from disk. That is the meta-mechanism: *the process is data, not folklore*.

## 3. The cycles

Nine areas ran through the 8-step loop, 2026-06-11 to 06-12: foundations → IaC structure → naming → IAM → networking → runtime → delivery → data → observability. By the end of the recorded arc the register held **68 entries** (63 at freeze); ten QA reports; the most-touched design doc (A2) reached v2.11 through eight areas' amendment sweeps.

One cycle in miniature — **Area 8, data & artifacts**, all on 2026-06-12:

1. **Mine**: fresh-context agent extracts the area's claims and conflicts from the archive, chronologically (the tfvars-vs-write-only arc above is its mining note #1).
2. **Decision document**: six questions (S1–S6), each at the completed-staff-work bar — see [`examples/decision-doc-section.md`](../examples/decision-doc-section.md) for S1 whole.
3. **Gated Q&A + register** (loop steps 3–4): the operator reads, then ratifies one-by-one → **D056** (all six, as recommended — but only after reading the runners-up).
4. **Derive**: a different fresh-context agent writes A8-data.md from the register alone; cross-doc amendments go to a ledger, unapplied.
5. **Adversarial gate**: a third agent attacks it → the report behind **D057**, catching among other things that the recommended IAM grant would 404 at genesis (the repo it binds to wouldn't exist yet — create-then-bind reordering).
6. **Sweep**: the ledger executes across five other docs with version bumps.
7. **Ratify**: "continue to 9" → **D058**. One word, and only that word, unblocks Area 9.

**The override culture is what kept the human in charge rather than in the loop.** Overrides are first-class register entries, recorded verbatim with the human's reasoning — and they went *against* the agents' recommendations at load-bearing points:

- **D006 — engine choice**: the research recommended OpenTofu (parity verified: tofu 1.11+ has the ephemeral/write-only features). The operator overrode: *ecosystem majority + zero feature lag* → Terraform ≥ 1.11, with D027 keeping configs engine-neutral so reversal is a pin change. The recommendation, the override, and the escape hatch live in one immutable entry.
- **D038 — SA naming**: the decision doc recommended `tf-exec`/`deployer`/`runtime`; the operator overrode to `sa-terraform-*`/`sa-deploy-*`/`sa-app-*` — self-documenting IDs matching the v1 convention he had operated — recorded as a *documented exception* to the no-embed naming rule, with the reasoning attached: SA emails surface in other projects' IAM bindings and audit logs, where self-documentation pays.
- Overrides also ran in the **more-ambitious** direction: D050·R5 and D059·O6/O7 record the operator overriding "keep it out of v1" recommendations to *ship* the ops-scan job and TF-managed budgets.

Because the register records the road not taken, later work could cite an override's own logic by ID — D044's adjudications invoke "D038 precedent" the way case law cites a holding.

## 4. Four verification saves

What the adversarial gates are for, told properly: the claim, the oracle, the catch, the avoided cost.

### 4.1 The `for_each` that doesn't exist (A2 gate, 2026-06-11 → D031)

**The claim**: the IaC-structure doc's region/service fan-out used `for_each` on `unit` blocks in Terragrunt stack files — clean, plausible, exactly what Terraform-trained intuition writes.
**The oracle**: docs.terragrunt.com blocks reference, per the gate contract (*verify every TG claim against official docs; re-derive, don't confirm*).
**The catch**: `for_each` on `unit`/`dependency` blocks **does not exist in TG 1.0**. The code would not parse. The same gate's companion finding (V2) caught that nested stacks generate a *second* `.terragrunt-stack` path segment — every printed path and state prefix in the doc was wrong.
**The avoided cost**: these two findings changed the on-disk layout and every state key *before any state existed*. Found at first `terragrunt apply`, the fix is an evening; found after three environments exist, it's state-migration surgery across every unit. The register kept the ratified contract (D025: one unit per region) and superseded only the mechanism *wording* — D031 records hand-written unit blocks as the fan-out. Substance and mechanism are separately amendable because they are separately recorded.

### 4.2 The deadline one month out (A4 gate, 2026-06-12 → D044·F1)

**The claim**: WIF trust bindings use subjects of the form `repo:{org}/{repo}:environment:{env}` — the grammar in every corpus source and most of the internet.
**The oracle**: docs.github.com, live-fetched with access date (claim type: time-sensitive external behavior).
**The catch**: GitHub repos created or renamed **on/after 2026-07-15** emit an immutable grammar — `repo:OWNER@ID/REPO@ID:environment:NAME`. If the repo were created after that date, *every* IAM binding would silently never match: CI fails closed, and nothing in any log says "wrong subject grammar."
**The avoided cost**: the review ran 33 days before the deadline, and genesis hadn't run yet — precisely the window where the failure would have been built in and discovered months later as an undiagnosable auth mystery. The resolution standardized on the immutable grammar, which uses the same numeric IDs the design's provider condition already required — the fix *strengthened* the ratified model rather than reopening it, and FREEZE.md §5 carries the date-conditional checklist item forward.

### 4.3 The block that would have killed every PR plan (A7 gate, 2026-06-12 → D054·F1)

**The claim**: PR plan jobs mint OIDC tokens through GitHub environments whose deployment branch policies list the protected branches.
**The oracle**: GitHub Actions documentation, re-derived from first principles: what ref does a `pull_request` run actually carry?
**The catch**: `GITHUB_REF=refs/pull/N/merge` — matching **no** listed branch pattern, so the environment protection gate rejects *before token minting*. Every PR plan in the designed CI, without exception, dead on arrival. Fix: a `refs/pull/*/merge` rule per environment — plus an "honesty sentence" the gate demanded about what that rule does and doesn't expose.
**The avoided cost**: this fails on the first real PR, on the GitHub side, before GCP logs anything — the natural misdiagnosis is a WIF misconfiguration, and the natural "fix" is loosening the trust condition. The same report's adjudication 4 shows the flip side of adversarial review: the author's OIDC permission model was **verified correct** against the docs, and the verification still produced output — two pin-notes that hardened the design (see [`examples/gate-report-excerpt.md`](../examples/gate-report-excerpt.md)). A gate that can only find bugs is a linter; this one also certified load-bearing correctness, with citations.

### 4.4 The half-applied sweep (zoom-out, 2026-06-12 → D062 Finding 1)

**The claim** — this time made by the process about itself: a QA resolution (D051) changed a per-environment IAM role count from 11 to 12, and its executed ledger claimed the sweep covered `A1 §2/§4.3/§7`.
**The oracle**: the disk. The cross-area zoom-out is a fresh-context agent instructed to re-verify *executed ledgers against what the documents actually say*.
**The catch**: A1 §4.3 and §7 were fixed; **§2's prose still said 11, three lines below a diagram saying 12** — with two sweeps' records claiming the line was covered. Whether the applying agent's session crashed partway or silently skipped the edit is unknowable, and didn't matter: **the ledger said what should exist, grep said what did**, and the delta was mechanically provable with no access to any conversation memory. That is the crash-recovery property doing its job (see the coda in [`examples/amendment-ledger.md`](../examples/amendment-ledger.md)).
**The avoided cost**: A1 §2 is the corpus's most-read section. A runbook derived from it provisions an env folder missing `cloudscheduler.admin`, and every new environment's scheduler applies fail. The zoom-out found 16 such findings — *all* propagation debt, *none* reopening a ratified decision. The pattern does not prevent half-applied changes; it makes them detectable, attributable, and cheap to fix before derivation multiplies them.

## 5. What the freeze delivered

**D064, 2026-06-12 — Golden Design v1.0.0**, behind two named gates:

- **Zoom-out passed** (D062): eight cross-doc invariants verified over all nine docs — exactly two sanctioned cross-project reads everywhere, grant sets consistent, unit inventory with zero ghost references, a canonical merged genesis timeline assembled from six docs with no ordering contradictions.
- **Freshness passed** (D063): 16 externally-sensitive claims re-verified against live official docs — **12 confirmed, 4 changed, 0 unresolved**; the four changes landed as fact-fix version bumps, no design reopens.
- **The frozen corpus**: nine area docs at pinned versions (A1 v13 … A9 v2.2), PROCESS.md, 63 register entries, the 95-file archive, 10 QA reports.
- **The register-gated upgrade catalog**: 16 pre-analyzed future changes (canary deploys, read-only plan SA, a fourth SA class…), each with its trigger condition and what it would amend — deferred work with its re-entry path already designed.
- **Change policy**: register supersession only. "Frozen" means amendable on the record, not immutable.

After the freeze, **24 operational runbooks** were derived in one pass under the "zero new decisions" contract — and the contract worked in both directions: derivation surfaced five genuine gaps as *flags* rather than silent fixes, including one real design hole — no role in any grant table could perform a Firestore restore. D067 resolved it by supersession: the role count's fourth amendment (12→11→12→13), each step with its own entry and sweep. The frozen docs never drifted; they were amended, on the record.

## 6. Honest costs

| Cost | Observed |
|---|---|
| Agent sessions | ~6 fresh-context sessions per area (mine, decision doc, derive, gate, sweep, resolutions) × 9 areas + intake/seed + 3 closing passes + runbook derivation ≈ **60+ sessions**; gates and zoom-out are the expensive kind — live doc-fetching and re-derivation, not summarization |
| Human ratification moments | 5 gates × 9 cycles + seed, tooling fork, zoom-out, freshness, freeze, runbooks ≈ **~50 in two days** |
| Human reading | the real cost: decision docs run ~200 dense lines each, design docs 200–310, and the process is built on the human reading *before* being asked anything; ratification itself was often a one-word "continue" — but only because the reading wasn't |
| Residual debt | even with per-item contracts and grep post-conditions, the zoom-out found **16 propagation-debt findings** and seven of nine docs with stale cross-version headers — budget for the zoom-out; it is not optional polish |

**When RGV is overkill**: a corpus that fits in one context window; single-source material with no conflicting generations; decisions that are cheap to reverse; exploratory work where being wrong is how you learn.

**When it pays**: the inputs disagree across time; the decisions are expensive to unwind (state topology, IAM, naming); external deadlines lurk in the details (a grammar change 33 days out); and the work must survive sessions, crashes, and a solo operator's memory — which is to say, when the cost of a silent skip exceeds the cost of fifty gates.
