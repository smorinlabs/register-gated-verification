---
name: rgv-close
description: The three closing passes that turn a fully-ratified corpus into a frozen design — (1) zoom-out, whole-corpus invariant checks with named-flag verdicts; (2) freshness, every parked verify-item re-checked against live oracles with CONFIRMED/CHANGED/UNRESOLVED verdicts; (3) freeze, the version manifest, register-gated upgrade catalog, watch list, and change policy — each pass ending at its own human ratification gate. Use when the user says "close the design", "run the zoom-out", "freshness pass", "freeze the corpus", "we're done with the areas — what now", or where the rgv conductor routes when all areas are ratified and no FREEZE.md exists. Requires every area's ratification entry in the register.
---

# RGV Close — zoom-out · freshness · freeze

Areas were verified one at a time; the close verifies the FOREST. Three passes, three
distinct oracles — the corpus against itself (zoom-out), the corpus against the live world
(freshness), then the freeze that makes immutability official. Each pass ends at a human
gate; a later pass never starts on an unratified earlier one.

## State contract

Requires on disk: ALL areas ratified — every area in PROCESS.md's order has its
ratification entry in the register. Anything unratified → **STOP and route to the
conductor (`rgv`)**; the close cannot paper over an open area.

Leaves on disk: the zoom-out report · the freshness report · `FREEZE.md`. Defaults where
PROCESS.md is silent: `qa/zoomout-report-{YYYY-MM-DD}.md`, `qa/freshness-report-{YYYY-MM-DD}.md`,
`FREEZE.md` at repo root. Each pass's ratification is a register entry — the conductor
reads closure from the register, never from inference. Resuming mid-close: the first pass
without a ratification entry is the live pass.

## Step 0 — GATE ZERO (entry-criteria check)

Before anything else: evaluate `references/entry-criteria.md` — every MUST-HAVE and every
STRENGTHENER gets a per-criterion PASS/MISS — then issue the verdict per
`references/gate-zero-verdicts.md` (READY / IMPROVABLE / REJECTED) and write the run's
block to `READINESS.md` at the target project's repo root. On REJECTED: **HARD STOP** —
name each missing must-have and exactly what to assemble; proceed only on a recorded
register override. Never silently proceed on bad inputs.

## Pass 1 — Zoom-out (whole-corpus invariants) → 🧑 ratify

Launch a **fresh-context agent** with `references/invariant-checklist.md` filled in. The
reviewer audits the whole corpus — every design doc at its current version, PROCESS.md,
the full register — for cross-cutting integrity that no single-area gate could see:

- **Dangling-pointer sweep**: every handoff/"resolved in area N" pointer traced to a live
  target; zero orphans or each one a finding.
- **Cross-doc contradiction hunt**: counts, role sets, names, and mechanisms stated in
  more than one doc, verified identical everywhere (the census discipline).
- **Census checks**: enumerable inventories (units, keys, roles) counted against their
  canonical table — completeness by enumeration, not impression.
- **Single-source-of-truth audits**: every fact has exactly one canonical defining doc;
  duplicated definitions either match or are findings.
- **Named-flag verdicts**: parked area flags addressed to the close get per-flag verdicts
  (F-a, F-b, …); flags that ask for a decision get DECISION-STYLE adjudications (verdict
  on the premise · options · recommendation + why · runner-up + why).

The report leads with a verdict (e.g. "freeze-ready after fixes") and severity-graded
findings. Internal consistency only — external verification belongs to pass 2.

**Triage and fix**: mechanical findings go to `rgv-sweep` as a ledger (within-phase, no
permission needed); decision-style adjudications go to the human. 🧑 **Ratification**:
present report path, verdict, fix evidence; the human's word closes the pass — record it
as a register entry summarizing findings, resolutions, and touched-doc versions.
**STOP — wait for the user.**

## Pass 2 — Freshness (live-oracle re-verification) → 🧑 ratify

Inventory every PARKED verify-item across the corpus — "verify at freshness" notes,
version pins, provider-issue statuses, external-behavior claims deferred by the
corpus-first rule. Launch a **fresh-context agent** whose contract binds it to the
sanctioned-oracle list and access-date-only citations, and demands for EACH item:

- a completed verification act against the LIVE oracle (fetch the page, check the issue,
  confirm the field), cited with access date;
- a verdict: **CONFIRMED / CHANGED (say what to) / UNRESOLVED (say what blocked)**;
- for CHANGED items: the fact-fix — applied directly with a PATCH bump citing the pass
  (fact-fixes are not design reopens; ratified mechanisms stay untouched). Anything whose
  change would alter a ratified mechanism is FLAGGED to the human, not fixed. Direct
  application here is the pattern's one sanctioned exception to the two-phase ledger
  discipline (pattern.md §3): the pass report enumerates every fix, and this pass's
  ratification gate reviews the list.

The report carries verdict counts up front (N confirmed / N changed / N unresolved) and a
freeze-ready verdict at bottom. 🧑 **Ratification**: present counts, the CHANGED list with
its patch bumps, and any flags; register the pass on the human's word.
**STOP — wait for the user.**

## Pass 3 — Freeze → 🧑 the freeze ratification + tag

Write `FREEZE.md` from `references/freeze-template.md`:

1. **Declaration** — what frozen means: no content change to frozen docs except via a new
   register entry (supersession, never edit) + version bump; decision records and QA
   reports are frozen history as-written; fact-fixes ≠ design reopens; the register itself
   stays append-only — it IS the change mechanism.
2. **Version manifest** — every frozen doc with its exact frozen-at version and a one-line
   scope; the register's entry range; the supporting-record inventory (decision docs,
   mining notes, QA reports, archive).
3. **Register-gated upgrade catalog** — every deferred/declined/escape-hatch item swept
   from the docs and register, consolidated in tiers: register-entry-required ·
   pre-sanctioned (record when done) · instance-layer options. Completeness-checked
   against the full register range.
4. **Watch list** — external items from the freshness report with WHERE each lives and
   WHEN to re-check.
5. **Change policy + what comes next** — supersession only; the derivation phase's entry
   point.

🧑 **The freeze ratification**: present FREEZE.md; ask in so many words. On the human's
word: append the freeze register entry (citing both passed gates by their entry IDs) and
tag the version if the repo is versioned. **STOP — wait for the user.** An unratified
FREEZE.md is a draft, not a freeze.

## Handoff

```
STATE: frozen at <version> — zoomout + freshness + freeze ratified (D0XX/D0YY/D0ZZ); FREEZE.md live
NEXT:  rgv-derive — the frozen corpus's derivation backlog (runbooks/guides) is now writable
       (crosses a human gate → say "continue" or invoke directly)
```
