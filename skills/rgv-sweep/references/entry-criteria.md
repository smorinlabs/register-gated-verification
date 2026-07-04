# Entry criteria — rgv-sweep (gate zero)

Evaluated at gate zero, before any other step. Verdict semantics:
`references/gate-zero-verdicts.md` — any MUST-HAVE miss ⇒ REJECTED (hard stop); a
STRENGTHENER miss ⇒ at best IMPROVABLE. Every criterion gets PASS or MISS in READINESS.md.

## MUST-HAVES (any miss ⇒ REJECTED)

- **A written ledger** — amendments with exact before → after per item; "fix the docs"
  is not a ledger.
- **Authorizing D-entries in the register** — the register entries backing every change
  in the set; a sweep without written authority is just editing.
- **Every ledger-target document reachable** — the exhaustive touched-doc list opens; an
  unreachable target guarantees a half-applied sweep.

## STRENGTHENERS (miss ⇒ IMPROVABLE)

- **An explicit grep post-condition list** — else the sweeper derives greps from the
  ledger's own claims; explicit is stronger.
- **The gate report at hand** — musts vs accepted ride-alongs stated, not reconstructed.
- **PROCESS.md's version conventions** — bump style stated beats defaulting.
