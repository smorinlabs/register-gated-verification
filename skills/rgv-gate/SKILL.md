---
name: rgv-gate
description: Standalone adversarial verification (IV&V) — a fresh-context reviewer, independent of the author, re-derives every load-bearing claim in a document against its assigned oracle and produces a per-item gate report with severities, verdicts, and flag adjudications. Use when the user says "gate this document", "adversarially review X against Y", "QA this design doc", "run IV&V on X", or asks to verify a document's claims item by item; also invoked by rgv-cycle at step 6. Requires the target document, the register (DECISIONS.md), and the sanctioned-oracle list.
---

# RGV Gate — adversarial verification (IV&V)

Verify a document the way an independent verifier would: a fresh-context reviewer,
structurally incapable of anchoring on the author's intent, re-derives every load-bearing
claim against its assigned oracle and emits per-item verdicts. Runs standalone on any
document in an RGV project, and as step 6 of rgv-cycle.

## State contract

Requires on disk: the target document · the register (DECISIONS.md) · the
sanctioned-oracle list (a register meta-entry). Leaves on disk: a gate report (findings +
adjudications + verdict). Missing any required state → **STOP and route to the conductor
(`rgv`)** rather than improvising.

**Standalone relaxation** (the sole exception, per docs/inputs-and-state.md): outside an
RGV repo, rgv-gate may run report-only against an **ad-hoc oracle list supplied by the
user** — no register required, no sweep implied. The register-conformance rows of the
claim-type table simply don't fire; everything else (fresh-context reviewer, re-derive,
per-item verdicts, access-dated citations) binds unchanged.

## Step 0 — GATE ZERO (entry-criteria check)

Before anything else: evaluate `references/entry-criteria.md` — every MUST-HAVE and every
STRENGTHENER gets a per-criterion PASS/MISS — then issue the verdict per
`references/gate-zero-verdicts.md` (READY / IMPROVABLE / REJECTED) and write the run's
block to `READINESS.md` at the target project's repo root. On REJECTED: **HARD STOP** —
name each missing must-have and exactly what to assemble; proceed only on a recorded
register override. Never silently proceed on bad inputs.

## IV&V rules (non-negotiable)

- **Reviewer ≠ author.** The review runs in a FRESH-context agent launched from
  `references/gate-contract.md`. If the current conversation authored, edited, or
  substantially discussed the target document, it is disqualified from reviewing it —
  delegate, always. Fresh context verifies what is *written*, not what was *meant*;
  anchoring becomes structurally impossible rather than merely discouraged.
- **Re-derive, don't confirm.** "Verify X" produces agreement; "recompute X yourself and
  flag ANY error" produces findings. The contract is phrased the second way; keep it so.
- **Sanctioned oracles only**, and every verification citation carries an access date
  only — page-update stamps churn; the access date is the evidentiary fact.

## Running the gate

1. Collect the inputs: target doc path · register path · the D-entries the doc claims to
   implement · the oracle list (verbatim from its meta-entry) · related docs (ledger
   targets, cross-consistency partners) · the report path (PROCESS.md's convention, else
   `qa/{AREA-or-doc}-qa-report-{YYYY-MM-DD}.md`).
2. Fill `references/gate-contract.md` and launch the fresh-context reviewer with it,
   verbatim.
3. Receive the report, triage it (below), and hand off.

## Per-item verdict contract (what the reviewer must emit)

Every load-bearing claim is enumerated and assigned an oracle by its claim type:

| Claim type | Oracle |
|---|---|
| Syntax / code shape | the language's parser or validator |
| API surface (fields, resources, enums) | official provider/vendor reference |
| Behavioral ("X happens when Y") | vendor documentation, quoted |
| Arithmetic / limits / counts | recomputation |
| Cross-document consistency | the other document |
| Time-sensitive (versions, deadlines, pricing) | live fetch, access-dated |
| Design conformance | the register |
| Existence / completeness | enumerated sweep ("list any orphans") |

- Verify-items: **CONFIRMED / CHANGED / UNRESOLVED**, each earned by a completed
  verification act, never by plausibility.
- Findings: **F-n · severity (HIGH/MEDIUM/LOW) · file/section · short quote · concrete
  fix** — the quote makes the finding checkable; the fix makes it actionable.
- Author flags: adjudicated in decision style — **verdict on the flag's premise first**
  (a flag can be wrong about the world), then options · recommendation + why · runner-up
  + why.

## The gate report (the output)

A file on disk, always. Sections: header (target + version, oracles used, date) · findings
by severity · flag adjudications · solid (verified) list · verdict.

The **verdict** performs the musts-vs-ride-along triage: MUSTS are findings that block
ratification — an implementer would build the wrong thing, a register conflict, a broken
mechanism. Ride-alongs are improvements that may ship with the sweep or be consciously
waived. The verdict names the musts explicitly ("not ratifiable as-is; musts: F1, F2,
FLAG-1 text into §9") so the next step is mechanical to read.

## Handoff — two channels out

- **Mechanical fixes** (fact, wording, plumbing, ledger completeness — the musts plus
  accepted ride-alongs): proceed to the sweep (`rgv-sweep`, or rgv-cycle step 7)
  directly — within-phase mechanics need no permission.
- **Genuine choices** (flag adjudications, findings that touch a ratified D-entry,
  anything with design discretion): 🧑 these return to the human, each presented with its
  adjudication's verdict / options / recommendation / runner-up. **STOP — wait for the
  user.** Overrides are recorded verbatim with the human's reasoning; every resolution
  becomes a register entry — supersede, never edit.

End with the handoff block:

```
STATE: <what is now true, with register/doc refs>
NEXT:  <named skill or gate> — <one-line why>
       (crosses a human gate → say "continue" or invoke directly)
```
