# Gate-zero verdicts — canonical semantics

The three readiness verdicts, stated once. This file is copied byte-identically into every
RGV skill's `references/` (drift is a packaging defect — the copies are diff-checked
pre-zip). Rationale and the lifecycle statement live in docs/pattern.md §4 (Gate zero).

Gate zero evaluates the skill's `references/entry-criteria.md` — MUST-HAVES and
STRENGTHENERS, each earning a per-criterion PASS or MISS — and issues exactly one verdict:

- **READY** — every criterion passes. State WHY (which criteria passed), then begin.
- **IMPROVABLE** — every must-have passes; one or more strengtheners miss. Beginning is
  allowed: state what would strengthen the run, record it in READINESS.md, then begin.
- **REJECTED** — any must-have misses. **HARD STOP.** Name each missing must-have and
  exactly what to assemble to satisfy it. The human MAY override; the override is WRITTEN
  TO THE REGISTER as an override entry carrying their reasoning verbatim — proceeding on
  bad inputs is itself a decision worth recording. No recorded entry, no proceeding.

Whatever the verdict, write the run's block to `READINESS.md` at the TARGET project's repo
root (format: docs/instruction-anatomy.md §6): date · skill · verdict · per-criterion
PASS/MISS table · guidance · override record if any. Re-runs append; never silently
proceed.
