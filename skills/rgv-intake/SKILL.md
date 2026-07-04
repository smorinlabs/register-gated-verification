---
name: rgv-intake
description: Corpus indexing — inventory every raw source with dates and provenance, dedupe byte-for-byte, trace supersession lineage, then build an immutable archive with a MANIFEST (origin → copy, date, checksum), an INDEX (groups, lineage narrative, gap list), all under a dated naming convention. Use when the user says "index this research", "index this corpus", "what do I have", "what research exists on X", or at the very start of an RGV project before any archive or register exists; also where the rgv conductor routes when archive/, MANIFEST, or INDEX are missing or partial. Requires only raw, dateable sources — intake creates the state every later phase needs.
---

# RGV Intake — corpus indexing

Turn a scattered corpus into the ground truth the whole process stands on: an immutable
archive, a provenance MANIFEST, and an INDEX that tells the corpus's story. **Chronology is
load-bearing**: the evidence-recency rule downstream (latest position = presumptively
evolved thinking) only works if every source carries a defensible date — dating is not
bookkeeping here, it is the verification substrate.

## State contract

Requires on disk: raw sources that can be dated. Nothing else — intake is the only RGV
skill with no upstream state. Leaves on disk: `archive/` (immutable copies) · `MANIFEST.md`
(origin → copy, date, checksum) · `INDEX.md` (groups, lineage, exclusions, gap list).

If a complete archive + MANIFEST + INDEX already exist, **STOP and route to the conductor
(`rgv`)** — the project is past intake, and re-copying an archived corpus is supersession
work, not indexing. A *partial* intake (e.g. new sources surfaced after the first pass)
resumes here: new files get new MANIFEST rows and an INDEX update note; existing archive
copies are never touched.

## Step 1 — 🧑 GATE: scope confirmation

Propose the search scope before searching in earnest: locations to scan (folders, repos,
cloud drives, chat/transcript exports), the in/out boundary (what topic counts as this
corpus), and known exclusions. Present it as a short list the human can edit.
**STOP — wait for the user.** Everything downstream inherits this boundary; an unratified
scope produces an archive nobody trusts.

## Step 2 — Inventory, with dates and provenance

Sweep the ratified scope. For EACH candidate file emit: path · date · how the date was
established (file mtime, content-stated date, an ID decoded from an export URL/filename —
say which) · a one-line content identification. An undated file may not proceed: if no
date can be established, list it separately with what was tried, for the human to date or
drop at the step-4 gate. Do not summarize files together — the inventory is per-item.

## Step 3 — Dedupe and lineage

- **Dedupe by bytes, not names**: byte-compare (`cmp` / checksum match) every suspected
  duplicate; record verified duplicates as pointers to one canonical copy.
- **Supersession chains**: where content was copied elsewhere and evolved, record the
  chain (what superseded what, with dates). Distinguish the **latest implemented** state
  from the **latest design direction** when they differ — both matter downstream.
- **Exclusions**: near-scope files examined and rejected go on an examined-and-excluded
  list with a reason each. An exclusion without a reason is a silent skip.

## Step 4 — 🧑 GATE: grouping ratification

Propose the groups (by effort/phase — research library, plan bake-off, spec, repo docs,
late-phase conversations…), each group's keep/superseded verdicts, the lineage narrative,
and dispositions for any undatable files from step 2. **STOP — wait for the user.** The
groups become the archive's naming skeleton; ratify before a single file is copied.

## Step 5 — Immutable archive

Copy every in-scope file into `archive/` under the naming convention:

```
g{group#}--{kebab-case-name}--{YYYY-MM-DD}.{ext}
```

The date is the ORIGINAL content date (step 2), never the copy date. Copies are immutable
from this moment: never edited, never renamed, never deleted — corrections happen in
MANIFEST/INDEX annotations, not in the copies.

## Step 6 — MANIFEST

Write `MANIFEST.md` from `references/manifest-template.md`: one row per archived file —
origin path → archive copy · date · checksum (computed from the copy, with the original
verified identical at copy time). Grouped by group number.

## Step 7 — INDEX

Write `INDEX.md` from `references/index-template.md`: the story-in-one-paragraph · the
lineage diagram (what superseded what, dated) · per-group tables · the
examined-and-excluded list · the **gap list** — research the corpus SHOULD contain but
does not (this list seeds decision-document research downstream; "none found" must be
stated, not implied).

## Exit criteria (verify before finishing)

- File count in `archive/` = MANIFEST row count (run the count, record both).
- Every MANIFEST row has a checksum; spot-verify a sample against the copies.
- Every group in the INDEX was ratified at step 4; every exclusion carries a reason.
- The gap list exists, even if it says "none".

## Handoff

Intake feeds the seed interview, which needs a human — never auto-invoke across it.

```
STATE: archive/ (N files, immutable) + MANIFEST.md + INDEX.md (M groups, gap list) written
NEXT:  rgv-seed — the register does not exist yet; the process interview comes next
       (crosses a human gate → say "continue" or invoke directly)
```
