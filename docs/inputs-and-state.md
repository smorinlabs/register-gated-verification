# Inputs and Necessary Initial State

## To start at all (rgv-intake / rgv-seed)
1. **A corpus with provenance** — sources that can be dated (recency weighting requires chronology). Intake produces: archive of immutable copies, MANIFEST (origin → copy, date, checksum), and an INDEX carrying the lineage/dedup narrative and the gap list.
2. **A goal statement separated from process** — what the end state is, distinct from how you'll get there. The seed interview extracts both and records the difference.
3. **A seeded register** — the meta-decisions come first: question format (completed-staff-work bar), the sanctioned-oracle list (which external sources count), gate rules (what a human must ratify), evidence-recency rule, derivation discipline. These are D-entries like everything else.
4. **A named human adjudicator** — with ratification authority and first-class override rights.
5. **Agent role contracts** — miner, decision-author, derivation-author, adversarial reviewer, sweep applier: each runs fresh-context and receives a contract-shaped prompt (see instruction-anatomy).

## Per-skill state contracts (how skills chain through the repo)

| Skill | Requires on disk | Leaves on disk |
|---|---|---|
| rgv-intake | raw sources | archive/ + MANIFEST + INDEX (incl. the gap list — a required INDEX section, not a separate file) |
| rgv-seed | intake outputs, the human | DECISIONS.md (meta-entries) + PROCESS.md (which CARRIES the area list + artifact-naming conventions) |
| rgv-cycle | register + PROCESS.md + prior areas ratified (= their register entries exist) | mining notes, decision doc, D-entries, area design doc (ratified), QA report, executed ledger |
| rgv-gate | a target doc + oracle list; the register when inside an RGV project. **Standalone relaxation**: outside an RGV repo, rgv-gate runs report-only against an ad-hoc oracle list supplied by the user — no register required, no sweep implied | a gate report (findings + adjudications + verdict) |
| rgv-sweep | a resolutions ledger + the register entries authorizing it (a sweep without written authority is just editing) | edited docs w/ version bumps + grep evidence |
| rgv-close | all areas ratified | zoom-out report, freshness report, FREEZE.md (manifest + gated-upgrade catalog + watch list + change policy) |
| rgv-derive | FREEZE.md with its ratifying register entry (an unratified FREEZE.md is a draft) | runbooks/guides citing frozen decisions; flags for anything the corpus left open |

A skill invoked without its required state STOPS and routes to the conductor rather than improvising (sole exception: rgv-gate's standalone relaxation above). Artifact file naming follows PROCESS.md's stated conventions; absent one, skills use their documented defaults and record them in PROCESS.md.
