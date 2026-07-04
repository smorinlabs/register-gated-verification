# Entry criteria — rgv-close (gate zero)

Evaluated at gate zero, before any other step. Verdict semantics:
`references/gate-zero-verdicts.md` — any MUST-HAVE miss ⇒ REJECTED (hard stop); a
STRENGTHENER miss ⇒ at best IMPROVABLE. Every criterion gets PASS or MISS in READINESS.md.

## MUST-HAVES (any miss ⇒ REJECTED)

- **ALL areas ratified, register-verifiable** — every area in PROCESS.md's order has its
  ratification entry in DECISIONS.md, verified by reading the register, never by
  asserted memory. The close cannot paper over an open area.
- **A parked verify-item inventory exists** — the "verify at freshness" notes, version
  pins, and deferred external checks are enumerable across the corpus; pass 2
  re-verifies an inventory, it does not invent one.

## STRENGTHENERS (miss ⇒ IMPROVABLE)

- **Sanctioned-oracle sources reachable now** — pass 2's live fetches need the network up
  and the sources answering.
- **Parked area flags addressed to the close collected** — the F-a/F-b adjudication list
  pre-assembled instead of swept for.
- **Design docs' version lines current** — the freeze manifest builds cleanly when every
  doc states its version.
