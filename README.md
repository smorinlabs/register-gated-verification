# Register-Gated Verification (RGV)

A method — and a Claude skill family — for consolidating research into verified designs where **every item is individually verified, across multiple phases and passes, with a human gate at every decision boundary**. Born from a real project: consolidating ~1 year of infrastructure research (95 sources) into a frozen, QA-gated golden design (68 register entries, 10 adversarial gates, 24 runbooks).

**Why it exists**: most efforts *claim* multi-pass verification and don't do it. RGV makes verification structural rather than virtuous — see [docs/pattern.md](docs/pattern.md) for the five mechanisms.

## The skill family

| Skill | Fires when | Human gates |
|---|---|---|
| **rgv** (conductor) | "continue the process" / "where are we" — reads repo state, routes to the right skill | mediates all |
| rgv-intake | "index this research" — provenance inventory, archive+manifest, lineage | scope · grouping |
| rgv-seed | project start — goals/process interview, register bootstrap, oracle list | it IS an interview |
| **rgv-cycle** | once per area — the 8-step core loop | 5 gates |
| rgv-gate | "adversarially verify this document" (standalone or called by rgv-cycle) | flag adjudication |
| rgv-sweep | apply a resolutions ledger across docs + grep post-conditions | report-only |
| rgv-close | zoom-out invariants → freshness pass → freeze | ratify each pass |
| rgv-derive | post-freeze: runbooks/guides, zero-new-decisions | derivation flags |

**Chaining rule** (the load-bearing discipline): within a phase, skills invoke each other directly; **across a human gate, never** — the skill ends with a handoff block and the user's word is the transition. Full statement: docs/pattern.md §4.

## Docs
[pattern](docs/pattern.md) · [terminology](docs/terminology.md) · [inputs & initial state](docs/inputs-and-state.md) · [instruction anatomy](docs/instruction-anatomy.md) · [case study](docs/case-study.md)

## Status
v0.1 — docs core + heart skills under construction. Install: grab `dist/*.skill` (when published).
