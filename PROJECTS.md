# Projects

## [~] Project P01: RGV repo v0.1 — docs core + heart skills (v0.1.0)
**Goal**: The pattern documented and the three heart skills authored

### Tests & Tasks
- [x] [P01-T01] docs core: README, pattern, terminology, inputs-and-state, instruction-anatomy
- [x] [P01-T02] Heart skills: rgv (conductor), rgv-cycle, rgv-gate — skill-creator conventions, handoff-block footers (+7 doc ambiguities canonized)
- [x] [P01-T03] Case study (docs/case-study.md) + examples/ (4 genericized artifacts)
- [ ] [P01-TS01] Skill dry-read: each SKILL.md's trigger description unambiguous; state contracts match inputs-and-state.md

## [~] Project P02: Remaining skills (v0.2.0)
- [x] [P02-T01] rgv-intake, rgv-seed, rgv-sweep, rgv-close, rgv-derive
      Written 2026-06-12: each with SKILL.md (state contract, 🧑 gates, handoff footer) +
      references/ (7 files: manifest/index templates, meta-decision menu, sweep contract,
      invariant checklist, freeze template, runbook shape). Conductor sharpened: close-pass
      mid-position detection, within-phase-mechanics note, derive/intake/seed gate pendings.
- [x] [P02-TS01] Conductor routing table covers all seven + gate boundaries verified (self-gate)

## [~] Project P03: Packaging + publish (v1.0.0)
- [x] [P03-T01] dist/*.skill zips (8; pre-zip contract-diff check caught + fixed 1 divergence); README install section pending push
      Pre-zip check: diff the duplicated reference contracts (rgv-cycle vs rgv-gate
      gate-contract.md; rgv-cycle vs rgv-sweep sweep-contract.md) — copies must not
      drift except by documented scope wording (self-gate F2 ruling)
- [~] [P03-T02] RGV-gate the repo itself (eat own cooking) → publish smorinlabs
      Self-gate run 2026-06-12: docs/self-gate-report.md — 16 findings, 5 flag
      adjudications, mechanical fixes applied; 1 genuine-choice item left to the human
      (freshness contract template). Publish remains.

## [~] Project P04: Gate-zero readiness system (v0.3.0)
**Goal**: Conductor + per-skill entry criteria; REJECTED = hard block passable only by a
recorded register override; verdicts written to READINESS.md at the target repo root;
repo stays the plugin source

### Tests & Tasks
- [x] [P04-T01] references/entry-criteria.md for all 7 phase/mechanic skills
      (MUST-HAVES + STRENGTHENERS derived from inputs-and-state.md + each skill's
      real failure modes)
- [x] [P04-T02] references/gate-zero-verdicts.md — canonical verdict semantics
      (READY / IMPROVABLE / REJECTED + recorded-override rule), byte-identical copy
      in all 8 skills' references/
- [x] [P04-T03] SKILL.md "Step 0 — GATE ZERO" blocks (byte-identical across the 7
      skills) + conductor Step 2 routes through gate zero (detect → pick target →
      entry criteria → verdict → route only on READY/IMPROVABLE or recorded override)
- [x] [P04-T04] Docs: pattern.md §4 "Gate zero (entry criteria)" subsection + §5
      garbage-inputs failure mode; instruction-anatomy.md §6 READINESS.md block
      format; terminology.md 4 new terms; inputs-and-state.md READINESS.md
      contract-wide artifact + gate-zero stop rule
- [x] [P04-TS01] Diff check: gate-zero-verdicts.md identical across all 8 skills
      (shasum match); SKILL.md Step 0 blocks identical across all 7 (extracted-diff)
- [ ] [P04-T05] Re-zip dist/*.skill with the gate-zero files (rides with P03 publish;
      pre-zip contract-diff check now also covers gate-zero-verdicts.md copies)
