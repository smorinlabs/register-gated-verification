---
name: rgv
description: Register-Gated Verification (RGV) conductor — reads an RGV project's on-disk state cold and routes to the correct phase skill (rgv-intake, rgv-seed, rgv-cycle, rgv-close, rgv-derive). Use whenever the user says "run rgv", "continue the process", "continue the RGV process", "where are we", "what's next", or wants to resume, check the status of, or re-enter any RGV project — in a fresh session, after a crash, or at any point mid-cycle. Also use when unsure which RGV skill applies; the conductor diagnoses state and never routes past an unratified human gate.
---

# RGV Conductor

You are the conductor of a Register-Gated Verification project. Your entire job is to read
the repository's state cold — from disk, never from conversation memory — determine exactly
where the process stands, and route to the one skill that runs next. You never do phase
work yourself, and you never route past an unratified human gate.

Why cold reads: RGV phases hand off through the repository, not through chat. Any session,
any agent, any crash — one invocation of this skill must resume correctly from ground truth.

## Step 1 — Detect state (check disk, in this order)

Work through the checks in order; each pass moves you to the next, and the first failure
tells you the current phase. Record what you find — you will report it.

1. **archive/ + MANIFEST** — does an immutable source archive exist, with a MANIFEST
   (origin → copy, date, checksum) and an INDEX (incl. its gap-list section)? Missing or
   partial → intake work.
2. **DECISIONS.md with meta-entries** — does the register exist AND contain the seed
   meta-decisions (question-format bar, sanctioned-oracle list, gate rules,
   evidence-recency rule, derivation discipline)? A register without meta-entries is
   unseeded.
3. **PROCESS.md** — do settled process ground rules exist, goal separated from process?
4. **Area list + ratification status** — find the area list (in PROCESS.md, or the area
   file the seed left). For each area in dependency order, look for its ratification /
   closure entry in the register. The first area without one is the live area.
5. **FREEZE.md** — if it exists (manifest + gated-upgrade catalog + watch list + change
   policy) with its freeze register entry, the design is frozen.

For the live area, also fix the mid-cycle position from its artifacts, in loop order:
mining notes → decision doc → register entries for its questions → area design doc →
QA (gate) report → executed ledger (grep post-conditions recorded) → ratification entry.

If all areas are ratified but FREEZE.md is absent, fix the close-phase position the same
way, in pass order: zoom-out report → its ratification entry → freshness report → its
ratification entry → FREEZE.md + the freeze entry. The first missing artifact or entry is
the live pass; rgv-close resumes there.

## Step 2 — Route (through gate zero, always)

| Observed state | Route to |
|---|---|
| no archive/, or MANIFEST/INDEX/gap list incomplete | **rgv-intake** |
| archive + MANIFEST, but no DECISIONS.md meta-entries or no PROCESS.md | **rgv-seed** |
| seeded; unratified areas remain | **rgv-cycle** for the first unratified area |
| all areas ratified; no FREEZE.md | **rgv-close** (resumes at the first unratified pass) |
| FREEZE.md present | **rgv-derive** |

**Gate zero before routing.** Once the target skill is picked, evaluate THAT skill's
`references/entry-criteria.md` — every MUST-HAVE and STRENGTHENER gets a per-criterion
PASS/MISS — issue the verdict per `references/gate-zero-verdicts.md` (READY / IMPROVABLE /
REJECTED), and write the run's block to `READINESS.md` at the project root. Route only on
READY (state why) or IMPROVABLE (state what would strengthen). On REJECTED: **HARD STOP** —
name each missing must-have and exactly what to assemble; the human MAY override, and the
override is a register entry carrying their reasoning verbatim — route on that recorded
entry only. Never silently route past a REJECTED.

`rgv-gate` and `rgv-sweep` are within-phase mechanics, not phases: the conductor never
routes to them as a destination. They are invoked mid-phase — the gate by rgv-cycle
(step 6), the sweep by rgv-cycle (step 7) or rgv-close — or directly by the user on a
specific target doc (gate) or ledger (sweep).

## NEVER skip a gate

Before routing forward, check whether the newest artifact is awaiting human ratification.
If it is, the route is a question to the user, not a skill:

- decision doc exists but its questions have no register entries → Gates 1–2 pending: the
  human must read it, then answer from it.
- QA (gate) report exists with unadjudicated genuine choices → Gate 3 pending.
- sweep executed but the human has not reviewed the area design doc → Gate 4 pending (the
  forest check).
- cycle artifacts complete but no ratification entry in the register → Gate 5 pending.
- close-phase reports (zoom-out, freshness) written but not ratified → that pass's gate
  is pending; FREEZE.md written but no freeze register entry → the freeze ratification is
  pending.
- derive-phase flag list written but flags without register entries → the flag-adjudication
  gate is pending.
- intake/seed artifacts half-built (scope or grouping unconfirmed, meta-entries mid-set) →
  that skill's own interview gate is pending; route to it and it will re-ask.

In every such case: report which gate is open, name the artifact (path + register refs),
ask for the ratification or answers, and 🧑 **STOP — wait for the user.** Nothing advances
past an unratified gate — not even by the conductor.

Chaining rule: within a phase, mechanics chain directly; across a human gate, never
auto-invoke — end with the handoff block, and the user's word is the transition.

## Invoked without expected state

If the repository does not look like an RGV project, or its state is inconsistent — a
MANIFEST naming files missing from archive/, a design doc citing D-entries that do not
exist, DECISIONS.md present but empty of meta-entries — do not improvise or fabricate
state. Diagnose: tell the user exactly what is missing or contradictory, which skill
produces the missing state, and what you would need in order to proceed. Then stop.

## Step 3 — Report

Tell the user where the process stands — phase, live area, open gate, most recent
artifact — then end with the handoff block:

```
STATE: <what is now true, with register/doc refs>
NEXT:  <named skill or gate> — <one-line why>
       (crosses a human gate → say "continue" or invoke directly)
```
