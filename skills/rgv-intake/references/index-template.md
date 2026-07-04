# INDEX template

The INDEX is the corpus's narrative layer: where the MANIFEST proves provenance file by
file, the INDEX tells a cold reader what exists, how it evolved, what won, what was
excluded, and what is missing. Downstream skills read it constantly — the miner scopes
areas from its groups; decision documents cite its lineage; the gap list becomes research
items.

Sections, in order (all required; write "none" rather than omitting):

```markdown
# {Project} — Research & Design Index

**Generated:** {date} · **Updated:** {date + what changed}
**Scope scanned:** {the ratified step-1 scope, verbatim}
**Dating:** {how dates were established — mtimes, content dates, decoded export IDs;
duplicates verified byte-for-byte}

## The story in one paragraph

{One paragraph: the effort(s) the corpus records, its phases with date ranges, and which
artifacts are the latest implemented state vs the latest design direction. Bold the two
"latest" pointers — every later phase leans on them.}

## Lineage (what superseded what)

{A dated chain diagram, oldest to newest:}

    {date}   {artifact / location}
                │  {copied / evolved / exported → where}
    {date}   {artifact}  ← ★ LATEST IMPLEMENTED
    {date}   {artifact}  ← ★ LATEST DESIGN DIRECTION

## Group {N} — {name} {★ keep as reference / ⚠ superseded, keep as historical record}

**Location:** {original location(s)}. {One line: what this group is.}

| Date | Title / file | Notes |
|---|---|---|
| … | … | {duplicates, twins, supersession pointers} |

{…one section per ratified group. Mark each group's disposition in the heading: keep as
reference, superseded-but-content-rich, newest design phase, etc.}

## Examined and excluded

{Every near-scope file rejected, each with a reason — "{file} ({why it is out of scope})".}

## Gaps — research described or needed but NOT on disk

1. {Missing topic — why it is believed to exist or be needed, and where it was looked for.}
2. …

{When a later intake pass fills a gap, strike it through with a dated FILLED note rather
than deleting it — the gap history is evidence too.}
```

Notes:

- The gap list is load-bearing: decision-document authors treat it as pre-authorized
  research topics ("thin legs get researched BEFORE presenting").
- Update stanzas (new sources, corrections) are dated and additive — the INDEX narrates;
  it never silently rewrites its own history.
