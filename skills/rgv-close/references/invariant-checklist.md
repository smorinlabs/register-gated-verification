# Zoom-out invariant checklist — fresh-context reviewer prompt template

Fill every {PLACEHOLDER}, then launch a NEW agent (no conversation history) with the
template below as its complete task. Pass it verbatim; do not summarize it.

| Placeholder | Fill with |
|---|---|
| {PROCESS_PATH} | path to PROCESS.md |
| {REGISTER_PATH} | path to DECISIONS.md (state the entry range, e.g. D001–D061) |
| {DOC_LIST} | every design doc at its CURRENT version — list doc + version explicitly |
| {NAMED_FLAGS} | parked flags addressed to the close, one ID each (F-a, F-b, …), verbatim |
| {INVARIANTS} | the project's cross-cutting invariants, enumerated (I-1, I-2, …) — every count, role set, topology rule, or contract stated in more than one doc |
| {REPORT_PATH} | where the zoom-out report goes |

---

You are the ZOOM-OUT REVIEWER. Every area passed its own gate; your job is what no
single-area gate could see — whole-corpus integrity. Internal consistency ONLY: your
oracles are the documents and the register themselves. Do NOT verify against the outside
world; that is the freshness pass's job, with its own oracles.

READ (in order):
1. {PROCESS_PATH} — the process ground rules.
2. {REGISTER_PATH} — the full range; ratified entries are the conformance oracle.
3. {DOC_LIST} — every doc, whole. Decision docs / mining notes / QA reports are frozen
   historical records: context only, never audit targets.

PER-ITEM OUTPUT CONTRACT — four sweeps, then flags, then findings:

1. DANGLING-POINTER SWEEP: trace EVERY forward/backward pointer ("→ area N", "resolved
   in", "handed to", "see §") in {DOC_LIST} to its target. Per pointer class: resolved ✓
   or an orphan finding. State the census: pointers traced, orphans found.
2. CROSS-DOC CONTRADICTION HUNT: for EACH invariant in {INVARIANTS}, check every doc that
   states it and give a per-invariant verdict: PASS (all statements agree — list the
   agreeing homes) or FAIL (quote both sides). Recompute counts yourself — a stated "12"
   is checked by counting the 12, not by finding another "12".
3. CENSUS CHECKS: for EACH enumerable inventory (units, keys, roles, entries), rebuild the
   full inventory from its canonical table and sweep all docs for members — list any
   orphans or phantoms.
4. SINGLE-SOURCE-OF-TRUTH AUDIT: for EACH fact defined in more than one doc, name the
   canonical home; duplicates either match it exactly or become findings. Flag any fact
   with NO canonical home.
5. NAMED-FLAG VERDICTS: for EACH flag in {NAMED_FLAGS}, verify its premise first, then:
   verdict-class flags get PASS/FAIL + defects; decision-class flags get a decision-style
   adjudication — verdict on the premise · options with pros/cons · recommendation + why ·
   runner-up + why. Never resolve one yourself.
6. FINDINGS: for EACH defect, F-n · severity (HIGH/MEDIUM/LOW) · doc/section · short
   quote · concrete fix. Distinguish resolution-class defects from annotation-class
   (stale markers, missing RESOLVED notes) — both are findings.

ANTI-BEHAVIORS: do NOT fix anything. Do NOT reopen ratified decisions — a D-entry that
seems wrong is a HIGH finding addressed to the human. Do NOT verify externally. Do NOT
sample when you can enumerate — "spot-checked" is a verdict qualifier, not a default.

EXIT CRITERIA: every invariant has a verdict; every named flag has a verdict or
adjudication; every sweep states its census numbers. Write the report to {REPORT_PATH}
with: verdict up front (freeze-ready / freeze-ready after fixes / not freeze-ready, with
counts by severity) · the five sweep sections · findings list · a consolidated
deferred-item list (every escape hatch, declined option, and register-gated deferral you
encountered — raw input for the freeze catalog).
REPORT (back to your caller): the verdict · finding counts by severity · per-flag one-line
verdicts · the invariant PASS/FAIL tally.
