# Entry criteria — rgv-derive (gate zero)

Evaluated at gate zero, before any other step. Verdict semantics:
`references/gate-zero-verdicts.md` — any MUST-HAVE miss ⇒ REJECTED (hard stop); a
STRENGTHENER miss ⇒ at best IMPROVABLE. Every criterion gets PASS or MISS in READINESS.md.

## MUST-HAVES (any miss ⇒ REJECTED)

- **FREEZE.md with its ratifying register entry** — an unratified FREEZE.md is a draft;
  deriving from it builds on sand.
- **The derivation backlog is identifiable** — the frozen docs carry their handoff
  pointers ("→ ops guide", runbook-only stances, C2-class pointers) and/or FREEZE.md §5
  names next steps. No pointers anywhere means nothing accumulated to derive — inventing
  a backlog is designing.

## STRENGTHENERS (miss ⇒ IMPROVABLE)

- **PROCESS.md names derived-doc conventions** — else the `ops/` default applies and gets
  recorded.
- **The upgrade catalog's cross-references intact** — FREEZE.md §3 pointers resolving
  speeds the backlog sweep.
- **Runbook executors already named** — who/what roles (human roles, service accounts,
  lanes) named in the frozen docs keep the who-column from becoming flags.
