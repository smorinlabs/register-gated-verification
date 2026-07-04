---
name: rgv-sweep
description: Ledger application — a fresh-context agent executes an already-decided change set (an amendment ledger plus register-backed resolutions) across every affected document, with version bumps, one-line reasons citing D-IDs, and grep post-conditions proving application. Use when the user says "apply this ledger", "apply these resolutions", "execute the amendments", "sweep the docs", or when rgv-cycle (step 7) or rgv-close invokes it after a gate; the sweep decides nothing and has no human gates — it is mechanical and exits report-only. Requires a resolutions ledger and the register entries authorizing it.
---

# RGV Sweep — ledger application

The sweep is the EXECUTE half of the two-phase commit for documents: a derivation or gate
LISTED the amendments; the sweep applies them and PROVES it with grep post-conditions. It
carries zero discretion — the ledger and the register-backed resolutions are authoritative,
and anything that does not apply cleanly comes back flagged, never improvised around.

## State contract

Requires on disk: a resolutions ledger (amendments with exact before → after) · the
register entries (D-IDs) authorizing the change set · the gate report, when the sweep
follows a gate. Missing the ledger or its authorizing entries → **STOP and route to the
conductor (`rgv`)** — a sweep without written authority is just editing.

Leaves on disk: the edited documents, each version-bumped with a changelog line citing
D-IDs · recorded grep evidence (expected vs actual, verbatim). The sweep writes no report
file of its own; its report returns to the caller.

## Step 0 — GATE ZERO (entry-criteria check)

Before anything else: evaluate `references/entry-criteria.md` — every MUST-HAVE and every
STRENGTHENER gets a per-criterion PASS/MISS — then issue the verdict per
`references/gate-zero-verdicts.md` (READY / IMPROVABLE / REJECTED) and write the run's
block to `READINESS.md` at the target project's repo root. On REJECTED: **HARD STOP** —
name each missing must-have and exactly what to assemble; proceed only on a recorded
register override. Never silently proceed on bad inputs.

## No gates here — and why

The sweep is within-phase mechanics: every judgment it executes was already made at a
gate. It therefore never asks the human anything. The human checkpoint that FOLLOWS a
sweep — the forest check (rgv-cycle GATE 4) or a closing pass's ratification — belongs to
the caller, not to this skill. Exit is report-only.

## Running the sweep

1. **Collect the inputs**: ledger path (usually a section of the area design doc or a gate
   report's fix list) · the authorizing D-IDs · the gate report if one exists · the
   EXHAUSTIVE list of documents the change set touches · the grep post-condition list
   (from the ledger's own claims — every "X now says Y everywhere" claim implies a grep:
   pattern + files + expected count).
2. Fill `references/sweep-contract.md` (placeholders: {LEDGER}, {NEW_IDS}, {DOC_LIST},
   {GREP_LIST}, …) and launch a **fresh-context agent** with it, verbatim. Fresh context
   is not optional: an agent that argued for an amendment will bend its application.
3. Receive the report and relay it whole (see exit).

## The rules the contract enforces

- **The ledger is authoritative — no discretion.** Apply exactly what is listed and
  resolved; nothing more, nothing less, no ride-along "improvements".
- **Version bumps with reasons**: every touched doc gets a version bump and a one-line
  changelog citing the authorizing D-ID(s). Fact-fix-class changes are patch bumps.
- **PRESERVE ratified-status lines**: status/ratification annotations in the target docs
  ("ratified", "RESOLVED per D0NN", version-history lines) survive edits untouched unless
  a ledger entry explicitly amends them — sweeps update content, not ratification history.
- **Grep post-conditions prove application**: run EVERY grep, record expected vs actual
  verbatim. A grep miss is a finding, not a thing to quietly fix.
- **Flag, don't improvise**: an amendment whose before-string is absent, whose context has
  drifted, or which turns out bigger than a fact-fix is left UNAPPLIED and flagged with
  what was found instead. Register entries are never edited — a needed register change is
  a flag for the caller (supersession is the caller's, ultimately the human's, act).

## Exit — report-only

Relay to the caller (or user), per item:

- **Per-file edits**: file · section · before → after applied · new version.
- **Grep results**: each pattern · files · expected · actual — verbatim, including passes.
- **Not applied cleanly**: each flagged amendment — what the ledger said, what the doc
  actually contained, left unapplied.

A sweep with flags is not failed — it is a sweep whose ledger met a drifted world, caught
exactly as the two-phase design intends.

## Handoff

```
STATE: ledger executed — N docs edited + bumped, M greps recorded (E expected-met, F flagged)
NEXT:  <caller's next gate — rgv-cycle GATE 4 forest check, or the closing pass's ratification;
       standalone: rgv conductor> — the human checkpoint after a sweep belongs to the caller
       (crosses a human gate → say "continue" or invoke directly)
```
