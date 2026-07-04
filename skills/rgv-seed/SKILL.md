---
name: rgv-seed
description: Process bootstrap — the seed interview that separates goals from process, ratifies the meta-decision set (question-format bar, sanctioned-oracle list, gate rules, evidence-recency rule, derivation discipline, QA-gate rule, propagation/ledger rule) into the register as D-entries, decomposes the work into dependency-ordered areas, and writes PROCESS.md. Use at project start when the user says "set up the verification process", "seed the register", "start the RGV process", "let's define how we'll work", or where the rgv conductor routes when an archive exists but DECISIONS.md lacks meta-entries or PROCESS.md is missing. Requires intake outputs (archive/ + MANIFEST + INDEX) and the human — the seed IS an interview; nothing here is delegated.
---

# RGV Seed — the process interview

The seed makes the process itself a register artifact: every rule later agents obey is a
citable D-entry ratified here, by the human, before any area work begins. This is the
self-hosted-process mechanism — feedback like "short briefs don't work" becomes an
enforceable entry, not folklore. **Every step of this skill crosses the human.** There are
no fresh-context agents in the seed; delegating an interview defeats it.

## State contract

Requires on disk: intake outputs — `archive/` + MANIFEST + INDEX (the corpus the process
will consolidate) — and a present human adjudicator. Missing intake state → **STOP and
route to the conductor (`rgv`)** rather than improvising an archive.

Leaves on disk: `DECISIONS.md` seeded with the meta-entries · `PROCESS.md`, which CARRIES
the dependency-ordered area list and the artifact-naming conventions (downstream skills
read their file conventions from it).

## Interview discipline (binding throughout)

- One topic per exchange; after each, **STOP — wait for the user.** This skill is
  human-paced end to end.
- Every resolution is registered IMMEDIATELY as a Dnnn entry — decision, status, source
  ("interview"), date. Entries are immutable; a changed mind mid-interview is a
  superseding entry, even minutes later.
- Recommended defaults come from `references/meta-decision-menu.md` — present each with
  its recommendation and what varying it costs. **Overrides are first-class**: recorded
  verbatim with the human's reasoning.
- Record the **named human adjudicator** (who ratifies gates from here on) in the first
  entries — ratification authority is state, not assumption.

## Step 1 — 🧑 Goals vs process separation

Interview for two distinct statements and keep them apart:

- **Goal**: the end state — what artifact set exists when this is done, who consumes it,
  what is explicitly out of scope (e.g. "a reusable reference design; the first instance
  is thin-layered on top, not baked in").
- **Process**: how the corpus becomes that end state — which this interview is settling.

Draft both from the INDEX's story paragraph, present, revise, and register the goal + the
scope boundary as the first D-entries. **STOP — wait for the user** at each proposal.

## Step 2 — 🧑 The meta-decision set

Walk `references/meta-decision-menu.md` item by item — question-format bar ·
sanctioned-oracle list · gate rules · evidence-recency rule · derivation discipline ·
QA-gate rule · propagation/ledger rule. For EACH: present the recommended default and its
rationale, take accept / adjust / override, and register the resolution as its own
meta-entry. Do not batch the whole menu into one question — these seven rules bind every
later agent, and each deserves its own considered word. **STOP — wait for the user** per
item or small batch.

The sanctioned-oracle list deserves extra care: name the actual sources (official vendor
docs, specific ecosystem references) — "reputable sources" is not a list an agent can be
held to.

## Step 3 — 🧑 Area decomposition

Propose the area list from the INDEX's groups and the ratified goal: each area one
consolidation unit (one cycle), ordered by DEPENDENCY — an area comes after every area
whose decisions it builds on (foundations before structure before naming; a
tooling/direction fork that everything depends on goes first, often as a lone decision
brief). Present order + one-line scope per area; revise until ratified; register the list.
**STOP — wait for the user.**

## Step 4 — Write PROCESS.md and verify the seed

Write `PROCESS.md` carrying, with D-IDs cited inline: the settled ground rules (goal/scope
separation, the step-2 meta-rules) · a short statement of the cycle as this project runs
it · **the ratified area order** · the register rules (one row per decision; immutable;
supersede, never edit; rejected alternatives live in the register) · **artifact naming
conventions** (mining notes, decision docs, design docs, QA reports — downstream defaults
apply only where this file is silent) · known tensions to decide consciously when they
recur.

Verify before finishing: every step-1–3 resolution has a D-entry; DECISIONS.md parses as
the one-row-per-decision table; PROCESS.md names the area list and file conventions.
Present both files for a final read.

## Handoff

```
STATE: DECISIONS.md seeded (D001–D0NN meta-entries) · PROCESS.md written (area list + naming)
NEXT:  rgv-cycle — first area in the ratified order: <area 1>
       (crosses a human gate → say "continue" or invoke directly)
```
