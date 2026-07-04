# Exhibit — executed amendment ledger

The networking area's cross-document amendment ledger (`design/A5-networking.md §10`), shown in its **executed** state. The two-phase discipline: the derivation *lists* every cross-document amendment its decisions imply — it never applies them (a derivation has zero authority over other docs). A separate sweep, running under its own contract, applies the ledger across the corpus with version bumps, and proves application with grep post-conditions. The ledger heading then carries the execution record: date, trigger (the register entry), and every touched doc's new version.

Content verbatim from the source project; register/section IDs real.

---

> ## 10. Amendment ledger — EXECUTED 2026-06-12 at post-ratification propagation (D047): A1 v10 · A2 v2.5 · A3 v2.2 · A4 v3
>
> - **A4 §2.2**: `compute.networkAdmin` trimmed, 12→11 roles; the "trim candidate" flag resolves (§3; F2 coverage verified and CLOSED; register entry amends D043·M1).
> - **A4 §9**: + row `compute.skipDefaultNetworkCreation` · org (Phase 1) · enforce (§3).
> - **A4 §1/§7/§11-1(b)**: + `certDnsAuthReader` org role + per-env `_member`; Phase-1 script gains the second role-create (§2; register entry D046/D047 — sanctioned reads 1→2).
> - **A4 §13**: both Area-5 handoff items resolve (cert method §2; networkAdmin confirm §3).
> - **A2 §1/§4/§5**: the five row deltas of §7 (module name, dependency edges, read count 1→2).
> - **A2 §3.2/§3.4** (added per D047·F1): env.hcl example gains `domain` + the `edge` knob map; §3.4 gains the FLAG-2 include-channel sentence (unit wrappers read env.hcl include-level constants directly).
> - **A2 §4 dns-records row**: the `dns-auth-records` + `dns-service-records` split carried as NAMED RECOMMENDATION, final call Area 7 (D047·FLAG-1).
> - **A2 §6**: steps 6–7 gain the first-genesis interleave / two-apply dns convergence (§9).
> - **A1/A2/A4 — F5 sweep (added per D047·F5)**: every "12-role"/"12 roles" reference → 11 (A1 §2/§4.3/§7/§10; A2 §4/§5/§6/§10; A4 §1/§2/§3/§10/§11), incl. A1 §10's and A2 §10's `6/6/12` handoff notations and A4 §9's knob-matrix "(enforcement → Area 5)" footnote resolution.
> - **A3 §1/§5/§9**: `{component}` tokens ratified (+`proxy`, `wild`; `-v6` address-family modifier — D047·F6); Area-5 name table delivered; cert example row `api-cert` → `lb-cert` (§7) — the v2.2 touch.
> - **A1 §10**: the Area-5 handoff line resolves (cert-validation method decided, §2).

---

## The grep post-conditions

The sweep's contract ends with machine-checkable exit criteria (see instruction-anatomy §1: "VERIFY before finishing: grep X (0 expected), grep Y (≥1)"). For the F5 count-sweep above — the ledger item most prone to silent partial application, because it names **eleven** scattered locations — the post-conditions take this shape (reconstructed from the ledger's enumerated touch points; the original commands ran in the sweep agent's session):

```bash
# F5: no live "12-role" claim may survive in the three swept docs
grep -n "12-role\|12 roles\|6/6/12" design/A1-foundations.md design/A2-iac-structure.md design/A4-iam.md
# expected: 0 hits outside history parentheticals that cite the amendment (D046/D047)

# the new role + policy row must now exist in every home the ledger names
grep -c "certDnsAuthReader"                    design/A4-iam.md          # expected ≥ 3 (§1, §7, §11-1(b))
grep -n  "skipDefaultNetworkCreation"          design/A4-iam.md          # expected: the §9 org row
grep -n  "domain\|edge = {"                    design/A2-iac-structure.md # §3.2 example gained both

# version bumps prove the touch reached every named doc
head -5 design/A1-foundations.md design/A2-iac-structure.md design/A3-naming.md design/A4-iam.md \
  | grep -n "v10\|v2.5\|v2.2\|v3"
```

A post-condition that fails means the ledger was not fully applied — and the sweep report must say so rather than claim completion.

## Coda — what happens when this discipline slips

This exact invariant class is where the pattern's crash-recovery property was later exercised. The role count amended here (12→11) was re-amended by a later gate (11→12, D051), and *that* sweep's ledger claimed coverage of "A1 §2/§4.3/§7" — but the cross-area zoom-out (D062, Finding 1, HIGH) found A1 §2's prose still reading 11, three lines below a diagram reading 12, with **two executed ledgers on record claiming the line was covered**. Ledger said what should exist; grep said what did; the delta was provable without any session memory. The fix was one line — because it was caught before a single runbook was derived from the bad prose. The ledger + post-condition pair doesn't make sweeps infallible; it makes their failures *detectable and adjudicable*, which is the actual contract.
