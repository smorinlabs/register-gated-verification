# The RGV Pattern

## 1. The lifecycle

```
INTAKE ──► SEED ──► CYCLE × N areas ──► CLOSE ──► DERIVE
(index      (goals/     (the core loop)     (zoom-out ·   (runbooks
 corpus)     process                         freshness ·    from frozen
             interview)                      freeze)        decisions)
```

Each phase leaves state on disk that the next phase requires — phases hand off through the repository, never through conversation memory. This is what makes the process resumable across sessions, agents, and crashes.

## 2. The core loop (rgv-cycle) — 8 steps, 5 gates

1. **Mine** (agent, fresh context): extract the area's claims + conflicts from the corpus, evidence ordered chronologically with provenance. Latest position = presumptively evolved thinking (overridable with reason). Output: mining notes.
2. **Decision document** (agent): EVERY open question, each carrying — cold-start background, chronological corpus evidence, externally verified practice (cited, access-dated), options with pros/cons/risks, recommendation + why, runner-up + why. Thin legs get researched BEFORE presenting. 🧑 **GATE 1: the human reads it.**
3. **Gated Q&A**: 🧑 **GATE 2** — questions asked ONLY from the document, item by item or batched; questions needing more evidence carry a research option that loops back into the document before re-asking; free-form concerns last. **Overrides are first-class**: recorded verbatim with the human's reasoning.
4. **Register**: every resolution becomes an immutable Dnnn entry. Supersede, never edit.
5. **Derive** (agent, fresh context): the area design doc, implementing the register exactly; cross-document amendments LISTED in a ledger, not applied; inconsistencies FLAGGED, not resolved.
6. **Adversarial gate** (different agent — IV&V): verify every load-bearing claim against its oracle; re-derive, don't confirm; per-item verdicts; adjudicate the author's flags decision-style (options/rec/runner-up). 🧑 **GATE 3: genuine choices return to the human; mechanical fixes proceed.** Gate-3 resolutions are register entries like any other. Chaining nuance: a gate report containing ONLY mechanical fixes chains directly to the sweep; ANY genuine choice stops at the human first.
7. **Sweep**: apply resolutions + execute the ledger across all affected docs; version bumps; grep post-conditions prove application. 🧑 **GATE 4: the human reviews the area design doc (the forest check).**
8. 🧑 **GATE 5: ratification** — one word closes the area and unblocks the next. Nothing advances past an unratified gate. **Ratification is machine-readable: it is written as a register entry** (the conductor detects area state by reading the register, never by inference).

## 3. The five mechanisms (why item-by-item verification actually happens)

1. **Enumerability creates accountability objects.** You cannot verify "the design"; you can verify D043·M2. Everything is decomposed into identified items — decision IDs, numbered findings, checklist steps. Verification only exists where items have identities.
2. **Exhaustiveness contracts.** Instructions demand per-item output: "give EACH item a verdict", "list any orphans", "for each: severity + quote + fix." An agent forced to emit item-by-item output cannot silently skip item 9 — omission becomes syntactically visible. This is the single biggest delta from ordinary practice, where "review this" invites a summary that hides skips.
3. **Every claim carries its own oracle.** What to check against is assigned by claim type (see instruction-anatomy §3); requiring inline citation makes unverified claims visible as naked assertions.
4. **Re-derivation framing.** "Verify X" produces agreement; "recompute X yourself and flag ANY error" produces findings. Reviewers are adversarial and independent of authors.
5. **State outside the conversation.** Register + docs + reports on disk mean every pass starts from ground truth. Also the crash-recovery property: a half-applied change is detectable because the ledger says what should exist and grep says what does.

**The meta-mechanism**: the process is self-hosted — process rules are themselves register entries, amended by supersession like any design decision. Human feedback about the process ("short briefs don't work") becomes a citable, enforced rule, not folklore.

**One sanctioned exception to the two-phase ledger discipline**: the close's freshness pass applies CHANGED fact-fixes directly, as patch bumps citing the pass — no separate ledger-then-sweep. It stays safe because the exception is fact-class only (anything touching a ratified mechanism is still flagged, never fixed), the pass report enumerates every fix per-item (the report is the ledger), and the pass's own ratification gate reviews the list before it stands.

## 4. The chaining rule (skill orchestration)

- **Within a phase**: skills/agents invoke each other directly (cycle → gate → sweep) — mechanics need no permission.
- **Across a human gate: never auto-invoke.** The skill ends with a **handoff block** (canonical format: instruction-anatomy §5).
- **The conductor (rgv)** reads repo state cold and routes: no archive → intake; no register → seed; areas remaining → cycle; all ratified → close; frozen → derive. Any session, any point, one invocation resumes correctly.

**The full gate census** (every point a named human must ratify, across the lifecycle — the cycle's five are the famous ones, not the only ones):

| Phase | Human gates |
|---|---|
| intake | 2 — scope confirmation · grouping ratification |
| seed | the whole phase — an interview, human-paced per meta-decision |
| cycle (× N areas) | 5 — document read · gated Q&A · gate-report triage · post-sweep forest check · area ratification |
| close | 3 — one ratification per pass: zoom-out · freshness · freeze |
| derive | 1 — flag adjudication |

rgv-gate's genuine-choice channel surfaces inside the cycle's gate 3 (or standalone, its own stop); rgv-sweep has none — it is mechanics, and the checkpoint after it belongs to its caller.

## 5. Failure modes this pattern is built against

Silent skips (→ exhaustiveness contracts) · confirmation bias (→ re-derivation + independence) · propagation debt / shadow schemas (→ ledgers + sweeps + grep post-conditions) · folklore process (→ self-hosted rules) · lost context between sessions (→ state on disk) · unratified drift (→ gates) · stale facts (→ access-dated citations + a dedicated freshness pass) · scope creep in derivations (→ "zero new decisions; flag, don't resolve").
