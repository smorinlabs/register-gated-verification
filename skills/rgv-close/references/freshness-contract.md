# Freshness-Pass Agent Contract (fill-in template)

You are running the GLOBAL FRESHNESS PASS for a completed corpus — re-verifying every parked
verify-item against live authoritative sources before freeze. You are independent of the authors.
Today is {DATE}. Sanctioned oracles: {ORACLE_LIST}. Citations carry ACCESS DATES ONLY.

READ FIRST: {REGISTER_PATH} (the register); the design docs at their current versions; the parked
verify-item inventory below.

VERIFY EACH ITEM — verdict per item, no exceptions:
  CONFIRMED  — the claim holds as written (cite where verified)
  CHANGED    — reality moved (state exactly what changed)
  UNRESOLVED — could not verify (state why; never guess)
{ITEM_LIST — one numbered row per parked item, each naming its claim AND its oracle}

RULES (the sanctioned fact-fix exception — pattern.md §3):
- CHANGED + a design doc states the stale fact → EDIT that doc directly: minimal touch, patch-version
  bump, "freshness pass per {D-ENTRY}" header note, PRESERVE ratified-status lines.
- CHANGED but only the watch list referenced it → record only.
- FACT-CLASS ONLY: anything larger than a fact-fix is a FLAG for the human — never a design change.
- The report enumerates every edit; the pass's ratification gate reviews them.

WRITE: {REPORT_PATH} — item-by-item verdicts w/ citations, edits made per doc, flags, and a
freeze-ready YES/NO verdict line.
REPORT BACK: verdict counts, edits per doc, flags, freeze-ready verdict.
