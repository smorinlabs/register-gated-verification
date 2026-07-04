# Entry criteria — rgv-cycle (gate zero)

Evaluated at gate zero, before any other step. Verdict semantics:
`references/gate-zero-verdicts.md` — any MUST-HAVE miss ⇒ REJECTED (hard stop); a
STRENGTHENER miss ⇒ at best IMPROVABLE. Every criterion gets PASS or MISS in READINESS.md.

## MUST-HAVES (any miss ⇒ REJECTED)

- **Seeded register + PROCESS.md** — DECISIONS.md with its meta-entries (question-format
  bar, sanctioned-oracle list, gate rules, recency rule, derivation discipline) and the
  process file; every contract this cycle fills cites them by ID.
- **The area is named in the area list** — PROCESS.md's ratified order knows this area.
  An area outside the list is unscoped work, not the next cycle.
- **Corpus/mining sources reachable** — `archive/` + INDEX open and readable; the
  miner's entire world is these files.
- **Prior areas' ratification entries present** — every earlier area in the dependency
  order has its closure entry in the register; building on an unratified area is
  building on sand.

## STRENGTHENERS (miss ⇒ IMPROVABLE)

- **Gap-list research done for this area** — INDEX gap items touching the area researched
  before step 2 keeps the decision doc's legs thick.
- **Prior areas' design docs at current versions** — the cross-consistency partners
  step 6 will grep.
- **The human's gate availability known** — five gates are coming; a known window keeps
  the cycle from stalling mid-loop.
