# FREEZE.md template

The freeze document is the immutability boundary made explicit: what is frozen at exactly
which version, under what change policy, with every known future change pre-catalogued so
evolution happens through the register instead of around it. It is written only after the
zoom-out and freshness passes are ratified, and it becomes real only with the freeze
register entry.

```markdown
# FREEZE — {Design name} v{X.Y.Z}

**Declared**: {date} · **Register entry**: D{NNN} · **Gates**: zoom-out passed (D{XX},
"{verdict}" — all findings applied) · freshness passed (D{YY}, {n} confirmed / {n} changed
/ {n} unresolved — verdict {V})

## 1. Declaration — what "frozen" means

{The frozen doc set + PROCESS.md + the register} constitute **{name} v{X.Y.Z}**. From this
point:

- **No frozen doc changes except via the register.** Any content change requires a new
  DECISIONS.md entry (supersession or explicit amendment — the register is
  immutable-append; supersede, don't edit) plus a version bump on every touched doc,
  ratified-status lines preserved.
- **Decision records are never retro-edited**: decision docs, mining notes, and QA reports
  are frozen historical records as-written.
- **Fact-fixes ≠ design reopens**: external-currency pins may land as patch bumps citing
  their verification source and register entry; anything altering a ratified mechanism is
  a design reopen and takes the full decision-document path.
- DECISIONS.md itself is NOT frozen — append-only, it is the change mechanism. Adopting
  any §3 catalog item goes through it.

## 2. Corpus manifest — frozen versions

| Doc | Frozen at | Scope (one line) |
|---|---|---|
| {doc path} | **v{N}** | {what it owns} |
| … | … | … |

Also part of the freeze: PROCESS.md · DECISIONS.md ({N} entries, D001–D{NNN-1}, at freeze;
D{NNN} records this declaration) · frozen supporting records (decision docs, mining notes,
QA reports incl. the zoom-out and freshness reports) · the provenance layer (INDEX /
MANIFEST / archive — never edited).

## 3. The register-gated upgrade catalog

{Swept from every doc + the whole register range; completeness-checked. Three tiers:}

### Tier A — register entry required before adoption
| # | Upgrade | Source | Trigger | What it would change |
|---|---|---|---|---|

### Tier B — pre-sanctioned evolutions (adopt without re-ratification; record when done)
| # | Evolution | Source | Trigger | What it changes |
|---|---|---|---|---|

### Tier C — documented instance-layer options (no register action)
{list, with sources}

{Plus a "closed / promoted out" line for completeness: items that shipped or died before
the freeze, with their entry IDs.}

## 4. Post-freeze watch list

| Item | Where | Check when |
|---|---|---|
| {external fact that may move} | {doc §} | {trigger event or cadence} |

## 5. What comes next

1. **Derivation phase**: {the runbook/guide backlog and its writing order}.
2. {Whatever the register says follows — implementation planning, instantiation, …}
```

Freeze-entry shape for the register (append on ratification, never before):

```
D{NNN} | freeze | **{NAME} v{X.Y.Z} FROZEN ({date})** — see FREEZE.md. Gates passed:
D{XX} zoom-out, D{YY} freshness ({counts}). Corpus: {doc versions, pinned}. Catalog:
{n} items in FREEZE.md §3. Change policy: register supersession only. Next: {phase}.
{Adjudicator} to tag v{X.Y.Z} | ratified | FREEZE.md | {date}
```
