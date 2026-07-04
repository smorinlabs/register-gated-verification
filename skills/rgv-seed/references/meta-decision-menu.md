# The meta-decision menu — recommended defaults

Seven process rules the seed interview must settle, each becoming its own register
meta-entry. The defaults below are genericized from a completed RGV project's ratified
meta-entries; they are recommendations with track records, not requirements. Present each
with its default and rationale; take accept / adjust / override; record overrides verbatim
with reasoning.

## 1. Question-format bar (the completed-staff-work rule)

**Default**: EVERY decision — detail or load-bearing — is presented in a comprehensive
decision document BEFORE any question is asked: background a cold reader needs · full
corpus evidence in context · externally verified practice · options with pros/cons/risks ·
recommendation + why · runner-up + why. If any leg is thin, it is researched BEFORE the
document is presented — in-question research options exist for the human's judgment calls,
not for thin preparation. Questions are asked ONLY from the document, in order; questions
needing more evidence carry a research option that loops back into the document before
re-asking; free-form concerns come last. Chat-only question lists violate the process.

**Why this bar**: the source project first tried pre-answered short briefs and the human
overturned it within a day ("I can't evaluate these") — the supersession chain
(D013 → D023 → D030 in the original) is the canonical self-hosted-process example.
**Variations**: a two-tier bar (full document for load-bearing calls, lighter for details)
trades rigor for speed — the source project tried exactly that split first and abandoned it.

## 2. Sanctioned-oracle list

**Default**: a NAMED list of external sources that count for verification — official
vendor/provider documentation domains, the official docs of each core tool, primary issue
trackers — plus the rule that the corpus itself is the oracle for provenance claims. No
other sources; an agent citing off-list is in breach. Citations carry ACCESS DATES only
(page-update stamps churn; the access date is the evidentiary fact).

**Interview must produce**: the actual domain/source names for THIS project, and any
standing exception (the source project sanctioned one tool's ecosystem docs for all areas
because the corpus predated that tool's 1.0 — decide such exceptions now, as entries).

## 3. Gate rules (what a human must ratify)

**Default**: five gates per cycle — decision-document read · gated Q&A · gate-report
triage (genuine choices only; mechanical fixes proceed) · post-sweep forest check ·
area ratification — plus one ratification per closing pass (zoom-out, freshness, freeze)
and a flag-adjudication gate in derivation. Nothing advances past an unratified gate;
ratifications are register entries (machine-readable closure). **Interview must produce**:
the named adjudicator, and any extra gates this project wants (e.g. budget/scope gates).

## 4. Evidence-recency rule

**Default**: per design element, corpus positions are presented CHRONOLOGICALLY with
provenance, and the latest position is treated as presumptively evolved thinking —
overridable by the human with a reason, never silently. Name any special re-evaluation
class now: positions formed before a pivotal decision (a tooling switch, a direction fork)
must be re-checked against the new context rather than inherited.

## 5. Derivation discipline

**Default**: derived artifacts (design docs, runbooks) make ZERO new decisions — every
statement traces to a cited D-entry; where the register is silent or conflicts, the author
FLAGS (premise + what it blocks) and never resolves; cross-document amendments are LISTED
in a ledger, never applied by the author. "Flag, don't resolve" is the separation of
detection from adjudication — the audit principle that keeps discretion at the gates.

## 6. QA-gate rule

**Default**: before an area closes, an INDEPENDENT fresh-context agent (never the author,
never the authoring conversation) adversarially reviews the area's design doc: re-derives
every load-bearing claim against its oracle, emits per-item verdicts and severity-graded
findings, adjudicates author flags decision-style. Findings are resolved or consciously
waived — recorded either way — before the next area begins.

## 7. Propagation / ledger rule

**Default**: two-phase commit for documents. Derivations LIST cross-document amendments in
a ledger (target · section · exact before → after); a separate sweep EXECUTES the ledger,
version-bumps every touched doc with a one-line reason citing D-IDs, and PROVES application
with grep post-conditions (pattern + files + expected count, run and recorded). A
half-applied change is thereby detectable: the ledger says what should exist, grep says
what does.

---

Register the seven as consecutive meta-entries (with the goal/scope entries from step 1
ahead of them), each: `D### | meta | <the rule as ratified> | ratified | interview | <date>`.
