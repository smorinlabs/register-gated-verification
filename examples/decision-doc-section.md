# Exhibit — decision-document section (one complete question)

One full question from the data-area decision document (`design/A8-data-decisions.md`, S1 — the area's crux), shown at the completed-staff-work bar the register demands (D013/D023/D030): cold-start **background**, **chronological corpus evidence** with the recency rule applied, **externally verified practice**, a **recommendation with its mechanism worked out**, and a **runner-up with the reason it lost** — all *before* the human is asked anything. Bound decisions are declared up front so the question's edges are explicit. Genericized per repo policy; content otherwise verbatim.

**Outcome**: ratified as recommended → **D056·S1** (the operator, 2026-06-12, one-by-one with the area's five sibling questions).

---

> **Status**: v1 DRAFT for review (2026-06-12) — decision document per D013/D023/D030; questions are asked ONLY from this document, after reading. Mining: A8-data-mining.md.
> **Bound (not open here)**: C4/D029 (per-env Artifact Registry — the isolation question is CLOSED, only repo *lifecycle* is open); D006 (engine TF ≥ 1.11 — ephemeral values + write-only args are in-scope engine features); D010/D017/D018/D019 (**state buckets are NOT reopened here**); D043·M2 (sa-app = per-secret accessor, never project-wide); D053·P5 (deploy track pushes `:{git-sha}` per env registry — the churn S3 must manage); … Template = defaults + knobs (D001); web facts accessed 2026-06-12 (D047·FLAG-4).

## S1 — Secret WRITE path: how values get into Secret Manager (the area's crux)

**Background.** Three candidate writers exist for secret *values*: (a) Terraform itself, safely possible only since ephemeral values/write-only args (the DR3/D006 pattern — `secret_data_wo` never touches plan or state); (b) the v1 operator-script flow (`populate-secrets.sh` + `secrets-map.toml` + `secrets.sh generate` → `secrets.auto.tfvars`); (c) operator-manual `gcloud secrets versions add` (or console). The choice interacts with two ratified postures: A7 §2's **"env secrets: none"** (the CI lanes are keyless and carry no value channels — a clean SOC 2 story), and D043·M7/A4 (humans act only via impersonation, audited). Note what the corpus flows were actually *for*: v1's script moved **SA emails** into Secret Manager so tfvars could impersonate — WIF made that payload extinct (A3 §9). The surviving need is app runtime secrets (Plaid/Stripe/OAuth — g6).

**Corpus (chronological, D016).** g4 (12-13): the full shell flow, values transiting `secrets.auto.tfvars` into apply — meaning plan/state exposure, plus a gitignored plaintext file on the operator disk. DR3 (12-14, one day later — the evolved position): ephemeral/write-only is "the intended solution"; tfvars-transit is the thing it exists to kill. DR4 (2026-01-01): the secrets unit exists per env; `secrets.sh` survives only as *validate presence/mapping*. The corpus's own arc: **keep the inventory idea, kill the value transit**.

**Verified practice (mining).** The write-only path is implementable TODAY: `google_secret_manager_secret_version.secret_data_wo` + `secret_data_wo_version` (google provider ≥ 6.23.0; TF ≥ 1.11 = D006 floor) — GA resource surface, not beta. The ephemeral READ resource also ships. So D006's premise holds and no fallback-to-script-flow trigger fires. But observe: write-only args solve *values-through-Terraform*; they do not answer *where CI would get the value from* — a TF-owned write path in CI would force secret values into GitHub environment secrets, breaching A7 §2's ratified no-env-secrets posture.

**Recommendation — SPLIT OWNERSHIP: Terraform owns containers + IAM; operators own values (impersonated gcloud, inventory-driven).**

- The **secrets unit** creates `google_secret_manager_secret` containers + the per-secret accessor `_member`s (A4 §3) from a committed per-env **inventory map** — `secrets-map.toml`'s descendant, now `secrets` in `live/{env}/env.hcl` (include channel, D047·FLAG-2/D050·R2 precedent; the secret SET is instance data and never lives in catalog/, D036):

```hcl
  secrets = {                       # env.hcl — names only, NEVER values; drives containers + accessor IAM
    "api-db-url"          = {}      # {} = defaults: accessor sa-app, labels from the env map
    "sync-plaid-secret"   = {}
  }
```

- **Values enter out-of-band, before first real deploy**: `gcloud secrets versions add {name} --data-file=- --impersonate-service-account=sa-terraform-{env}@…` — keyless (infra-admins' tokenCreator, A4 §5 row 5), audit-logged, runbook-owned (ops guide C2: populate step at genesis between step 6a and first app deploy; validate recipe = every inventory name has an ENABLED version — DR4's surviving `secrets.sh validate` role). No value ever exists in git, tfvars, state, plan, or CI.
- **Write-only args = the sanctioned in-Terraform write mechanism** where an instance *wants* TF to mint a value (e.g., ephemeral `random_password` → `secret_data_wo`, bump `secret_data_wo_version` to rotate): documented knob on the secrets module, default off. Never `secret_data` (plaintext state — banned in catalog code).
- **Retired**: `populate-secrets.sh` (payload extinct under C6), `secrets.auto.tfvars` + `secrets.sh generate` (value transit through tfvars — the DR3-condemned mechanism). Console-manual writes: discouraged, not blocked (same IAM surface; drift-visible via the validate recipe).
- **Who writes what, when** — the complete writer table:

| Act | Writer | Identity | When |
|---|---|---|---|
| Create containers + accessor IAM | secrets unit (TF) | `sa-terraform-{env}` via WIF/impersonation | step 6a, every apply (idempotent) |
| First values populate | operator, `gcloud versions add` | human impersonating `sa-terraform-{env}` (A4 §5 row 5) | genesis runbook, after 6a, before first real deploy |
| Rotation (add version) | operator, same command | same | any time; see S2 for propagation |
| TF-minted values (opt-in) | secrets unit via `_wo` knob | `sa-terraform-{env}` | instance choice only |
| Inventory change (add/remove a secret) | env.hcl edit → PR → secrets unit apply | normal infra lane (A7 §3) | steady state |

Removal semantics ride the map: deleting an inventory entry destroys the container (and its versions) on the next apply — `deletion_policy` stays provider-default `DELETE`; prod protection is the review gate on the PR, not a resource flag (secrets are re-creatable from the runbook, unlike Firestore data — contrast S6).

**Runner-up**: full TF-owned values in the secrets unit via write-only args + ephemeral variables. Modern and state-clean, but every CI plan/apply of the secrets unit needs the value channel populated → GitHub env secrets return, A7 §2's no-secrets story breaks, and rotation couples to infra applies. Kept as the documented instance escape hatch (it is the same module knob as above); rejected as template default.

---

## Why this shape earns the gate

- **The question is answerable cold.** A reader with no session memory gets the candidate writers, what the old scripts were actually *for*, and which prior decisions box the answer in — before any evidence is weighed.
- **The recency rule is visible, not implied**: g4's flow and DR3's condemnation of it are dated one day apart, and the document says which one is the evolved position and why the surviving fragment (validate) survives.
- **Verified practice is load-bearing, not decorative**: the provider surface is named to the resource-argument level with its version floor — and the verification produced the *counter-argument* (write-only args don't answer where CI gets the value), which is what actually decided the question.
- **The runner-up is a real option**, kept alive as a documented escape hatch — so a future override has its evidence pre-assembled.
