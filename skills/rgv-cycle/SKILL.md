---
name: rgv-cycle
description: Run one full Register-Gated Verification cycle — 8 steps, 5 human gates — on a single area or topic; mine the corpus, author the decision document, run gated Q&A, register decisions, derive the area design doc, adversarially gate it, sweep the ledger, and obtain ratification. Use when the user says "run the cycle for X", "start area X", "do the next area", names an area to consolidate or design, or when the rgv conductor routes here with an area. Requires a seeded register (DECISIONS.md), PROCESS.md, and all prior areas ratified.
---

# RGV Cycle — one area, 8 steps, 5 human gates

The core loop turns corpus evidence into ratified register entries and a verified area
design doc. Five human gates punctuate it; nothing advances past an unratified gate.

## State contract (check before step 1)

Requires on disk: the register (DECISIONS.md, seeded with meta-entries) · PROCESS.md ·
all prior areas in the area order ratified. Missing any of these → **STOP and route to the
conductor (`rgv`)** rather than improvising state.

Leaves on disk when complete: mining notes · decision doc · D-entries · area design doc
(ratified) · QA report · executed ledger.

Use PROCESS.md's file conventions where it names them; otherwise default to
`mining/{AREA}-mining-notes.md`, `decisions/{AREA}-decision-doc.md`,
`design/{AREA}-<name>.md`, `qa/{AREA}-qa-report-{YYYY-MM-DD}.md`.

## Step 0 — GATE ZERO (entry-criteria check)

Before anything else: evaluate `references/entry-criteria.md` — every MUST-HAVE and every
STRENGTHENER gets a per-criterion PASS/MISS — then issue the verdict per
`references/gate-zero-verdicts.md` (READY / IMPROVABLE / REJECTED) and write the run's
block to `READINESS.md` at the target project's repo root. On REJECTED: **HARD STOP** —
name each missing must-have and exactly what to assemble; proceed only on a recorded
register override. Never silently proceed on bad inputs.

## Agent discipline (applies to steps 1, 2, 5, 6, 7)

Every delegated step launches a **fresh-context agent** — a new agent with no conversation
history, whose entire world is its contract prompt plus the files the contract names. Fill
in the matching template from `references/` (placeholders like {AREA}, {REGISTER_IDS},
{ORACLE_LIST}) and pass it verbatim as the agent's task. Each template implements the
six-part contract anatomy: read-list · binding constraints · per-item output contract ·
oracle assignment · anti-behaviors · exit criteria + report shape.

Never do author and reviewer with the same context. The step-5 derivation author and the
step-6 gate reviewer must be different agents; if this conversation authored or
substantially shaped an artifact, this conversation may not review it. Fresh context is
what makes the reviewer verify what is *written*, not what was *meant*.

The sanctioned-oracle list, question-format bar, evidence-recency rule, and gate rules are
register meta-entries — read them before filling any contract, and cite their IDs inside
the contracts.

## The eight steps

### Step 1 — Mine (agent, fresh context)
Fill `references/mining-contract.md` and launch. The miner extracts the area's claims,
choices, and conflicts from the archived corpus, each element's evidence ordered
chronologically with provenance (file + date). Latest position = presumptively evolved
thinking, overridable with reason — by a human, not by the miner. Conflicts are recorded,
never resolved. Output: the mining notes file.

### Step 2 — Decision document (agent, fresh context) → 🧑 GATE 1
Fill `references/decision-doc-contract.md` and launch. The document carries EVERY open
question, each with six legs: cold-start background · chronological corpus evidence ·
externally verified practice (cited, access-dated) · options with pros/cons/risks ·
recommendation + why · runner-up + why. Thin legs get researched BEFORE the document is
presented — a question is not ready until its legs hold.

🧑 **GATE 1 — the human reads the document.** Present the file path and a one-line map of
its questions. **STOP — wait for the user.** Ask nothing yet; reading comes first.

### Step 3 — Gated Q&A → 🧑 GATE 2
🧑 **GATE 2.** Walk the document's questions with the human, item by item or in small
batches, in document order. Rules, all binding:

- Questions are asked ONLY from the document — no chat-only questions. If a new question
  surfaces mid-Q&A, it goes INTO the document (new version) before it is asked.
- A question whose evidence is thin carries a **research option**; choosing it loops the
  research back into the document, and the question is re-asked from the updated document.
- Free-form concerns come last, after the document's questions are exhausted.
- **Overrides are first-class.** When the human decides against the recommendation, record
  the override verbatim, with the human's reasoning. No silent normalization.
- Every resolution — accept, override, or defer — feeds step 4 immediately.

This step is human-paced: after each question or batch, **STOP — wait for the user.**

### Step 4 — Register
Every resolution becomes an immutable Dnnn entry in DECISIONS.md: ID · area · the decision
· status (proposed | ratified | superseded-by-Dnnn) · sources · date. An override is a
RATIFIED entry whose decision text records the overridden recommendation and the human's
reasoning verbatim — override is content, not a status. Entries
are never edited — change is a new entry that supersedes the old one and says so. Rejected
alternatives and their reasons live here too; that keeps the design docs clean.

### Step 5 — Derive (agent, fresh context)
Fill `references/derivation-contract.md` and launch. The author writes the area design doc
implementing the register exactly, citing D-IDs inline, with ZERO new decisions. Two hard
disciplines:

- Cross-document amendments are **LISTED in a ledger** (target doc · section · exact
  before → after) — never applied. Application is step 7's job.
- Inconsistencies are **FLAGGED** (premise + what it blocks) — never resolved.

### Step 6 — Adversarial gate (different agent — IV&V) → 🧑 GATE 3
Fill `references/gate-contract.md` and launch a NEW agent — never the step-5 author, never
this conversation. (If the standalone `rgv-gate` skill is installed, invoking it directly
is equivalent — within a phase, mechanics chain without permission.) The reviewer verifies
every load-bearing claim against its oracle by re-deriving, not confirming; emits per-item
verdicts (findings as severity + quote + fix; verify-items as CONFIRMED / CHANGED /
UNRESOLVED); and adjudicates the author's flags decision-style — verdict on the premise,
options, recommendation, runner-up. Output: the QA (gate) report file.

🧑 **GATE 3 — triage the report.** Mechanical fixes (fact, wording, plumbing, ledger
completeness) proceed to step 7 without asking. Genuine choices — flag adjudications,
anything touching a ratified D-entry, anything with design discretion — return to the
human, each presented with its adjudication's options / recommendation / runner-up.
**STOP — wait for the user** on every genuine choice. Each resolution here is registered
per step 4, overrides verbatim.

### Step 7 — Sweep (agent, fresh context) → 🧑 GATE 4
Fill `references/sweep-contract.md` and launch. The sweeper applies the gate resolutions
and EXECUTES the ledger across every affected doc; bumps each touched doc's version with a
changelog line citing D-IDs; then proves application with grep post-conditions (pattern +
files + expected count, run and recorded). Anything bigger than a fact-fix comes back
flagged and unapplied. A half-applied sweep is detectable: the ledger says what should
exist, and grep says what does.

🧑 **GATE 4 — the forest check.** The human reviews the swept area design doc as a whole —
not the diffs, the design. Present the doc path, its version, and the grep evidence.
**STOP — wait for the user.**

### Step 8 — 🧑 GATE 5: Ratification
Ask for ratification in so many words. One word from the human closes the area; record the
ratification as a register entry (that entry is how the conductor sees closure from disk),
and name the next area. If the human raises concerns instead, loop back — Q&A (step 3) →
register (step 4) → sweep (step 7) — and return to this gate. Nothing advances past an
unratified gate. **STOP — wait for the user.**

## Ledger discipline (two-phase commit for documents)

Derivations LIST amendments; sweeps EXECUTE them and grep-verify. Never fold the two into
one step: listing separates detection from application, so a crash, a bad edit, or a
skipped file is mechanically visible instead of silently absorbed.

## Handoff

End the cycle with the handoff block. NEXT is the conductor (`rgv`) — or the next area's
cycle if the area order makes it obvious. Either way the ratification gate has just been
crossed, so never auto-invoke: the user's word is the transition.

```
STATE: <what is now true, with register/doc refs>
NEXT:  <named skill or gate> — <one-line why>
       (crosses a human gate → say "continue" or invoke directly)
```
