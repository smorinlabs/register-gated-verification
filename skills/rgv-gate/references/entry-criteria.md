# Entry criteria — rgv-gate (gate zero)

Evaluated at gate zero, before any other step. Verdict semantics:
`references/gate-zero-verdicts.md` — any MUST-HAVE miss ⇒ REJECTED (hard stop); a
STRENGTHENER miss ⇒ at best IMPROVABLE. Every criterion gets PASS or MISS in READINESS.md.

## MUST-HAVES (any miss ⇒ REJECTED)

- **The target document exists** — at a stated version; a gate without a fixed target
  verifies vapor.
- **An oracle list** — the sanctioned-oracle register meta-entry, or (standalone, outside
  an RGV repo) an ad-hoc list supplied by the user. No list, no verification acts — only
  plausibility.
- **The claims are citable** — the document carries checkable claims: D-ID citations
  in-project, or claims specific enough to assign an oracle by claim type. A document of
  pure opinion has nothing to gate.

## STRENGTHENERS (miss ⇒ IMPROVABLE)

- **The register readable** — lets the design-conformance rows of the claim-type table
  fire (in-project runs).
- **Cross-consistency partners enumerated** — the related docs the reviewer should grep
  against, named up front.
- **Author flags carried in the document** — flags present with premises give the
  adjudication channel real work instead of reconstruction.
