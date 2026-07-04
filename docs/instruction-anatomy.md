# Instruction Anatomy — what makes agents verify

The pattern's agents are ordinary LLM agents; the verification behavior comes entirely from **contract-shaped instructions**. Every RGV agent prompt has six parts:

## 1. The contract shape

| Part | What it does | Example fragment |
|---|---|---|
| **Read-list, in order** | grounds the agent in disk state, not assumptions | "READ (in order): 1. PROCESS.md … 2. DECISIONS.md — you implement D0XX (read fully)…" |
| **Binding constraints, quoted** | the register entries this work must respect, by ID | "under D011/D025 (regions contract), D038 (SA names)…" |
| **Per-item output contract** | forces enumerated verdicts — the anti-skip mechanism | "for EACH finding: severity + file/section + short quote + concrete fix"; "give each item a verdict: CONFIRMED / CHANGED / UNRESOLVED" |
| **Oracle assignment** | names what to verify against, and sanctions it | "verify online against docs.terragrunt.com and cloud.google.com — sanctioned; no other sources" |
| **Anti-behaviors, named** | the discretion boundary | "do NOT redesign ratified decisions"; "flag, don't resolve"; "no design discretion — anything bigger than a fact-fix gets flagged" |
| **Exit criteria + report shape** | machine-checkable completion | "VERIFY before finishing: grep X (0 expected), grep Y (≥1). REPORT: per-file edits, grep results, anything not applying cleanly" |

## 2. Why this produces validation (and plain instructions don't)

- "Review this" → a summary, which can silently omit. "For each of the 14 items, emit a verdict" → omission is visible in the output's shape.
- "Verify the budgets" → agreement. "**Recompute every budget yourself** and flag ANY arithmetic error" → re-derivation, which is where findings come from.
- "Make sure it's consistent" → vibes. "grep all five docs for '12-role' — 0 live claims expected" → a post-condition that passes or fails.

## 3. The oracle taxonomy — how each item's validation method is decided

The item's **claim type** selects its oracle:

| Claim type | Oracle | Verification act |
|---|---|---|
| Syntax / code shape | the language's parser or validator | "this HCL must parse" — run/inspect against grammar |
| API surface (fields, resources, enums) | official provider/vendor reference | fetch the doc page; match field names exactly |
| Behavioral ("X happens when Y") | vendor documentation, quoted | find the sentence; quote it with access date |
| Arithmetic / limits / counts | recomputation | redo the math from the primitives |
| Cross-document consistency | the other document | grep/diff both sides |
| Time-sensitive (versions, deadlines, pricing) | live fetch, access-dated | re-verify at defined passes (freshness) |
| Design conformance | the register | does the artifact implement the D-entry verbatim? |
| Existence/completeness | enumerated sweep | "list any orphans" against the full inventory |

Two supporting rules make the taxonomy bite: **inline citation is mandatory** (an uncited claim is visibly unverified), and **citations carry access dates only** (page-update stamps churn; the access date is the evidentiary fact).

## 4. Role independence

The derivation author and the gate reviewer are different agents with different framings: the author's contract says "implement the register exactly; flag what you can't reconcile"; the reviewer's says "you are NOT the author; be adversarial; re-derive; verify the author's flags' premises before adjudicating them." Fresh context on both sides means the reviewer verifies what's *written*, not what was *meant* — anchoring is structurally impossible.

## 5. The handoff block (every skill's footer)

```
STATE: <what is now true, with register/doc refs>
NEXT:  <named skill or gate> — <one-line why>
       (crosses a human gate → say "continue" or invoke directly)
```
