# Terminology

**Register-Gated Verification (RGV)** — a consolidation-and-design method whose two load-bearing mechanisms are (1) an append-only **decision register** every artifact cites, and (2) **human ratification gates** no phase advances past. Verification is per-item, multi-phase (each artifact level gets its own pass), and multi-pass (each pass has a distinct oracle).

## The activities and their established names

| RGV activity | Established name | Field |
|---|---|---|
| Numbered, immutable, supersede-only decision entries | **Architecture Decision Records (ADR)**, append-only / event-sourced | software architecture |
| Artifacts cite decision IDs; every change traces to authority | **Requirements traceability** (RTM applied to design docs) | systems engineering |
| Author-agent ≠ reviewer-agent; reviewer instructed adversarially | **Independent Verification & Validation (IV&V)**; four-eyes principle | aerospace/NASA |
| Decision docs: cold context + evidence + options + recommendation + runner-up BEFORE any question | **Completed staff work**; **trade study** | management / SE |
| Nothing advances past an unratified human gate | **Stage-gate (phase-gate) process**; the register as **change control board** log | product/process mgmt |
| Distinct passes checking distinct authorities (internal consistency → external docs → currency) | multiple **test oracles**; the **V-model** pairing of artifact and verification levels | software testing / SE |
| Amendments LISTED in a ledger, applied separately, grep-checked | **two-phase commit** for documents + **post-condition checks** | databases / testing |
| "Flag, don't resolve" | **separation of detection from adjudication** | audit |
| Fresh-context agents reading only the repo | **clean-room review** (verifies what's written, not what was meant) | software law/QA |
| Evidence ordered chronologically; latest position presumptively evolved | **recency weighting** with overridable presumption | RGV-specific |
| The register governing its own process rules | **self-hosted process** ("the process is data, not folklore") | RGV-specific |

## Core RGV terms

- **Register**: the append-only decision log (Dnnn entries). Entries are immutable; change = supersession.
- **Gate**: a point where a named human must ratify before anything advances. Five per cycle; the full lifecycle census (intake 2, seed's interview, close 3, derive 1) is in pattern.md §4. Distinguish the **adversarial gate** (the IV&V review pass, rgv-gate, producing a *gate report*) — an agent activity named for the human gate it feeds (cycle gate 3), not itself a ratification point.
- **Oracle**: the authority a claim is verified against. Assigned by claim type (see instruction-anatomy).
- **Decision document**: the completed-staff-work artifact — every open question with context/evidence/options/rec/runner-up. Questions are asked ONLY from it.
- **Derivation**: an artifact produced purely from ratified decisions (design docs, runbooks). Zero new decisions; deviations are flags.
- **Ledger**: a list of cross-document amendments — a derivation's ledger section or a gate report's accepted fix list — listed, not applied; execution is a separate, checked step.
- **Sweep**: applying a ledger across the corpus with version bumps and grep post-conditions.
- **Handoff block**: the standard end-of-skill state statement naming the next skill; the user's word crosses the gate.
- **Override**: a human decision against the recommendation — first-class, recorded verbatim with reasoning.
- **Freeze**: the immutability boundary; after it, docs change only via register supersession.
- **Entry criteria**: a skill's gate-zero checklist (`references/entry-criteria.md`) — MUST-HAVES (any miss ⇒ REJECTED) and STRENGTHENERS (miss ⇒ IMPROVABLE). The stage-gate term, used in its stage-gate sense: conditions verified before a phase may begin.
- **Gate zero**: the readiness check every skill runs before its first step — and the conductor runs for the target skill before routing. Not a human ratification gate: a mechanical entry check whose only human moment is the override.
- **Readiness verdict**: gate zero's output — READY / IMPROVABLE / REJECTED — written to READINESS.md with a per-criterion PASS/MISS table. Canonical semantics: each skill's `references/gate-zero-verdicts.md`.
- **Recorded override**: the only way past a REJECTED verdict — a register entry carrying the human's decision to proceed despite missing must-haves, reasoning verbatim. Proceeding on bad inputs is itself a decision worth recording.
