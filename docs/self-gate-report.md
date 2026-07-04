# Self-Gate Report — RGV applied to the RGV repo

**Target**: the whole repository (README, PROJECTS.md, docs/ ×5, examples/ ×4, skills/ ×8 + 12 reference files) · **Reviewer**: fresh-context agent, not any author · **Oracles**: the repo against itself (docs ↔ skills ↔ examples), per the repo's own instruction-anatomy; hygiene grep; file-existence checks; `diff` on duplicated contracts · **Date**: 2026-06-12 (P03-T02, eat-own-cooking)

**Verdict up front**: **publish-ready after fixes** — 16 findings (2 HIGH / 6 MEDIUM / 8 LOW), zero contradictions of the pattern's substance, all HIGHs mechanical. Mechanical fixes applied below; one genuine choice left to the human (Finding 7).

---

## Findings

### HIGH

**1. rgv-gate's SKILL.md contradicted the standalone relaxation the docs grant it.**
File: `skills/rgv-gate/SKILL.md` (state contract) vs `docs/inputs-and-state.md`.
Quote (skill): "Missing any required state → **STOP and route to the conductor (`rgv`)** rather than improvising." Quote (docs): "**Standalone relaxation**: outside an RGV repo, rgv-gate runs report-only against an ad-hoc oracle list supplied by the user — no register required, no sweep implied."
A user invoking rgv-gate outside an RGV repo — the exact use its own description advertises ("adversarially review X against Y") — would be refused by the skill while sanctioned by the docs. Fix: carry the relaxation into the skill's state contract. **APPLIED.**

**2. The sweep-contract copies diverged on a load-bearing clause (P02 flag F2).**
File: `skills/rgv-cycle/references/sweep-contract.md` vs `skills/rgv-sweep/references/sweep-contract.md`. `diff` confirms: rgv-cycle's copy lacked "PRESERVE ratified-status lines: status annotations, 'RESOLVED per D0NN' markers, and version-history lines … survive your edits untouched" — present in rgv-sweep's copy AND stated as a rule in rgv-sweep/SKILL.md. The unprotected copy is the one on the most-travelled path (every cycle's step 7). Fix: backport (see Adjudication F2). **APPLIED.**

### MEDIUM

**3. pattern.md §4's conductor route summary omitted intake.**
File: `docs/pattern.md` §4. Quote: "routes: no register → seed; areas remaining → cycle; all ratified → close; frozen → derive" — but the conductor's own first check and route (`skills/rgv/SKILL.md` step 1/2) is "no archive/ … → **rgv-intake**", and §1's lifecycle diagram starts at INTAKE. Fix: add "no archive → intake". **APPLIED.**

**4. The full gate census was undocumented (P02 flag F4).**
File: `docs/pattern.md` (names only the cycle's 5 gates) and `docs/terminology.md` (Quote: "**Gate**: … Five per cycle."). The skills implement more: intake 2 (scope, grouping) · seed (interview, human-paced per item) · close 3 (one per pass) · derive 1 (flag adjudication) — the README table even lists them, but no docs/ file did. Fix per Adjudication F4: census table in pattern.md §4 + terminology cross-ref. **APPLIED.**

**5. "Gate" was overloaded without disambiguation.**
File: `docs/terminology.md`. "Gate" is defined only as the human ratification point, yet rgv-gate, "gate report", and "adversarial gate" use it for the IV&V review pass — an agent activity with no ratification in it (its human moments belong to cycle gate 3). Fix: one disambiguating sentence in the Gate entry. **APPLIED.**

**6. Register status vocabulary diverged between skill and exhibit.**
File: `skills/rgv-cycle/SKILL.md` step 4. Quote: "status (proposed | ratified | overridden | superseded-by-Dnnn)" vs `examples/register-slice.md` legend ("**Status** — `ratified` | `superseded-by-D###`") and the exhibited override D006, which carries status **ratified**; the meta-decision-menu entry shape has no "overridden" either. Per terminology.md an override is recorded content, not a status. Fix: drop "overridden" from the enum; state that overrides are ratified entries recording the overridden recommendation verbatim. **APPLIED.**

**7. The freshness pass is the only delegated agent step with no contract template.**
File: `skills/rgv-close/SKILL.md` Pass 2 vs `skills/rgv-close/references/` (contains only invariant-checklist.md + freeze-template.md). Quote: "Launch a **fresh-context agent** whose contract binds it to the sanctioned-oracle list…" — every other delegated step says "Fill `references/<x>.md` and launch". Fix options: (a) author `references/freshness-contract.md` to the six-part anatomy, or (b) declare Pass 2's inline contract deliberate. Authoring a new contract is content work, not a mechanical fix — **LEFT FOR THE HUMAN** (recommendation: option (a), for uniformity and packaging completeness).

**8. The two-phase-ledger exception was exercised but never sanctioned in docs (P02 flag F5).**
File: `docs/pattern.md` (silent) vs `skills/rgv-close/SKILL.md` Pass 2. Quote (skill): "for CHANGED items: the fact-fix — applied directly with a PATCH bump citing the pass". Direct application bypasses list-then-sweep; the source project did exactly this (D063: 4 changes as fact-fix patch bumps). Fix per Adjudication F5: document the exception with its guardrails. **APPLIED.**

### LOW

**9. inputs-and-state.md understated rgv-sweep's Requires.**
Quote: "| rgv-sweep | a resolutions ledger |" — the skill also requires "the register entries (D-IDs) authorizing it" ("a sweep without written authority is just editing"). Fix: table row. **APPLIED.**

**10. FREEZE.md's watch list missing from two summaries.**
Files: `docs/inputs-and-state.md` (rgv-close row: "FREEZE.md (manifest + gated-upgrade catalog + change policy)") and `skills/rgv/SKILL.md` check 5 (same parenthetical) — the freeze template has five sections including §4 watch list, and rgv-close's own description names it. Fix: add "+ watch list"; conductor check also now requires the freeze register entry (matching rgv-close: "An unratified FREEZE.md is a draft"). **APPLIED.**

**11. Gap-list home ambiguous (P02 flag F1).**
Files: `docs/inputs-and-state.md` ("archive/ + MANIFEST + INDEX + gap list" — reads as a fourth artifact; §1 prose likewise) and `skills/rgv/SKILL.md` check 1 ("MANIFEST …, INDEX, and gap list") vs the intake skill + index-template, which make it INDEX's required §Gaps section. Fix per Adjudication F1: reword docs + conductor to "INDEX (incl. gap list)". **APPLIED.**

**12. terminology.md's Ledger definition narrower than actual usage.**
Quote: "a derivation's list of cross-document amendments" — but rgv-sweep also executes "a gate report's fix list" and its description says "an amendment ledger plus register-backed resolutions". Fix: broaden to derivation ledger *or* gate-report fix list. **APPLIED.**

**13. README status stale.**
Quote: "v0.1 — docs core + heart skills under construction" — all eight skills were authored 2026-06-12 (PROJECTS P02-T01 ✔), and the README's own table already lists all eight. Fix: status line updated to v0.2-pre + self-gate pointer. **APPLIED.**

**14. Case-study miniature: 7 numbered items for an "8-step loop".**
File: `docs/case-study.md` §3 — the register step is silently folded into item 3. Fix: relabel item 3 "Gated Q&A + register (loop steps 3–4)". **APPLIED.**

**15. Conductor imprecise about who invokes rgv-gate.**
File: `skills/rgv/SKILL.md`. Quote: "invoked by rgv-cycle/rgv-close mid-phase" — rgv-close invokes only the sweep; its reviewers run from its own checklists, never rgv-gate. Fix: "the gate by rgv-cycle (step 6), the sweep by rgv-cycle (step 7) or rgv-close". **APPLIED.**

**16. inputs-and-state.md's rgv-derive row weaker than the skill (and the skill is right).**
Quote (docs): "| rgv-derive | FREEZE.md |" vs skill: "FREEZE.md with its ratifying register entry". Fix: docs row strengthened to match. **APPLIED.**

---

## Adjudications of the five P02 flags (decision-style)

### F1 — gap list: separate artifact vs INDEX §Gaps?
**Premise verdict**: valid — docs table/conductor and the intake skill genuinely disagreed on the gap list's home.
**Options**: (a) INDEX §Gaps section, docs reworded · (b) separate GAPS.md artifact.
**Recommendation — (a) INDEX §Gaps**: it is what the shipped skill and index-template implement; the gap list's consumers (decision-doc authors: "thin legs get researched BEFORE presenting") read the INDEX anyway; one fewer artifact for the conductor's state detection; the template already handles gap lifecycle (dated FILLED strike-throughs).
**Runner-up — (b) GAPS.md**: more visible as a standing research backlog and independently greppable — loses because it adds a file every skill and the conductor must track, for content that is narratively part of the corpus's story.
**Ruling applied**: (a) — docs/inputs-and-state.md + rgv/SKILL.md reworded (Findings 11).

### F2 — duplicated contract copies: backport vs single-shared-reference vs accept divergence?
**Premise verdict**: confirmed by diff — sweep copies diverged on the PRESERVE clause (substantive) plus three placeholder-table wordings (scope-appropriate: cycle copy is cycle-scoped, e.g. its gate report always exists so "(omit the read item if none)" rightly appears only in the standalone copy). Gate-contract copies: **byte-identical today** — no defect, only a drift risk.
**Options**: (a) backport the PRESERVE clause; keep per-skill copies; add a pre-packaging diff check · (b) single shared reference file · (c) accept divergence.
**Recommendation — (a)**: skills package as independent `dist/*.skill` zips (P03), so cross-skill file references cannot be relied on at install time; the principle is *divergence-by-scope is legitimate, divergence-by-drift is the defect* — fix the drift, institutionalize the diff.
**Runner-up — (b) single shared reference**: eliminates drift permanently and is the right answer if packaging ever bundles the family as one artifact — loses today because it breaks the per-skill install model.
(c) rejected: the PRESERVE clause protects ratification history on the most-travelled sweep path.
**Ruling applied**: backported (Finding 2); pre-zip diff check added to PROJECTS.md P03-T01. Same ruling governs the gate-contract pair (currently in sync).

### F3 — sweep standalone relaxation: grant one (like rgv-gate's) or keep register-authorization-always?
**Premise verdict**: valid question; asymmetry is real but principled.
**Options**: (a) keep register-authorization-always · (b) narrow relaxation: outside an RGV repo, a user-supplied ad-hoc ledger counts as written authority.
**Recommendation — (a) keep, no relaxation**: rgv-gate's relaxation is safe because the gate is *read-only, report-only* — it can run anywhere and damage nothing. The sweep *edits documents*; its own contract states the principle: "a sweep without written authority is just editing." A relaxation would delete the authority half of the two-phase commit exactly where harm is possible, and would invite using rgv-sweep as a generic batch editor.
**Runner-up — (b) ad-hoc-ledger authority**: defensible — the ledger's exact before→after plus grep post-conditions still bind, and the discipline might be worth exporting to non-RGV repos. Loses because authority-from-the-register is the thing the skill teaches; blurring it costs more than the convenience gains.
**Ruling**: status quo — **no change applied** (and none needed: the docs and skill already agree here).

### F4 — gate census: add to pattern.md §4, or a new docs section?
**Premise verdict**: confirmed — the lifecycle's non-cycle gates (intake 2 / seed interview / close 3 / derive 1) existed only in skills and the README table; docs/ was silent, and terminology.md's "Five per cycle" read as the whole story.
**Options**: (a) census table in pattern.md §4 · (b) new docs section/file.
**Recommendation — (a) pattern.md §4**: §4 is the orchestration home — the chaining rule and the conductor already live there, and every gate is precisely a point where chaining stops; a table's worth of content does not justify a new file.
**Runner-up — (b) new section**: room for per-gate prose — loses as fragmentation; the skills themselves are the per-gate prose.
**Ruling applied**: census table in pattern.md §4 + terminology.md Gate entry cross-ref (Finding 4).

### F5 — freshness fact-fix exception: document in pattern.md?
**Premise verdict**: confirmed — rgv-close Pass 2 applies CHANGED fact-fixes directly (patch bump citing the pass), bypassing list-then-sweep; deliberate, with a track record (source project D063: 4 changes landed exactly this way, 0 design reopens).
**Options**: (a) document the sanctioned exception with guardrails · (b) make freshness conform — emit a ledger, route through rgv-sweep.
**Recommendation — (a) document**: the exception is safe by construction — fact-class only (ratified mechanisms still FLAG, never fix), per-item enumeration in the pass report (the report *is* the ledger; detection is preserved), and the pass's own ratification gate reviews the list before it stands. What was missing was the sanction, not the safety.
**Runner-up — (b) conform**: uniform discipline, no exceptions to teach — loses because it adds a full sweep session per close to re-apply facts the freshness agent just verified against the live oracle, ceremony without a detection gain.
**Ruling applied**: exception paragraph in pattern.md §3 + cross-ref sentence in rgv-close Pass 2 (Finding 8).

---

## What's solid (verified, enumerated)

- **Conductor coverage**: routing table covers all 7 skills — 5 as destinations, gate/sweep explicitly refused as destinations with their real invocation paths; the NEVER-skip-a-gate section enumerates every pending-gate state including close passes, derive flags, and intake/seed interview gates. Mid-cycle and mid-close position-fixing orders match the 8-step loop and 3-pass close exactly.
- **State contracts**: rgv-seed, rgv-cycle, rgv-close rows in inputs-and-state.md match their SKILL.md Requires/Leaves verbatim in substance (divergences found were all in the other rows, and all fixed above).
- **Chaining rule**: pattern.md §4, README, conductor, and all 8 skill footers agree; every skill ends in the canonical 3-line handoff block from instruction-anatomy §5, verbatim shape.
- **Gate-contract copies**: byte-identical (diff-verified).
- **Six-part contract anatomy**: mining, decision-doc, derivation, gate (×2), sweep (×2), runbook templates all carry READ / BINDING CONSTRAINTS / PER-ITEM OUTPUT CONTRACT / ORACLE ASSIGNMENT / ANTI-BEHAVIORS / EXIT+REPORT. (invariant-checklist folds binding-constraints/oracle into its intro — acceptable, noted, not a finding.)
- **Oracle taxonomy**: the 8-row claim-type table is identical in instruction-anatomy §3, rgv-gate SKILL.md, and both gate-contract copies.
- **References resolve**: all 12 `references/` files named by skills exist; all README/docs/case-study relative links resolve; all four exhibits exist and match the shapes the docs claim (decision-doc's six legs · gate report's F-n + severity + quote + fix, adjudication-with-runner-up, and enumerated Solid list · register entry's ID/area/decision/status/sources/date with all four species · executed ledger heading + grep post-conditions + the slipped-line coda).
- **Numbers cross-check**: 95 sources, 68 entries (63 at freeze), 10 QA reports, 24 runbooks, 5+1 questions in A8, D-ID arcs (D002→D013→D023/D030; D006/D027; D054; D062/D063/D064) consistent across README, case-study, and exhibits.
- **Hygiene**: zero private tokens/org names — no "banksheets"/"getbanksheets"/personal identifiers anywhere in tracked content; only sanctioned generic vendor domains (docs.github.com, docs.terragrunt.com, cloud.google.com); app genericized as "exampleapp"; `smorinlabs` appears once in PROJECTS.md as the owner's own publish target (sanctioned). `dist/` exists, empty, pre-publish — matches README's "when published".

## Verdict

**Publish-ready after fixes.** MUSTS were Findings 1–2 (a skill refusing a sanctioned use; a load-bearing protection missing from the busiest contract copy) — both applied. Everything else was docs-lagging-skills, one vocabulary drift, and phrasing. No finding touched the pattern's substance: the five mechanisms, the 8-step/5-gate loop, the chaining rule, and the exhibits survived adversarial re-derivation intact. Remaining before v1.0.0 publish: Finding 7 (freshness contract template — human's call), P03-T01 packaging (with the new pre-zip diff check), and a human read of this report — which is, per the pattern, its own gate. 🧑

## Applied (the mechanical-fix record)

| # | File | Edit |
|---|---|---|
| 1 | skills/rgv-gate/SKILL.md | added the standalone-relaxation paragraph to the state contract (Finding 1) |
| 2 | skills/rgv-cycle/references/sweep-contract.md | backported the 3-line PRESERVE-ratified-status-lines clause (Finding 2 / F2) |
| 3 | docs/pattern.md §4 | conductor route line gains "no archive → intake" (Finding 3) |
| 4 | docs/pattern.md §4 | full gate-census table + gate/sweep note added (Finding 4 / F4) |
| 5 | docs/pattern.md §3 | sanctioned freshness fact-fix exception documented with guardrails (Finding 8 / F5) |
| 6 | docs/terminology.md | Gate entry: lifecycle census cross-ref + adversarial-gate disambiguation (Findings 4, 5) |
| 7 | docs/terminology.md | Ledger definition broadened to include gate-report fix lists (Finding 12) |
| 8 | docs/inputs-and-state.md | intake row + §1 prose: gap list = INDEX section (Finding 11 / F1) |
| 9 | docs/inputs-and-state.md | sweep row Requires + close row watch list + derive row ratified-freeze (Findings 9, 10, 16) |
| 10 | skills/rgv/SKILL.md | check 1 gap-list wording; check 5 watch list + freeze entry; gate/sweep invocation precision (Findings 11, 10, 15) |
| 11 | skills/rgv-cycle/SKILL.md | step-4 status enum aligned; override = ratified entry with verbatim record (Finding 6) |
| 12 | skills/rgv-close/SKILL.md | Pass 2 cross-ref to the sanctioned exception (Finding 8 / F5) |
| 13 | README.md | Status section updated to v0.2-pre with self-gate pointer (Finding 13) |
| 14 | docs/case-study.md | miniature item 3 relabeled "Gated Q&A + register (loop steps 3–4)" (Finding 14) |
| 15 | PROJECTS.md | P03 → [~]; P03-T02 progress note; pre-zip contract-diff check added to P03-T01 (F2 ruling) |

**Left unapplied, flagged to the human**: Finding 7 — author `skills/rgv-close/references/freshness-contract.md` (recommended) or declare Pass 2's inline contract deliberate. F3 ruling requires no edit (status quo affirmed).
