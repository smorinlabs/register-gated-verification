# MANIFEST template

The MANIFEST is the provenance ledger: it proves each archive copy's origin, date, and
integrity. One row per archived file, grouped by group number. The checksum makes tampering
and bit-rot detectable; the origin path makes every claim about a source traceable to where
it actually lived.

Conventions:

- **Date** = original content date (mtime, content-stated, or decoded from an export ID) —
  never the copy date. Say in the header which dating methods were used.
- **Checksum** = a stated truncation of a stated algorithm (e.g. first 12 hex of sha256),
  computed on the copy; record that the original was verified identical at copy time.
- The MANIFEST is append-mostly: new intake passes add rows (with a dated note); existing
  rows are corrected only by an annotation, never silently rewritten.

---

```markdown
# MANIFEST — archive provenance

Copied {COPY-DATE} from the locations indexed in INDEX.md. Dates = original content/mtime
dates. SHA = first 12 hex of sha256 of the copy (originals verified identical at copy time).

## Group 1 — {group name}

| Original | Archive copy | Date | SHA-12 |
|---|---|---|---|
| `~/{original/path/file one.pdf}` | `g1--{kebab-name}--{YYYY-MM-DD}.pdf` | {YYYY-MM-DD} | {abc123def456} |
| `~/{original/path/file two.md}` | `g1--{kebab-name}--{YYYY-MM-DD}.md` | {YYYY-MM-DD} | {0123456789ab} |

## Group 2 — {group name}

| Original | Archive copy | Date | SHA-12 |
|---|---|---|---|
| … | … | … | … |
```

---

Post-conditions to run before calling the MANIFEST done:

- `ls archive/ | wc -l` equals the total row count — record both numbers.
- Every row's archive filename parses against `g{#}--{kebab}--{YYYY-MM-DD}.{ext}`.
- Recompute the checksum for at least one file per group; record the spot-check.
