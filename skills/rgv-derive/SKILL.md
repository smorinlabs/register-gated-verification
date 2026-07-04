---
name: rgv-derive
description: Post-freeze derivations — inventory the derivation backlog the frozen docs accumulated (handoff sections, "→ ops guide" pointers), then write runbooks and guides that cite frozen doc versions and D-IDs, make ZERO new decisions, and follow who/what/verify/failure discipline for every procedure; anything the corpus left genuinely open becomes a FLAG list adjudicated by the human, never resolved inline. Use when the user says "write the runbooks", "write the ops guides", "derive the guides", "what does the frozen design say we build next", or where the rgv conductor routes when FREEZE.md exists. Requires FREEZE.md (a ratified freeze).
---

# RGV Derive — post-freeze derivations

Derivations translate a frozen design into operable documents — runbooks, guides,
checklists — without adding a gram of design. The freeze is what makes this safe: every
statement can cite a doc version that cannot drift and a D-ID that cannot be edited. The
discipline is the same as the cycle's derivation step, corpus-wide: **zero new decisions;
flag, don't resolve.**

## State contract

Requires on disk: `FREEZE.md` with its ratifying register entry — the freeze pins the doc
versions derivations cite. No FREEZE.md → **STOP and route to the conductor (`rgv`)**;
deriving from an unfrozen corpus builds on sand.

Leaves on disk: the derived docs (default home `ops/` with an index README, absent a
PROCESS.md convention) · the flag list · register entries for adjudicated flags.

## Step 1 — Inventory the derivation backlog

The backlog was accumulated, not invented: sweep EVERY frozen doc's handoff sections —
"→ ops guide", "recipe ships with the guides", "runbook-only stance", C2-class pointers —
plus FREEZE.md §5's named next steps and the freeze catalog's cross-references. Build the
inventory: one row per derivation — ID (R1, R2, …) · one-line scope · source pointers
(doc §s + D-IDs) · a dependency-aware writing order (the genesis/end-to-end doc usually
first: it forces every seam into the open early). Per-item, so omissions are visible:
every swept pointer lands in some derivation's row or is listed as intentionally unhomed.

Present the inventory + order to the user before writing (a reading checkpoint, not a
formal gate — the formal gate is step 4).

## Step 2 — Write the derivations (agents, fresh context)

For each backlog item (singly or in dependency-safe batches), launch a **fresh-context
agent** with `references/runbook-template.md` filled in. The contract binds each derivation
to:

- **Citations to frozen state**: every derived doc opens with a status line naming its
  source docs AT THEIR FROZEN VERSIONS plus the governing D-IDs, and cites D-IDs inline
  at each load-bearing step. A claim with no frozen source is a defect.
- **ZERO new decisions**: where the frozen corpus is silent, ambiguous, or contradictory,
  the author emits a FLAG (premise · what it blocks · suggested direction optional) and
  writes around it. Inventing a default IS a decision — flag it instead.
- **Who / what / verify / failure discipline** for procedures: every step names its
  executor (which human role, which service account, which lane), what to do (exact
  commands/values, no "appropriately"), how to verify it worked (an observable
  post-condition), and what failure looks like + the first recovery move.

## Step 3 — Consolidate the flag list

Merge every agent's flags into one list: FLAG-n · premise · what it blocks · which
derivations it touches · suggested direction if any. These are REGISTER CANDIDATES —
genuinely open points the corpus never decided. They are never resolved inline, however
obvious; obvious answers adjudicate fast.

## Step 4 — 🧑 GATE: flag adjudication

Present the flag list decision-style — per flag: the premise, options, a recommendation +
why, runner-up + why. The human adjudicates each (batching is fine); every resolution
becomes a register entry (post-freeze entries are normal — the register is the change
mechanism), and the affected derivations are updated to cite the new D-IDs. Overrides
recorded verbatim. **STOP — wait for the user.** A derivation set with unadjudicated flags
is incomplete, not shippable-with-asterisks.

## Step 5 — Close out

Write/refresh the derived-docs index (one line per doc + any operating calendar the
runbooks imply) and restate the change policy: derived docs are maintained derivations —
factual bugs fix freely, but anything that would alter a ratified mechanism goes through
the register, not through an ops edit. Verify: every backlog row has its doc; every doc's
citations point at frozen versions; every flag has a register entry.

## Handoff

Derivation is RGV's last phase; what follows (implementation, instantiation) is named by
FREEZE.md §5 and the adjudicated flags.

```
STATE: N derivations written (backlog complete) · M flags adjudicated → D0XX–D0YY · index live
NEXT:  <FREEZE.md §5's next phase, or rgv conductor for a status read> — the RGV lifecycle
       is complete; the register remains the change mechanism
       (crosses a human gate → say "continue" or invoke directly)
```
