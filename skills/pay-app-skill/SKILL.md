---
name: pay-app-skill
description: End-to-end Pay App lifecycle skill for PMs across Stitser BUILT operating companies (BUILT, CRF LLC, Formation Homes). Covers project budget setup (G-702, G-703, Baseline Budget Items, Create First Pay App trigger), recurring monthly Pay Apps (stamp PA-N, load invoices from Excel + Drive, retention release, change orders, parent/child invoice splits, lock workflow), per-invoice PM audit (value, completion, documentation), and maintenance (cost-code backfills, stale-record cleanup). Trigger on phrases like import this budget, load the SOV, kick off Pay App #1, import Pay App #N, process pay app invoices, split this invoice across scopes, release retention, apply change order, lock the pay app, audit invoice, fix cost codes, stand up new project from hardbid, get this job into S-Bos, reconcile Pay App vs Excel, or any phrase touching the budget → invoice → pay-app → close chain.
---

# Pay App Skill — Stitser BUILT Pay App Lifecycle

Single skill for Project Managers at BUILT (KCS Homes LLC), CRF LLC, Formation Homes, and any Stitser BUILT operating company. Covers the full Pay App lifecycle: project budget setup → Pay App #1 → recurring monthly Pay Apps with retention/change orders → per-invoice PM audit → close.

This document supersedes the earlier `Pay App SKILL.md`, `SKILL.md` (invoice-import), and `budget-import/Budget Import SKILL.md`. Every rule from those three skills appears here exactly once.

## When to use

Trigger when the user wants to:

- Stand up a new project from an estimator's Excel, hardbid PDF, or transcribed SOV
- Kick off Pay App #1 (the "Create First Pay App" automation trigger)
- Import a recurring Pay App #N (parse Excel, match Drive invoice files, stamp PA, create bills, link to G-703 lines)
- Allocate existing project invoices to G-703 line items
- Split a bundled invoice (one wire covering multiple scopes) into per-scope children
- Release retention on a final Pay App
- Apply a Change Order and a CO-driven mid-project G-703 line addition
- Reconcile a Pay App between Excel and SmartSuite (cost-code-by-cost-code)
- Audit an individual invoice before approving it (PM-level review)
- Lock or recover a Pay App that was accidentally locked
- Patch cost codes after accounting populates new Intacct codes
- Convert a stipulated-sum project into a spec-home line-item structure (or vice versa)

If the user only wants to create one line item or one invoice, skip the skill and call the SmartSuite create directly. The value is in the multi-record orchestration and the audit guarantees.

## Apps & Field Reference

Always call `smartsuite_get_app_schema` on each app before writing — slugs drift over time.

### G-702 (header / contract rollup) — `68a8c3d2bba73ca6e62d0cb5`

| Field | Slug | Notes |
|---|---|---|
| Project | `s12698a7c3` | linked record → Projects |
| SB Company Collecting Revenue | `s338ec3e01` | linked record → **Intacct Location Records** (NOT Companies/Entities) |
| Children (G-703) | `sp14ysu8` | inverse-side link, force-update to trigger rollup |
| Bills & Invoices | `s5f5b897f4` | linked record → Bills & Invoices |
| Budget Actions | `s8a81dec45` | multi-select; `["7L6Rf"]` = "Create First Pay App" |
| Improvement Type | `sdcf171890` | enum |
| Original Contract Sum | `s8560fefff` | formula (read-only) |
| Other rollup formulas | `sf1daf8d5a`, `s3f1c37afe`, `s146614652` | tie-out targets |
| Comment Status | `s9bee3e3d6` | formula (read-only) — DO NOT confuse with `s8a81dec45` |

### G-703 (line items) — `68db71a363e88ace0bd45439`

| Field | Slug | Notes |
|---|---|---|
| G-702 | `s076d5facd` | linked record (direct side) |
| Project | `s2a56fe76c` | linked record |
| Cost Code | `s955e34d4f` | linked record → Intacct Cost Codes |
| Vendor | `se6527712f` | linked record → Companies/Entities (NOT Intacct Vendor ID table) |
| Phase | `s60b9b16c5` | textfield (free-text in current schema) |
| x-Cost Code | `s3cfe5c30e` | numeric cost code for sorting/matching |
| SF/Units | `s518300c77` | currency |
| UW Material Cost/SF | `sebf9d079c` | currency — see Phase 1 §1.13 (Pattern A vs B) for repurposing |
| UW Labor Cost/SF | `sc8f0c85b2` | currency — see Phase 1 §1.13 (Pattern A vs B) for repurposing |
| Orig. Contract | `s40aa4fa79` | formula (Material × Units + Labor × Units) |
| Current Period Pay App | `sjumyime` | linked record → Pay Apps |
| Link to Bills & Invoices | `sb6xxwi6` | reverse link from invoices |
| CO Increase / Deduct (reverse links) | `sbf823a077` / `sdcd13ebea` | from CO Line Items |

### Baseline Budget Items — `69bb89ebf6a195c2c73a3b3e`

| Field | Slug | Notes |
|---|---|---|
| Account | `s32eed8560` | text — **drives the title formula** (the `title` you send is ignored) |
| Baseline Budget | `s818f40f1d` | currency |
| Cash Flow Section | `s8ee35f579` | enum (`x0IWR`, `ozQle`, `wExoS`) |
| Project | `s2ba7b261b` | linked record → Projects |

### Pay Apps — `68db724638a208d3257ea3a9`

| Field | Slug | Notes |
|---|---|---|
| Project | `sfa1b454d9` | linked record |
| G-702 Current Period | `s67quh1s` | linked record → G-702 (may be reverse-link; see Phase 2A) |
| Bills & Invoices in This Period | `s5bea81732` | linked record array |
| Application # | `s8a78edfe0` | text (e.g. `"1"`, `"2"`); **needs manual patch after PA-1 stamps** |
| Pay App Date | `sd28f22e3a` | date (period-through date) |
| Total Pay App $ / Total Completed this Period | `sfpilbou` | formula (read-only rollup of linked invoice amounts) |
| Previous Pay App | `s0hsbxxt` | linked record → Pay Apps (previous PA) — distinct from `s67quh1s` |
| PDF Pay Apps | `s2fb649b95` | file (combined PDF package) |

### Bills & Invoices — `68a8c3d2bba73ca6e62d1297`

| Field | Slug | Notes |
|---|---|---|
| Title | `title` | auto-formed |
| Invoice Name | `invoice_name` | text |
| Amount | `amount` | currency — **gross amount this period only** |
| Date of Invoice | `sd0332a63d` | date |
| Project | `s77950695a` | linked record |
| Line Item | `s0ef26247a` | linked record → G-703 |
| G-702 | `sj2lepts` | linked record → G-702 |
| Vendor | `s0bcc3e24a` | linked record → Companies/Entities |
| Retention % | `sf74e1bbd4` | percent — **WHOLE NUMBERS** (5 = 5%, not 0.05) |
| Bill No | `s3e3663e67` | text |
| Linked Pay App | `spsrmdjk` | linked record → Pay Apps |
| Memo | `s83e0052b4` | textarea — `PA#{N} - CC {code} {description}` |
| Status | `sa8a9d535f` | status — defaults to "backlog" |
| G-Drive File URL | `s5fd21f93c` | link — **REQUIRED for sub invoices** |
| Remove for Export | `s3fdf4e04c` | text — `"Remove for Export"` for GC allocations & material waivers |
| Cost Code (lookup) | `sbed918344` | linked record (PDF→invoice match aid) |
| File Attachment | `s01a9d36b7` | file (deprecated — use Drive URL) |
| Sub Invoice Retention | `scca5daaac` | formula (retention $) |
| Parent Invoice Being Split | `s7c5dbbb94` | linked record → Bills & Invoices (parent) |
| Children (split) | `sad3605fad` | linked record → Bills & Invoices (children) |
| Invoice Split Check | `s10ed6c569` | formula |
| Sum of Child Invoices | `saf7e93e52` | formula |
| Type | `type` | text (`"Invoice"`, `"Payment"`) |
| Read-only after lock | `sf3266ba5d` | NOT writable — see Common Errors |

### Change Orders & CO Line Items

CO Line Items reverse-link to G-703 via `sbf823a077` (Increase) and `sdcd13ebea` (Deduct). Confirm exact slugs against the schema; CO header amount is a formula off the line items.

### Intacct Cost Codes — `68dd6553caad578dc367ee61`

| Field | Slug | Notes |
|---|---|---|
| Cost Code | `s1059017f6` | text — 5-digit zero-padded |
| Name | `s85b8b6e83` | text |

### Intacct Location Records — `6914fb94e53085946b899cb0`

| Field | Slug | Notes |
|---|---|---|
| 4-digit Location code | `sa6ba30854` | text |
| Name | `s085617500` | text |

### Companies/Entities (vendors) — `68216a706900e8eaf75a05c0`

| Field | Slug | Notes |
|---|---|---|
| Name | `title` | text |

### S-Bos Activity Log — `69dc55333fe841263503f235`

| Field | Slug | Notes |
|---|---|---|
| Summary | `s85fec4906` | text |
| Source Record | `s0cef0cec3` | linked record |
| Reasoning | `sfddfe3ab3` | textarea |

---

## Phase 1 — Initial Budget & SOV Setup (one-time per project)

This phase stands up the project skeleton: G-702, G-703 line items, three Baseline Budget Items, and Pay App #1.

### 1.1 — Cost Code Translation (SOV → Intacct)

SOV sheets use **4-digit codes with optional letter suffixes** (e.g. `3600`, `8200A`). Intacct wants **5-digit codes with a leading zero** (e.g. `03600`, `08200`).

Transform:

1. Strip any trailing letter (A/B/C…).
2. Left-pad with zero to 5 digits.
3. Look up in the Cost Codes app via `s1059017f6` (text) or `title contains`.
4. If not found, **do not fail** — write the raw SOV code into the description as `FLAG: cost_code=<4-digit> not in Intacct` and carry on. Missing codes get collected and emailed at the end (§1.9).

See **Reference Tables → CRF Manufactured Home cost code reference** for resolved Intacct cost code IDs.

### 1.2 — Title Uniqueness Prefix

G-703 enforces unique titles across the tenant. Many projects have "03600 Framing" as a line, so the second project errors on insert.

Rule: every line-item title is `{PROJECT_CODE} - {description}`, e.g. `CRF-FLT - 32100 Home + Options`. Project code is short (3–5 chars) and unique per project. Examples observed: `LL-SQTI` (Lazy Lizard Squeeze Inn TI), `CRF-FLT` (CRF Fleetwood), `CRF-NV` (CRF North Virginia).

### 1.3 — Phase Mapping

Phase (`s60b9b16c5`) is a **textfield in the current schema, not an enum** — it accepts free text. Common values seen: `PreCon`, `Manufactured Home`, `Site Work`, `Exterior`, `Interior`, `MEP`, `Construction`, `Final`, `Professional Services`, `Sales Costs`, `Margin`. Use whatever clearly groups the line; the Phase field powers grouped views.

(Historic note: an earlier schema had Phase as an enum. If you ever see Phase reject a value, drop back to grouped textfield convention and log `FLAG: phase=<raw> unmapped`.)

### 1.4 — Amount Format

SmartSuite currency fields round-trip as **decimal strings with up to 6 implied decimal places** (e.g. `499493.160000`). Never write comma-formatted strings. Cast to Decimal, quantize to 2 places, then serialize:

```python
from decimal import Decimal, ROUND_HALF_UP
amt = Decimal(raw).quantize(Decimal("0.01"), rounding=ROUND_HALF_UP)
payload = f"{amt:.6f}"  # e.g. "499493.160000"
```

Plain `"269308.00"` also works for write — the system accepts either format.

### 1.5 — Linked Record Resolution

Four link fields on G-703 must be resolved from text to record IDs **before** the create call:

| Field | Slug | Target app |
|---|---|---|
| Project | `s2a56fe76c` | Projects |
| G-702 | `s076d5facd` | G-702 |
| Cost Code | `s955e34d4f` | Cost Codes |
| Vendor | `se6527712f` | Companies/Entities |

All take arrays of record IDs. A missing Vendor or Cost Code lookup must not block the import — write `FLAG:` into the description and leave the field empty.

### 1.6 — Force the G-702 Rollup

After creating G-703 children via the direct link (`s076d5facd` → G-702), the G-702's inverse-side field (`sp14ysu8`) and its rollup formulas often stall (we've seen them stuck for 10+ minutes).

Fix: immediately after the batch create, do an explicit `smartsuite_update_record` on the G-702 with `sp14ysu8` = **the complete array of all child G-703 IDs** (including any that were already there). This triggers instant recalc. Verify `sf1daf8d5a`, `s8560fefff`, `s3f1c37afe`, `s146614652` all equal the expected total to the penny.

### 1.7 — Multi-Select Fields Need Arrays

Single-select and multi-select look identical in the UI but require different payload shapes. Multi-select requires an array even for a single value: `["7L6Rf"]`, not `"7L6Rf"`.

Beware formula fields that look like enums — on G-702, `s9bee3e3d6` is *Comment Status* (formula, read-only) while `s8a81dec45` is *G702-Budget Actions* (the real multi-select). Writing to the wrong slug silently returns an empty string with no error. **Always confirm field `type` via the schema before writing.**

### 1.8 — Baseline Budget Items

Every imported project needs three records in the Baseline Budget Items app (`69bb89ebf6a195c2c73a3b3e`).

**Critical:** the title field is **auto-formed by formula from `s32eed8560` (Account text field) + project name**. The `title` field you send is ignored. Set `s32eed8560` explicitly:

| `s32eed8560` Account | `s818f40f1d` Amount | `s8ee35f579` Cash Flow Section |
|---|---|---|
| `Revenue` | SOV contract total / Sales Price | `x0IWR` |
| `Gross Profit` | Total margin built into line items | `x0IWR` |
| `Job Costs` | Internal cost target (Material Total sum) | `ozQle` |

Other Cash Flow Section codes seen: `wExoS` (Required Debt/Equity in capital stack work). Use `ozQle` for any cost-like baseline (Purchase Price, Carry Costs, Improvement Cost).

**Before creating, search the app for the project — stale baselines from prior partial imports are common.** If found with wrong numbers, soft-delete via DELETE endpoint (Phase 5).

### 1.9 — Missing-Item Capture & Email

Every `FLAG:` entry written during the import is an item accounting/admin needs to fix in Intacct. At the end of the run:

1. Grep descriptions of all created G-703 records for `FLAG:` lines.
2. Group into **Missing Cost Codes** and **Missing Vendors**.
3. Draft a Gmail with HTML tables to the current accounting contacts (Lisa Coleman <lisa@built-nv.com>, Linda Davila <linda@built-nv.com> as of 2026-04; confirm names before sending).
4. Subject pattern: `{Project} — missing Intacct cost codes & vendor records needed`.
5. **Save as draft, never auto-send.** Surface the draft URL to the user.

### 1.10 — CRUD Approval Gate (Guiding Spec §9)

Before any bulk write that touches more than 3 records, present a summary to the user:

- Count of records to create / update / delete per app
- Dollar total for amount-bearing records
- The first 2–3 titles as a sanity check

Then wait for explicit confirmation. This rule comes from the project's Guiding Spec and is non-negotiable. It applies to every phase below as well.

### 1.11 — Revenue-Collecting Company (Bill-To) on G-702

The G-702 has a linked-record field identifying **the entity collecting revenue on this contract** — i.e., the Stitser BUILT operating co. that invoices the client. It must be populated **on the G-702 at creation time** (the Pay App automation reads it at stamp time and writes it onto the Pay App; if blank on the G-702, blank on the Pay App, and every downstream AIA form is blank).

**The slug is `s338ec3e01` and it links to the Intacct Location Records app (`6914fb94e53085946b899cb0`), NOT Companies/Entities.** Each Intacct Location has a 4-digit code in slug `sa6ba30854`. Common values:

| Code | Entity | Record ID |
|---|---|---|
| `1100` | KCS Homes LLC dba BUILT | `6914fe61e127b5f69fb770da` |
| `1330` | CRF, LLC | `69c9686c08fe517f9db38db2` |

Search by code:

```json
{
  "app_id": "6914fb94e53085946b899cb0",
  "filter": {
    "operator": "and",
    "fields": [{"comparison": "is", "field": "sa6ba30854", "value": "1330"}]
  }
}
```

**Validation step:** after the initial G-702 create, immediately fetch it back and verify `s338ec3e01` is non-empty. We've observed silent drops on the initial create — if empty, run `smartsuite_update_record` to set it explicitly before proceeding to §1.12.

After Pay App #1 is stamped, verify the same chain on the Pay App record. If blank, do not proceed to send invoices — investigate, don't patch.

### 1.12 — Budget Actions Trigger — "Create First Pay App"

Once §1.6 shows the G-702 rollup matches the SOV total to the penny, set `s8a81dec45` on the G-702 to `["7L6Rf"]` (multi-select array). The label for that code is **"Create First Pay App"** — it fires the automation that stamps Pay App #1 with all defaults, G-702 linkage, and period/date fields.

**Never create a Pay App record directly via `smartsuite_create_record`.** Doing so produces a Pay App missing the bindings the automation sets, and downstream AIA forms, period roll-forward, and cash flow calcs will be silently wrong. If a Pay App was created manually by mistake, delete it and use the trigger instead.

Do **not** set the trigger before:

- §1.6 verification passes (rollup ties)
- §1.11 verification passes (revenue-collecting company is populated on the G-702)

After the trigger fires, see §1.15 — the Pay App # field needs a manual patch.

### 1.13 — Stipulated Sum vs Spec Home (Pattern A vs Pattern B)

Two distinct contract patterns coexist. The G-703 line-item structure must match. Tag the G-702's Improvement Type field (`sdcf171890`) accordingly.

#### Pattern A — Stipulated Sum (manufactured-home, prefab, pre-priced jobs, traditional TI hardbids)

Customer pays a fixed contract; profit is built into individual line items. There's no separate Labor scope.

| Existing G-703 field | New meaning |
|---|---|
| `sebf9d079c` UW Material Cost/SF → Material Total | **Internal cost target** — what we want to spend, what vendor invoices should add to |
| `sc8f0c85b2` UW Labor Cost/SF → Labor Total | **Margin** — built-in profit per line, NOT actual labor |
| `s40aa4fa79` Orig. Contract (formula = Material × Units + Labor × Units) | **Customer scheduled value (retail)** — Pay Apps draw against this |

Improvement Type → `Manufactured Home — Stipulated Sum` (or similar with "Stipulated Sum" suffix). Works for traditional TI hardbid as well — just label it appropriately.

**Example (9555 Fleetwood):** 32100 Home + Options — Material $269,308 (Skyline cost), Labor $59,720 (margin), Orig. Contract $329,028 (customer billed).

#### Pattern B — Spec Home (CRF self-funded flips, Acquire→Rehab→Sell)

CRF (or whichever Stitser BUILT co. is doing the flip) builds and sells to a third-party buyer; the operating co. is BOTH contractor AND customer. The G-702 contract sum = sales price.

Required line additions on top of the rehab SOV:

| Cost Code | Description | Material | Labor | Notes |
|---|---|---|---|---|
| 02100 | Property Acquisition / Purchase Price | purchase $ | 0 | See cost code reference — uses 02100 placeholder; ask accounting for a dedicated Property Acquisition code |
| 14010 | Sales Commissions (e.g. 3.5%) | base + bonus | 0 | |
| 14020 | Closing Costs | closing $ | 0 | |
| 17900 | Contingency 5% | rehab × 5% | 0 | |
| 18000 | Spec Home Margin (Sales Price − Costs) | 0 | computed | Labor side; Material=0 |

Math: `Sales Price = Σ Material (cost) + 18000 Labor (margin) = G-702 Original Contract Sum`. Tune the 18000 Labor amount to make the G-702 tie exactly to the resale price.

Improvement Type → `Manufactured Home — Stipulated Sum` (works for prefab spec) or `Spec Home`.

**Example (10300 N Virginia):** 29 rehab lines + 17900 contingency $17,984 + 02100 acquisition $199,000 + 14010 commissions $23,275 + 14020 closing $1,600 + 18000 margin $62,571 = **$665,000** sales price.

#### Choosing between A and B

- External customer paying a fixed contract → Pattern A
- Operating co. flipping the home to a buyer → Pattern B
- Confirm with user before proceeding; structures are not interchangeable.

### 1.14 — Auto-Link Existing Project Invoices to G-702 + Pay App #1

Many projects already have Bills & Invoices linked to the project record (slug `skyyzwjd` on Projects) before the G-702 exists — historic invoices the team logged as an expense tracker. The G-702 import must back-link them.

**Why:** without this, Pay App #1 ships with zero linked invoices and the team has to manually link 20–130+ records. We've seen this cause multi-day delays.

Rule:

1. Before creating the G-702, fetch the project and read `skyyzwjd` — that's the array of all invoice IDs already linked.
2. After §1.6 force-rollup verifies and §1.12 stamps Pay App #1, do TWO updates:
   - G-702 `s5f5b897f4` = full invoice ID array
   - Pay App #1 `s5bea81732` = same full array
3. Verify by re-reading both. The Pay App's `sfpilbou` (Total Pay App $) formula should now sum the linked invoice amounts.

The auto-link automation appears to fire only sometimes — Fleetwood auto-linked 20 invoices, Virginia auto-linked zero on the same code path. The automation also won't fire when a project gets a SECOND G-702 (CO contract or replacement). Always do the explicit back-link defensively regardless.

After the back-link, allocate each invoice to its specific G-703 line per Phase 2C.

### 1.15 — Pay App # Manual Patch

After the §1.12 trigger fires, the Pay App is created but `s8a78edfe0` (Application #) comes back empty. The title formula reads `PA--{Project}-...` instead of `PA-1-{Project}-...`.

Fix: immediately after the Pay App is created, run:

```python
smartsuite_update_record(
  app_id="68db724638a208d3257ea3a9",
  record_id=<pay_app_id>,
  fields={"s8a78edfe0": "1"}
)
```

The title formula then re-renders to `PA-1-{Project}-{ImprovementType}-G702-{PayAppDate}`. Repeat with `"2"`, `"3"`, etc. on subsequent Pay Apps (see Phase 2A).

### Phase 1 End-of-run checklist

- [ ] G-702 rollup totals (4 formula fields) equal SOV total / Sales Price to the penny
- [ ] Child count on G-702 matches the SOV line count exactly (including spec-home additions if Pattern B)
- [ ] Revenue-collecting company (bill-to) populated on G-702 (§1.11)
- [ ] All three Baseline Budget Items created with correct `s32eed8560` Account labels (§1.8)
- [ ] No stale baselines or other orphan records left for the project (Phase 5)
- [ ] Pay App #1 created via §1.12 trigger, **not** manually
- [ ] Revenue-collecting company populated on the stamped Pay App (§1.11)
- [ ] Pay App `s8a78edfe0` Application # patched to `"1"` so title reads `PA-1-...` (§1.15)
- [ ] Existing project invoices auto-linked to G-702 (`s5f5b897f4`) and Pay App #1 (`s5bea81732`) per §1.14
- [ ] Each invoice has `s0ef26247a` Line Item populated (Phase 2C)
- [ ] Bundled-scope invoices split into parent/child where appropriate (Phase 2D)
- [ ] Pattern A vs B selected per §1.13 and Improvement Type tagged accordingly
- [ ] All `FLAG:` lines collected and missing-items email drafted (§1.9) or confirmed N/A
- [ ] Activity Log entry written with totals, line count, duration, flag counts
- [ ] Concise status summary returned to the user with `app.stitserbuilt.com` links

---

## Phase 2 — Recurring Pay App Lifecycle (monthly per project)

Once Phase 1 is complete and Pay App #1 is locked, every subsequent month brings a new Pay App import. This phase is the workhorse — covers Pay App #2 through final retention release.

**Prerequisites for this phase:** Phase 1 complete (Project, G-702, G-703, all vendors, all cost codes, Pay App #1 exist). If any are missing, drop back to Phase 1.

### 2A — Stamping the next Pay App (PA-2, PA-3, …)

#### Parse the Pay App Excel

Pay App Excel workbooks follow the AIA G-702/G-703 format. The key data lives in a "Schedule of Values" or "G-703" tab containing:

- **Cost Code** — 4-5 digit code (may need zero-padding to match Intacct format per §1.1)
- **Description** — line item description
- **Scheduled Value** — original contract amount for this line
- **Work Completed Previous Applications** — cumulative through prior PAs
- **Gross Amount This Period** — the amount being billed THIS period (this is the invoice amount)
- **Retainage** — typically 5% for subcontractor work

Parsing approach:

1. Use Python with openpyxl to read the Excel file
2. Identify the G-703 tab (look for headers like "Cost Code", "Description", "Scheduled Value")
3. Extract line items, focusing on rows where `gross_amount_this_period > 0`
4. Also extract the G-702 summary data: application number, period-through date, contract totals
5. Save parsed data to a JSON file for reference and auditing

**Critical:** the `amount` field on every invoice record must be the **gross_amount_this_period**, NOT the scheduled_value or the cumulative total. This was a real error encountered on Squeeze Inn — setting invoice amounts to scheduled values inflates the PA total by orders of magnitude.

#### Drive folder structure

Pay App invoice files live in Google Drive under a nested per-project / per-PA structure:

```
Construction Pay Apps/
  └── {Project} (TI or Shell)/
      └── Pay App #{N} {date}/
          ├── 1 - Inv/                ← Original invoices as received
          ├── 2 - PM Approved/        ← Approved copies (USE THESE for linking)
          └── 3 - Pay App Invoices/   ← Combined PDF goes here
```

Use `2 - PM Approved/` as the source of truth for linking — these are the approved copies. See Phase 3 for the PM-level approval criteria that move a file from `1 - Inv` to `2 - PM Approved`.

#### Create / locate the Pay App record

Before creating invoices, create or confirm existence of the Pay App record. **Do not create the Pay App via direct create** — for PA-2+, the system normally rolls forward from PA-1. If a fresh PA isn't already stamped, fall back to:

```python
fields = {
  "s8a78edfe0": "{pay_app_number}",          # e.g. "2"
  "sd28f22e3a": {"date": "YYYY-MM-DDT00:00:00Z", "include_time": False},
  "sfa1b454d9": ["{project_record_id}"],      # Project link
  "s0hsbxxt":   ["{previous_pa_record_id}"],  # Previous PA link (if PA#2+)
}
```

The `s5bea81732` (Invoices in This Period) field gets populated AFTER all invoices are created (§2I).

#### PA ↔ G-702 link pattern

The fields `s67quh1s` (G-702 Current Period) and `s0hsbxxt` (Previous Pay App) on Pay Apps may be **reverse-link fields that don't accept direct writes**. If an update returns empty arrays in the response, the linkage may need to happen from the G-702 side or be managed by automation. Test with a single write first and verify by re-reading.

### 2B — Loading invoices into the Pay App

#### Identify which lines need invoices (including GC self-perform)

Only create invoice records for G-703 lines that have **non-zero work this period** (`gross_amount_this_period > 0`).

**CRITICAL — GC Self-Perform Allocations:** the Excel Pay App contains TWO categories of line items with work this period:

1. **Vendor invoices** — work billed by subcontractors (Demo, Concrete, Plumbing, Electrical, etc.)
2. **GC self-perform allocations** — internal cost allocations by the GC (Superintendent, Project Management, GL Insurance, Overhead & Profit, Signs, Internet, Water, Protection, Cleaning, Dumpsters, Small Tools, Fuel, Concrete Tubs, etc.)

**Both categories need invoice records in SmartSuite.** On the Shell PA-1 import, only vendor invoices were initially created, leaving $13,296.26 in GC allocations missing from the pay app. The SmartSuite total didn't match Excel until all GC allocation records were added.

GC allocation records differ from vendor invoices:

- **Vendor**: always BUILT/KCS Homes (`68b9e31b723f6dd75bbd5c6b`), or the Stitser BUILT operating co. equivalent
- **Retention**: 0% (omit `sf74e1bbd4` or set null)
- **Remove for Export**: set `s3fdf4e04c` to `"Remove for Export"` — internal allocations should appear in SmartSuite tracking but NOT on the AIA PDF sent to the owner
- **G-Drive File URL**: typically empty — GC allocations don't have individual invoice PDFs
- **Memo format**: `PA#{N} - CC {code} {description}` (same as vendor invoices)

**Split items:** some cost codes have BOTH a vendor receipt AND a GC allocation in the same period. Example: if Excel shows $400 total for Protection but there's a vendor receipt for $200, the GC allocation record should be the DIFFERENCE ($200), not the full Excel amount. Always check existing invoice records for a cost code before creating a GC allocation.

**Don't create $0 invoices.** Shell PA-2 initially created invoices for ALL G-703 lines (including those with $0 work), which inflated the invoice count and confused the PA total.

#### Resolve G-703 line items by cost code

Each invoice must link to its G-703 line. To find the right G-703:

1. Search the G-703 app for records linked to the target G-702.
2. Match by cost code — compare `s3cfe5c30e` (x-Cost Code) field or search by title containing the cost code.
3. Some projects use prefixed titles like `LL-SQTI - 03600 Framing` — search by title containing the cost code digits.

**Gotcha:** SmartSuite's text search only supports "contains" comparison. When searching by cost code in title, use the zero-padded format (e.g., `"01110"` not `"1110"`). If searching by the numeric cost code field, use the raw number.

**Gotcha:** SmartSuite pagination is unreliable — the API always reports `total=100` and `has_more=false` regardless of actual count. Either use targeted filter searches for specific records (preferred) or paginate manually: offset 0, 100, 200… until items returned < 100.

#### Resolve vendors to Companies/Entities

The vendor field on Bills & Invoices (`s0bcc3e24a`) links to the **Companies/Entities** table, NOT the Intacct Vendor ID table. Same pattern as the G-703 vendor field.

To resolve a vendor:

1. Look up the vendor ID (e.g., `ENHA001`) in the `full_vid_to_eid.json` mapping file if available.
2. Or search Companies/Entities (`68216a706900e8eaf75a05c0`) by name.
3. The entity_id from the mapping goes into the linked record field as an array: `["entity_id_here"]`.

**Self-perform items** (work done by the GC) use the GC's entity ID. For Stitser BUILT, BUILT/KCS Homes is `68b9e31b723f6dd75bbd5c6b`. CRF, Formation, etc. each have their own entity ID — see Phase 1 §1.11.

**If a vendor is not found:** flag in the memo and leave the field empty. Do not block the import.

#### Collect & match Google Drive invoice files

Every sub-contractor invoice record must be linked to its source PDF in Google Drive. Drive links are the standard — do NOT use SmartSuite file attachments (`s01a9d36b7` is deprecated for new work).

Step-by-step:

1. **Ask the user for the Drive folder URL** (`https://drive.google.com/drive/folders/{folder_id}` or `https://drive.google.com/open?id={folder_id}`).
2. **List all files in the Drive folder.** Use Chrome browser tools (navigate to URL, then `get_page_text` or `read_page`) to enumerate every file. Record file name and Drive file ID.
3. **Open and read each file to identify it.** For each PDF, extract: vendor/company name, cost code (if present), invoice number, dollar amounts, date. Common naming convention: `{cost_code} {vendor_name} {invoice_number} APRVD.pdf` (e.g., `15800 NNM 2113 Shell APRVD.pdf`).
4. **Match each file to a G-703 line item.** Match priority: vendor name → dollar amount → cost code → invoice number. High confidence = vendor + amount both match a single line. If only one signal matches or multiple line items could match, flag for user review.
5. **Build the matching table and present it for review.** Wait for user confirmation or manual corrections before proceeding.
6. **Construct shareable Drive links:** `https://drive.google.com/file/d/{file_id}/view?usp=sharing`. This goes into `s5fd21f93c`.

**If a file cannot be matched:** flag in the summary and ask the user. If the user says it's not an invoice (lien waiver, cover letter), skip it.

**If a line item has no matching file in Drive:** still create the invoice record (Excel is the source of truth for amounts), but leave `s5fd21f93c` empty and add a note to the memo: `FLAG: No Drive file found for this invoice`. The user can upload and link later.

**One file → multiple line items:** some vendors submit a single invoice covering multiple cost codes. Link the same Drive URL to all invoice records that stem from that vendor's invoice. Note in the memo (e.g., `"Vendor invoice covers CC 08300 and CC 08310"`).

**GC self-perform items** typically do NOT have individual PDFs in Google Drive — they're billed internally. Only sub-contractor invoices will have Drive PDFs.

**Fuel receipts** (CC 01660) may have Costco receipts — match by date in filename. Multiple receipts mapping to the same invoice record: link the first one.

#### Create invoice records

For each line item with work this period, create a Bills & Invoices record:

```python
fields = {
  "amount":       "{gross_amount_this_period}",       # CRITICAL: this-period only, string form
  "sd0332a63d":   {"date": "YYYY-MM-DDT00:00:00Z", "include_time": False},
  "s77950695a":   ["{project_id}"],                   # Project
  "s0ef26247a":   ["{g703_line_item_id}"],            # G-703 line item
  "sj2lepts":     ["{g702_id}"],                      # G-702
  "s0bcc3e24a":   ["{vendor_entity_id}"],             # Vendor
  "sf74e1bbd4":   5.0,                                # Retention % — WHOLE NUMBERS
  "s83e0052b4":   "PA#{N} - CC {code} {desc}",        # Memo
  "spsrmdjk":     ["{pay_app_id}"],                   # Link to Pay App
  "invoice_name": "{sub_invoice_number}",
  "s3e3663e67":   "{bill_number}",
  "s5fd21f93c":   "{google_drive_file_url}",          # REQUIRED for sub invoices
  "s3fdf4e04c":   "Remove for Export",                # ONLY for GC allocations & waivers
}
```

**G-Drive File URL (`s5fd21f93c`):** required on every sub invoice record. Format: `https://drive.google.com/file/d/{file_id}/view?usp=sharing`. GC self-perform allocations typically won't have Drive files.

**Retention rules — WHOLE NUMBERS ONLY:**

The `sf74e1bbd4` field is a SmartSuite `percentfield`; the internal formula divides by 100:

- **5 = 5% retention** (correct)
- **0.05 = 0.05% retention** (WRONG — produces near-zero retention)
- **10 = 10% retention** (correct)

On Shell PA-1, retention was initially entered as `0.05` instead of `5`, producing $0.04 retention on a $42,970 invoice instead of the correct $2,148.50. Payment Due showed $87,558.99 instead of the correct $80,211.82.

Retention by record type:

- Subcontractor invoices: 5% (`sf74e1bbd4: 5.0`) — or whatever the pay app specifies
- GC self-perform allocations: 0% (`sf74e1bbd4: null` or omit)
- Materials supplier waivers: see §2G
- Check the pay app for line-specific retention percentages — some may differ

**Batch creation:** SmartSuite's `smartsuite_bulk_create_records` is unreliable — frequently returns HTTP 500 even with small batches (5-13 records) and especially when any record carries a multi-paragraph description. **Always fall back to individual `smartsuite_create_record` calls.** Slower but reliable.

#### Vendor assignment logic

Different line items map to different vendors based on the G-703 structure:

| Line Item Type | Vendor | How to identify |
|---|---|---|
| Subcontractor work | The sub (e.g., Enhanced Electrical, NNM HVAC) | Has a vendor assigned on the G-703 |
| GC self-perform | BUILT/KCS Homes (`68b9e31b723f6dd75bbd5c6b`) or the operating co. | GC labor, general conditions, overhead |
| Overhead & Profit | BUILT/KCS Homes (or operating co.) | Always the GC |
| Contingency | BUILT/KCS Homes (or operating co.) | Always the GC |
| Material-only purchases | The supplier (e.g., Ferguson Enterprises) | Check the G-703 vendor field |

The G-703 record's `se6527712f` field already has the correct vendor entity linked. Read this field from the G-703 and copy it to the invoice's `s0bcc3e24a`. Ensures consistency between budget and invoices.

### 2C — Linking invoices to G-703 lines (Invoice Allocation)

After the G-702/G-703/Pay App stack exists and invoices are back-linked (Phase 1 §1.14), each invoice's `s0ef26247a` Line Item field must point to the right G-703 line. This unlocks per-line cost tracking and budget-vs-actual reporting.

**Heuristic order — apply in this priority:**

1. **Vendor exact match** to SOV scope (most reliable):
   - `Skyline Homes`, `Champion Home Builders` → 32100 Home + Options
   - `Triple-B Transportation` → 32115 Shipping
   - `Constanza Receiving and Setting` → 32200 Construction Home Setting
   - `Carnes Engineering`, `Element Engineering` → 17030 Engineering and Permits
   - `Cooney Enterprises` → varies by scope; usually requires Phase 2D split
   - `Eagles Heating and Air Conditioning` → 15800 AC
   - `GCO Carpet Outlet`, `Sierra Interiors` → 09600 Flooring
   - `Stewart Title`, `Landmark Title Assurance` → 14020 Closing Costs (spec) or split into 02100 + 14020 via Phase 2D
   - `Washoe County Treasurer` → 24000 Past Due Taxes
   - `Washoe County Building Dept`, `City of Fallon` (permits) → 17030 Engineering and Permits *(per Clint convention; not 01020)*
   - `Adaptive Environmental Consulting`, `Washoe AQMD` (asbestos) → 17030 Engineering and Permits *(per Clint convention; NOT 24000 PreCon)*
   - `Sierra Waste Solutions`, `Www Dumpster.com` → 01510 DEMO Dumpster
   - `Home Depot`, `Reno Paint Mart`, `MinuteKey` (Lowe's) → 32200 Construction Home Setting *(per Clint convention on manufactured-home jobs)*; on traditional rehabs use 27000 Materials if line exists
   - `LR Handyman`, `Steven Andrades` → 27000 Materials/labor or DEMO catch-all
   - `NODS (Gas Test)` → 17030 (utility-permit related)
   - `Bedrooms / Extra Boxes` → 32100 Home + Options
   - `Gas Line Contractor` → 02200 Site Work Utilities

2. **Memo / invoice_name keyword fallback:**
   - "permit" → 17030 (CRF convention) or 01020 if line exists
   - "shipping" / "transport" → 32115
   - "setting" / "set" → 32200
   - "foundation" / "concrete" / "lot prep" → 02200
   - "flooring" / "carpet" / "LVP" / "SVP" → 09600
   - "AC" / "HVAC" / "furnace" → 15800
   - "well" / "pump" → 11715
   - "septic" → 11730
   - "tax" / "Q1 / Q2 Property Tax" → 24000 Past Due Taxes (Virginia-style); or 32110 if it's NV Sales Tax on the manufactured home
   - "engineer" / "site plan" / "structural" / "asbestos" → 17030
   - "purchase" / "acquisition" / "closing" → 02100 (purchase) or 14020 (closing) — split via Phase 2D

3. **Bundled "inclusive" contracts:** when a single sub's contract is intentionally inclusive rather than line-item granular (Cooney foundation + driveway + utilities + septic + power conduit all on one $39k contract), split via Phase 2D, but route the small sub-scopes (utility connections, septic, well work) onto the dominant inclusive line (e.g., 02200 lot prep / driveway) rather than spreading into $0-budget granular lines. Set the convention with the user; don't infer.

4. **Acquisition/closing invoices that aren't in the rehab SOV** belong on Pattern B's `02100 Property Acquisition` and/or `14020 Closing Costs` lines, often via a Phase 2D split (the wire is one transaction but the dollars are two scopes — see 10300 N Virginia $201,049.77 → $199,000 acquisition + $2,049.77 closing).

5. **Don't add invoice notes/descriptions during bulk allocation** — the bulk update endpoint chokes on long descriptions. Use the existing memo or leave blank.

**Execution:** loop sequential `smartsuite_update_record` calls (NOT bulk). Set `s0ef26247a` = `[<G703 record ID>]` per invoice.

### 2D — Splitting bundled invoices (Parent/Child)

When ONE wire/check covers MULTIPLE scopes (e.g., property acquisition + closing fees in a $201,049.77 wire; or a $39,385 sub invoice covering foundation + driveway + demo + utilities), use the parent/child split mechanism instead of forcing the whole amount onto one G-703 line.

**Why:** preserves the original transaction record (with file attachment for audit) AND maps spend to the right per-line budgets.

Procedure:

1. **Update the parent invoice record:**
   - Clear `s0ef26247a` (Line Item) → `[]`
   - Update `invoice_name` to prefix `[SPLIT] ` and append a parenthetical referencing the children (e.g. `[SPLIT] Cooney Inv #1175 (parent — see N children for scope breakdown)`)
   - **Keep the original amount, attachment (`s01a9d36b7`), Pay App link, G-702 link, project link, vendor link.** All audit fidelity stays.

2. **Create N child invoices** (one per scope), each with:
   - `invoice_name` = `<Vendor> #<original BillNo> - <Scope description> (split <A|B|C…>)`
   - `amount` = scope dollar amount (string format, §1.4)
   - `type` = `"Invoice"` (or `"Payment"` matching parent)
   - `s77950695a` (Project) = same as parent
   - `s0ef26247a` (Line Item) = the G-703 record ID for this scope
   - `sj2lepts` (G-702) = same as parent
   - `spsrmdjk` (Pay App) = same as parent
   - `s7c5dbbb94` (Parent Invoice Being Split) = `[<parent record ID>]` ← the link
   - `s0bcc3e24a` (Vendor) = same as parent

3. **Update G-702 `s5f5b897f4` and Pay App `s5bea81732`:** remove the parent ID, add all N child IDs. Prevents double-counting in the Pay App total. Verify Pay App `sfpilbou` is unchanged after the swap (sum of children = parent amount).

4. **Verify Σ children = parent amount to the penny.** Bills & Invoices has `s10ed6c569` (Invoice Split Check) and `saf7e93e52` (Sum of Child Invoices) formulas that surface mismatches.

**Worked example (Cooney $39,385 → 6 children, 10300 N Virginia, May 2026):**

| Child | Scope from invoice line items | $ | Target G-703 |
|---|---|---|---|
| A | Foundation + concrete + plans + mob + dirt for fdn | 24,160 | Foundation (02200) |
| B | Driveway + clear&grub + finish grade | 7,000 | lot prep, base rock, new septic |
| C | Demolition | 5,000 | DEMO (01510) |
| D | Septic system + extensions | 850 | rolled to inclusive line B per user direction |
| E | Primary power conduit | 1,125 | rolled to inclusive line B per user direction |
| F | Water from well | 1,250 | rolled to inclusive line B per user direction |

Sum of children = $39,385 = parent. Where the contract is "inclusive" (one trade covering many small site-work scopes) the user may direct rolling D/E/F onto the dominant line — not a generalizable rule, but a pattern to ask about.

### 2E — Retention Release patterns (CRITICAL — learned on PA-6)

Final Pay Apps frequently include a retention release — historically held retention from prior periods is now being billed.

The Excel pay app shows retention release as either:

- A **negative retention amount** on the line that's releasing (i.e., previously withheld retention is now negative because we're paying it out), with the gross amount this period at the released retention dollar amount, OR
- A **separate retention release line** with its own row

**Rules for retention release invoices:**

1. The invoice `amount` field is the **retention dollars being released** (this-period gross). NOT zero.
2. The retention % field (`sf74e1bbd4`) is **0** for the release invoice itself (you're releasing it, not withholding more).
3. The memo must explicitly say `RETENTION RELEASE — PA#{N} - CC {code} {desc}` so AP doesn't double-pay.
4. The invoice should usually be flagged `s3fdf4e04c = "Remove for Export"` — depends on what the AIA form is supposed to show. Check with the user. The G-702 itself shows the retention release at the header level, so subordinate retention-release line invoices are sometimes not exported. **Confirm convention with user before locking.**
5. There's an automation that's *supposed* to fire when a Pay App is marked Final — it didn't fire on PA-6 we saw. Always do the explicit retention release invoice creation defensively.

If the project has BOTH:

- Cumulative work-in-place at 100% on a line, AND
- The line still shows non-zero retention held in prior PAs

…then the line is a candidate for retention release. Walk every G-703 line on the final PA and confirm with the user before stamping releases.

### 2F — Pay App Comparison & Verification Workflow

After completing the import, perform a cost-code-by-cost-code comparison between Excel (source of truth) and SmartSuite. Catches discrepancies a simple total-to-total check misses.

Steps:

1. Parse the Excel pay app to get a table of: cost code, description, gross amount this period, retention %, retention $, net amount.
2. Fetch all invoice records linked to the Pay App in SmartSuite.
3. Group SmartSuite invoices by their G-703 line item (cost code).
4. For each cost code, compare:
   - **Gross amount**: Excel this-period vs sum of SmartSuite invoice amounts for that cost code
   - **Retention %**: Excel retention rate vs SmartSuite `sf74e1bbd4`
   - **Retention $**: Excel retention $ vs SmartSuite formula `scca5daaac` (Sub Invoice Retention)
   - **Net amount**: Excel (gross − retention) vs SmartSuite (amount − retention)
5. Flag any discrepancy > $0.02 (allow for rounding).

**Common discrepancy patterns:**

- Missing GC allocations (gross too low) → Create GC allocation records per §2B
- Double-counted supplier waivers (gross too high) → Unlink per §2G
- Wrong retention % (retention $ wrong) → Fix `sf74e1bbd4` to whole number per §2B
- Wrong amount (net invoice vs gross) → Amount should be gross, not net-of-retention
- Retention release missing → See §2E
- CO line not yet added → See Phase 4

This workflow was developed after the Shell PA-1 import where the initial "fix" only addressed retention and one supplier waiver, but missed $13,296.26 in GC allocations across 13 cost codes. A line-by-line comparison caught every discrepancy.

### 2G — Materials Supplier Waivers (don't double-count)

Materials suppliers (e.g., Ferguson Enterprises on TI, Main Electric Supply on Shell) sometimes provide conditional lien waivers uploaded to SmartSuite as invoice/bill records. However, these material amounts are **already included** in the subcontractor's pay request — the sub buys materials from the supplier and bills the GC for the full amount including materials.

**If a materials supplier waiver record exists AND its amount is already included in the sub's invoice for the same cost code, the waiver record must NOT be linked to the Pay App.** Linking it causes double-counting.

Fix for double-counted waivers:

```python
smartsuite_update_record(
  app_id="68a8c3d2bba73ca6e62d1297",
  record_id="{waiver_record_id}",
  fields={"spsrmdjk": []}
)
```

Also set `s3fdf4e04c` to `"Remove for Export"` to flag these records.

**How to detect:** compare the Excel pay app total for a cost code against the sum of SmartSuite invoice records linked to that cost code's pay app. If SmartSuite is higher, look for a supplier waiver whose amount explains the difference.

Shell PA-1 example: Main Electric Supply had a $17,211.20 conditional waiver linked to the Shell pay app, but that amount was already in Enhanced Electrical's $18,061.25 sub invoice for cost code 16000 Electrical. Unlinking Main Electric Supply's record brought the line from $35,272.45 down to $18,061.25, matching Excel.

### 2H — "Remove for Export" Flag

The `s3fdf4e04c` field is a text field used to mark records that should exist in SmartSuite for tracking purposes but should be excluded from PDF export (AIA G-702/G-703 forms sent to the owner).

Records that should have `s3fdf4e04c = "Remove for Export"`:

- **GC self-perform allocation records** (§2B) — internal cost allocations
- **Materials supplier waiver records** (§2G) — documentation-only, amounts already in sub invoices
- Some retention release invoices (§2E) — confirm convention with user

**Known issue:** setting this field during `smartsuite_create_record` sometimes returns an empty string in the response even though the value may have been saved. After creating records, verify by reading back. If it didn't save on create, update it separately via `smartsuite_update_record`.

### 2I — Update Pay App with Invoice IDs

After ALL invoices are created, collect every invoice record ID and update the Pay App's `s5bea81732` field with the complete array:

```python
smartsuite_update_record(
  app_id="68db724638a208d3257ea3a9",
  record_id="{pay_app_id}",
  fields={"s5bea81732": ["{inv_id_1}", "{inv_id_2}", ..., "{inv_id_N}"]}
)
```

**Critical:** do this in a SINGLE update with ALL invoice IDs. If invoices are created in batches (e.g., a subagent creates 9, then 5 more manually), collect ALL IDs and write the complete array. Partial updates cause `sfpilbou` to reflect only a subset.

The TI PA-2 import hit this exact issue: a subagent created 9 of 14 invoices and updated the PA, then 5 more were created manually but the PA wasn't updated until a second pass caught it.

### 2J — PDF Blending Workflow

After invoices are imported and linked to Drive files, generate a combined PDF package for the AP reviewer.

1. **Verify individual Drive links are in place.** Confirm all sub-contractor invoice records have `s5fd21f93c` populated. GC allocations may be empty.

2. **Blend invoice PDFs.** Use `scripts/blend_pay_app_invoices.py`:

   ```python
   from blend_pay_app_invoices import merge_pdfs_from_files, generate_invoice_index
   result = merge_pdfs_from_files(pdf_file_paths, "PA2_Combined_Invoices.pdf")
   index = generate_invoice_index(result["file_order"], "Pay App #2")
   ```

3. **Sort by cost code** — the script extracts cost codes from filenames and sorts numerically.

4. **Upload combined PDF** to the Google Drive `3 - Pay App Invoices` folder inside the relevant Pay App folder.

5. **Update the Pay App record** description with links to the Drive folders so the reviewer can access:
   - The G-702/G-703 report (printed pay app)
   - The invoice index (generated by the blending script)
   - The combined invoices PDF

6. **The `s2fb649b95` field** (PDF Pay Apps) is a file-type field designed to hold the final combined package. Direct file upload through the SmartSuite API requires binary upload capability.

### 2K — Review, Approve, Lock workflow

Once Pay App comparison (§2F) shows zero discrepancy and PM audit (Phase 3) is complete on every invoice, the Pay App moves through Review → Approve → Lock.

**Review:** PM (or designated reviewer) walks the printed G-702/G-703 PDF and the combined invoice index. Confirms:

- G-702 contract sum matches the executed contract
- This-period totals tie to the source Excel
- All sub invoices have an approved waiver attached (Phase 3C)
- Retention is correct per Subcontract terms

**Approve:** the Pay App Status field moves to `Approved`. Surface to AP.

**Lock:** once the AIA G-702/G-703 PDF is signed and the customer pays, the Pay App is locked. Locking has these effects:

- Bills & Invoices `sf3266ba5d` and similar lock-mirror fields become read-only
- Editing locked invoices throws "field not writable" — this is by design
- The next Pay App (PA-N+1) cannot reference a locked PA's invoices via the open-PA lookup; lock first, then create the next PA

**Accidental Lock Recovery:**

If a Pay App was locked early (e.g., before retention release was added, or with bad amounts), recover via:

1. Identify the locked PA. Confirm with user before unlocking.
2. Walk the related G-703 lines and Bills & Invoices — note any post-lock changes the user wants to preserve.
3. Use the SmartSuite UI to unlock (the API doesn't have a clean unlock — `smartsuite_debug_api` may be needed, but UI is safer). Unlocking is a privileged operation.
4. After unlock, re-run §2F comparison and §2I to ensure invoice array is complete.
5. Re-lock once corrected.

**Never delete a locked PA.** Even soft-deleting a locked PA breaks downstream cash flow records. Always unlock → fix → re-lock.

### 2L — Completed & Stored Invariants

After lock, the following must remain true and be checked periodically:

- **Completed**: every G-703 line that the customer has accepted as 100% complete shows the sum of (this-period invoice amounts) across all PAs equal to scheduled value (or scheduled value + CO adjustments).
- **Stored**: any G-703 line being billed via stored materials (rather than work-in-place) has the correct stored-vs-installed split. Stored materials usually have 0% retention; check with subcontract terms.
- **Retention held**: the cumulative retention dollars on the G-702 = sum of `scca5daaac` across linked invoices, less any retention release amounts (§2E).
- **Total billed to date**: G-702 cumulative billed = Σ(`amount`) across all linked invoices. Any drift means an invoice was created without linking to the PA.

If any invariant breaks, drop back to §2F and reconcile before moving forward.

### Phase 2 End-of-run checklist (per Pay App)

- [ ] **Invoice count matches**: number of invoices created = number of G-703 lines with non-zero work this period (BOTH vendor invoices AND GC allocations)
- [ ] **PA total matches Excel**: Pay App's `sfpilbou` matches Excel total within $0.02
- [ ] **All invoices linked to PA**: PA's `s5bea81732` contains exactly the right number of invoice IDs
- [ ] **Vendor links correct**: spot-check 2-3 invoices to confirm vendor entity IDs match G-703 assignments
- [ ] **Amounts are this-period only**: not scheduled value, not cumulative
- [ ] **Retention is correct**: spot-check that sub invoices have 5% (value=5, not 0.05) and GC allocations have 0%
- [ ] **No $0 invoices** unless intentionally tracking transfers
- [ ] **Date set correctly**: all invoices have the period-through date
- [ ] **No double-counted supplier waivers**: any materials supplier records either unlinked or genuinely additive
- [ ] **GC allocations flagged**: all GC self-perform records have `s3fdf4e04c = "Remove for Export"`
- [ ] **Drive links populated**: every sub-contractor invoice has `s5fd21f93c`. GC allocations may be empty. If more than 2 sub invoices are missing Drive links, flag as incomplete.
- [ ] **Drive links resolve**: spot-check 2-3 URLs by opening
- [ ] **Cost-code-level comparison done**: §2F shows no discrepancies > $0.02
- [ ] **Retention release lines correctly stamped** (if final or retention-release PA) per §2E
- [ ] **Per-invoice PM audit complete** (Phase 3)
- [ ] **Combined invoices PDF uploaded** to `3 - Pay App Invoices` and linked from PA description
- [ ] **Status moved to Approved/Locked** per §2K

---

## Phase 3 — Per-Invoice PM Audit (PM-level review) [PLACEHOLDER — to be filled in later]

This phase covers the PM-level audit each individual Bill/Invoice record passes before it's approved and added to the Pay App. The three sub-areas below are stubs — they capture the existing PM process at a high level and explicitly flag what Clint will refine later.

**Audit happens between §2B (invoices created) and §2K (Pay App locked).** A failed audit on any single invoice should hold up that invoice (move to a blocked state, add notes) without blocking the rest of the Pay App.

### 3A — Value Verification [TBD]

The PM confirms the invoice amount is reasonable before approving the line. Today this includes spot-checking against the budgeted line value, comparing against schedule of values % complete vs project schedule % complete (work earned vs work billed), and watching for duplicate billing across PAs (same vendor + same scope already billed in PA-N-1). The PM also looks for unit-rate vs lump-sum discrepancies on T&M-flavored work and confirms that any change-order-driven work has a corresponding executed CO before the line is approved.

**TODO (Clint to add):** specific dollar-or-percent thresholds for auto-approve vs require-manager-review, formulas for % complete vs % billed comparisons, red-flag triggers (e.g., "any line where this-period > 30% of remaining contract"), and where the audit notes get attached in SmartSuite (Bill memo, separate audit field, or Activity Log entry).

### 3B — Work Completion Verification [TBD]

The PM confirms work was actually performed before approving payment. Today this involves: a recent site visit (within X days of pay-app date), photo evidence linked to the invoice or to the project's photo log, schedule-milestone alignment (the work being billed is consistent with the construction schedule), super sign-off (Superintendent approves the % complete on the G-703 line item), and punch-list status for any line claiming 100% complete (no open punch items on closed-out lines).

**TODO (Clint to add):** which evidence is required at what threshold (e.g., site visit always; photos required when this-period > $5k; super sign-off in writing for lines >50% complete), where the evidence gets attached in SmartSuite (Bill record vs G-703 line vs Project photo log), and what the rejection workflow looks like when work-completion verification fails.

### 3C — Documentation Compliance [TBD]

The PM confirms required vendor documentation is on file before approving any pay request. Today the menu of required docs includes: W-9, Certificate of Insurance (General Liability + Workers Comp, named insured matches subcontract, expiration date in the future), conditional progress lien waivers (each PA), unconditional progress lien waivers (the period after each PA — covering the period that just got paid), conditional final lien waiver and unconditional final lien waiver at retention release, executed subcontract on file, contractor license verification, and certified payroll where prevailing-wage applies.

**TODO (Clint to add):** which documents are mandatory at first invoice vs every invoice vs at retention release only, how SmartSuite enforces the gate (Status field that won't advance, required-attachment field, automated check, manual PM signoff), what the "trust but verify" cadence is for items like COI expiration, and where waivers get filed in Drive (per-PA folder vs vendor folder).

(Phase 3 stubs intentionally do not invent compliance content — Clint will fill in detail on next pass.)

---

## Phase 4 — Change Orders

Change Orders modify the contract sum and the G-703 line structure mid-project. The existing skill content captures how to apply COs and add new G-703 lines mid-project; the bidirectional reverse-link recalc issue remains unresolved (see Lessons Learned).

### 4.1 — CO Header Amount Formula

The Change Order header has a formula that rolls up its CO Line Items (increase + deduct columns). When CO Line Items are created, the header should auto-populate; if it doesn't, force a recalc by updating the CO header with a no-op write.

The CO number on the header reads back as `"2.0"` instead of `"2"` in some integrations — when stamping CO numbers, write the integer string and don't be alarmed if reads come back with the trailing `.0` (see Common Errors).

### 4.2 — Adding a new G-703 line mid-project (CO-driven)

When a CO adds a scope that doesn't exist in the original SOV, a new G-703 line must be created. Steps:

1. Confirm the new cost code resolves to an Intacct cost code record (Phase 1 §1.1). If not, write a `FLAG:` and email accounting (§1.9).
2. Create the G-703 line per Phase 1 §1.5 (Linked Record Resolution) with:
   - `s076d5facd` G-702 = the project's existing G-702
   - `s2a56fe76c` Project = same project
   - `s955e34d4f` Cost Code = resolved Intacct cost code
   - `se6527712f` Vendor = relevant sub
   - `sebf9d079c` UW Material Cost/SF = CO line increase amount
   - `sc8f0c85b2` UW Labor Cost/SF = 0 (or margin per Pattern A convention)
   - `s518300c77` SF/Units = 1
   - Title = `{PROJECT_CODE} - {cost_code} {description} (CO #N)` to disambiguate from same-cost-code lines that might exist
3. Link the new G-703 line to the CO Line Item via `sbf823a077` (Increase) on the G-703.
4. Force-update the G-702 inverse-side `sp14ysu8` with the complete child array (Phase 1 §1.6) so rollups recalc.
5. Verify G-702 contract sum = original contract + Σ(CO net amounts).

### 4.3 — End-to-end CO workflow

1. **CO request received** — capture as a CO header record linked to the G-702.
2. **Create CO Line Items** — increase or deduct against existing G-703 lines, OR create new G-703 lines per §4.2.
3. **Reverse-link G-703 lines to the CO Line Items** — if the bidirectional sync stalls (see Lessons Learned), manually patch `sbf823a077` / `sdcd13ebea` on the G-703 side. Note: this sets the link but the Adjusted Actual Value formula recalc may require a UI touch to fire.
4. **Verify G-702 rollup** equals original contract + net CO amount.
5. **Stamp the next Pay App** (Phase 2) with the CO scope included. The next-period invoices for the CO scope link to the new (or modified) G-703 lines.
6. **Document the CO** in the project's CO log and attach the executed CO PDF.

### 4.4 — Unresolved: CO Line Item ↔ G-703 reverse-link recalc

When CO Line Items are created linking to G-703 rows via the CO LI's increase/deduct fields, the reverse link on the G-703 side syncs bidirectionally only for the first pair in some batches. Manually patching `sbf823a077` / `sdcd13ebea` on the G-703 side does set the link but does **not** trigger the Adjusted Actual Value formula recalc. Recalc requires a UI touch or an event path we haven't isolated.

**Workaround:** after CO line creation, walk the affected G-703 lines in the UI and trigger a recalc by editing then reverting any field. Or wait for an event (often the next Pay App stamp triggers it). **Do not assume CO numbers are correct on G-703 lines without verifying after a recalc.**

---

## Phase 5 — Maintenance & Corrections

### 5.1 — Cost code backfills

When accounting populates new Intacct cost codes that we previously flagged with `FLAG:`, walk the affected G-703 records and relink:

1. Search the G-703 records for `FLAG: cost_code=` in the description.
2. Look up the new cost code in Intacct Cost Codes app.
3. Update `s955e34d4f` on each G-703 record with the resolved cost code record ID.
4. Strip the `FLAG:` line from the description.
5. Confirm with accounting that the cost code is now active.

### 5.2 — Stale-record cleanup via DELETE endpoint

Prior partial imports leave orphan records with wrong numbers or wrong titles. Before creating, **search the target app for the project**.

Soft-delete via the DELETE endpoint (recoverable from trash):

```python
mcp__smartsuite__smartsuite_debug_api(
  endpoint="/applications/<app_id>/records/<record_id>/",
  method="DELETE"
)
```

Sets `deleted_date` automatically, records show `deleted_by`, and they can be restored from trash if needed. **Never use this without first showing the user the records you intend to delete (Phase 1 §1.10 approval gate).**

Common cleanup targets:

- Stale Baseline Budget Items from a prior `Create First Pay App` attempt
- Duplicate G-702 stubs
- Orphaned G-703 lines from a deleted G-702
- Partial Pay Apps from a failed PA-N stamp
- Bill/Invoice records created against deleted Pay Apps (orphans)

### 5.3 — Title / numeric repair when SOV definitions change

When an SOV gets re-issued with new line numbering or new descriptions mid-project (e.g., the customer reissues the contract with a CO baked in), the G-703 titles need a refresh. Procedure:

1. Diff the old SOV vs new SOV — identify renamed, renumbered, added, deleted lines.
2. For each renamed line: update G-703 `title` to match new convention (preserve project-code prefix per §1.2).
3. For each renumbered line: update `s3cfe5c30e` x-Cost Code if cost code numerics changed.
4. For added lines: create new G-703 records per Phase 4 §4.2.
5. For deleted lines: confirm zero invoices linked, then soft-delete per §5.2. If invoices are linked, do not delete — flag for reconciliation.
6. Force G-702 rollup per Phase 1 §1.6.

### 5.4 — Inclusive vs granular line consolidation

Some projects start out granular (one G-703 line per micro-scope) but the actual invoicing is inclusive (one sub bills lump-sum for all the micro-scopes). Pattern observed on Cooney @ 10300 N Virginia. **Not a hard rule per user direction** — confirm with user before consolidating, because inclusive billing does collapse cost-tracking granularity.

When the user directs consolidation:

1. Identify the dominant G-703 line that will absorb the smaller scopes.
2. For each smaller G-703 line: if any invoices linked, re-link them to the dominant line.
3. Adjust the dominant G-703's Material/Labor amounts to absorb the smaller lines' amounts.
4. Soft-delete the smaller G-703 lines per §5.2.
5. Force G-702 rollup per Phase 1 §1.6.

---

## SmartSuite Filter Quirks & Common Pitfalls

- **`is` comparison on currency fields fails** with `"Not allowed comparison"`. Use `contains` on title or filter on a related text field.
- **`is` comparison on number fields fails too** for x-Cost Code (`s3cfe5c30e`). Search by title `contains` instead — most G-703 lines have the cost code in the title.
- **Nested `or` groups inside an `and` filter** return `field is required` errors. Run multiple separate searches and merge in code.
- **`smartsuite_bulk_create_records` returns 500** when any record carries a multi-paragraph description. Loop single creates instead.
- **Bulk update is fine for short payloads** but switch to single updates if you hit a 500.
- **Egress is restricted.** `docs.google.com`, `cdn.filepicker.io`, and most third-party hosts are blocked. To fetch a sheet tab or PDF, use Claude in Chrome (auth'd browser) and pull via JS `fetch` against the source domain — not WebFetch.
- **SmartSuite pagination lies.** API always reports `total: 100` and `has_more: false` regardless of actual record count. Paginate manually offset 0, 100, 200… until items returned < 100.

---

## Common Errors & Fixes

### "Extra inputs are not permitted" on create_record
The `smartsuite_create_record` tool uses a `"fields"` key, not `"record"`. Correct format:
```json
{"app_id": "...", "fields": {"amount": 1234.56, ...}}
```

### Vendor field silently drops values
If `se6527712f` (G-703 vendor) or `s0bcc3e24a` (invoice vendor) returns an empty array, you're likely using record IDs from the **Intacct Vendor ID** table instead of the **Companies/Entities** table. These are different tables. The vendor fields link to Companies/Entities.

### Retention entered as decimal instead of whole number
If retention shows as 0.05% instead of 5%, you wrote `0.05` instead of `5`. The `sf74e1bbd4` percent field expects whole numbers — SmartSuite divides by 100 internally. Fix by updating to `5.0`. Single most impactful error on Shell PA-1.

### Retention release stamped with the wrong shape
On the retention-release line, `amount` should be the dollars being released and `sf74e1bbd4` should be `0` (you're releasing it, not withholding more). See Phase 2 §2E.

### Bulk create returns HTTP 500
`smartsuite_bulk_create_records` frequently returns HTTP 500 errors, even with small batches (5-13 records), and especially if any record carries a multi-paragraph description. **Do not retry bulk creates — fall back to individual `smartsuite_create_record` immediately.**

### Bulk update returns empty array
The `smartsuite_bulk_update_records` tool sometimes returns `[]` without applying changes. Always verify by reading back. Fall back to individual `smartsuite_update_record`.

### SmartSuite pagination lies
API always shows `total: 100` and `has_more: false` regardless of actual record count. Never trust these. Paginate offset 0, 100, 200… until returned items < 100.

### `s67quh1s` vs `s0hsbxxt` confusion
On Pay Apps, `s67quh1s` is **G-702 Current Period** (link to G-702) and `s0hsbxxt` is **Previous Pay App** (link to prior PA). Both may behave as reverse-link fields that don't accept direct writes. If a write returns an empty array, the linkage may need to happen from the other side or be managed by automation.

### "Remove for Export" silently dropped on create
The `s3fdf4e04c` text field sometimes doesn't persist when set during `smartsuite_create_record`. Verify after creation and update separately if needed.

### `sf3266ba5d` is not writable
On Bills & Invoices, `sf3266ba5d` mirrors Pay App lock status and is read-only. Don't try to write it directly. To unlock, unlock the parent Pay App.

### Drive link writes silently fail
`s5fd21f93c` is a link/URL field. If the Drive URL is malformed or missing the protocol (`https://`), SmartSuite may silently drop it. Always include the full URL with protocol. Verify by reading back.

### G-702 ↔ G-703 symmetric link lag
After creating G-703 children via `s076d5facd` → G-702, the G-702 inverse-side `sp14ysu8` and rollup formulas can stall for 10+ minutes. Force a recalc with an explicit `smartsuite_update_record` on the G-702 setting `sp14ysu8` to the complete child array (Phase 1 §1.6).

### Drive parents query unsupported
The Drive API doesn't support listing files by `parents` directly via our toolset in some cases — fall back to `search_files` with the folder name as a query, or to Chrome browser navigation + `get_page_text` to enumerate.

### CO number reads back as `"2.0"` instead of `"2"`
On read, the CO number field comes back with a trailing `.0` in some response paths (numeric formatting in serialization). Don't be alarmed — the integer is intact in storage. Always write the integer string (`"2"`, not `"2.0"`).

### Retention-release automation didn't fire (PA-6)
There's an automation that's *supposed* to stamp retention release invoices when a Pay App is marked Final — it didn't fire on at least one PA-6 we observed. Always create the retention release invoices defensively per Phase 2 §2E.

---

## Worked Examples

### Squeeze Inn @ RED — Tenant Improvement Hardbid (April 2026)

Pattern A. 43 SOV lines, contract $499,493.16. Project code `LL-SQTI`. 11 missing cost codes, 6 missing vendors → emailed to accounting. Baseline: Revenue $499,493.16, GP $36,777.27, Job Costs $462,715.89.

- TI G-702: `69dd6609083aa48c9b0421e6`
- TI PA-1: `69dd7f9bd7287b2ad6fdf4bc`
- TI PA-2: `69deaa86a7ec8f5cd6df788b`

**PA-2 detail:** 14 invoices created (9 by subagent + 5 manual). Total this period ~$127,906. Key subs: Enhanced Electrical ($4,320.60), EXXIA ($451.25), NNM ($115,490.52), Rick's AEC ($9.50). Self-perform: Progress Cleaning, Dumpsters, Small Tools, Overhead & Profit. **Lesson:** subagent created invoices for 9 of 14 lines and updated the PA, then 5 more were created manually but the PA wasn't updated until a second pass caught it. Drove Phase 2 §2I (always re-update PA with full ID array after every batch).

### Squeeze Inn @ RED — Shell Hardbid (Pay App #1, corrected April 2026)

- Total this period: $83,678.71 (retention $3,466.89, payment $80,211.82)
- Project: `687fbd6dca32ec50cc52d449`
- Shell G-702: `69ddc961d1dd1cc42a7c3b9b`
- Shell PA-1: `69ddc97916425a4094054ab5`
- Shell PA-2: `69deaa76b7872df40c8df604`

**PA-1 vendor invoices (4):** Demo $1,450, Concrete $42,970, Plumbing $6,856.50, Electrical $18,061.25 — all at 5% retention.
**PA-1 GC allocations (13):** Superintendent $2,888.67, PM $1,540.62, GL Insurance $2,119.09, O&P $5,846.79, Sign $161.29, Internet $129.03, Water $322.58, Protection $200, Cleaning $400, Dumpsters $274.19, Small Tools $258.73, Fuel $100, Concrete Tubs $100 — all at 0% retention, flagged "Remove for Export".
**Unlinked:** Main Electric Supply waiver ($17,211.20) — already in Enhanced Electrical's $18,061.25.

**Key lessons:**
1. Initial attempt only created vendor invoices, missing $13,296.26 in GC allocations.
2. Retention was entered as `0.05` instead of `5`, producing near-zero retention.
3. Main Electric Supply's conditional waiver was double-counting Electrical.
4. Total-to-total check masked these; only cost-code-by-cost-code (Phase 2 §2F) caught everything.

**PA-2:** 16 invoices (13 with non-zero + 3 incorrectly created at $0). Total this period ~$63,253 after removing $0 lines. Drove the rule: filter to `gross_amount_this_period > 0`.

### WL-CONS 677 Virginia TI — PA-1 → PA-7 (full pay-app chain through retention release)

Full lifecycle reference (see Activity Log for record IDs). Walked from initial budget import through 7 Pay Apps including final retention release. Surfaced Phase 2 §2E (retention release patterns) — the retention-release automation didn't fire on PA-6 and the release invoices had to be stamped manually with the right shape (`amount` = release dollars, `sf74e1bbd4` = 0, memo prefixed `RETENTION RELEASE`).

### 9555 Fleetwood Dr, Reno NV (Manufactured Home, Pattern A — Stipulated Sum, May 2026)

Pattern A. Project code `CRF-FLT`. 10 G-703 lines (HOME+OPTIONS bundled, NV TAX, SHIPPING, SETTING, ENG/PERMITS, SITE WORK, FOUNDATION, GARAGE, FLOORING, AC). Contract $545,454.54 = Material $475,651.64 cost + Labor $69,802.90 margin. 20 invoices auto-linked. Constanza Setting overrun ($14,498) and Skyline favorable ($8,891 vs $269,308 budget) surfaced via per-line allocation.

### 10300 N Virginia St, Reno NV (Manufactured Home, Pattern B — Spec Flip, May 2026)

Pattern B. Project code `CRF-NV`. 33 G-703 lines including 02100 Property Acquisition $199,000, 14010 Sales Commissions $23,275, 14020 Closing Costs $1,600, 17900 Contingency $17,984, 18000 Spec Margin $62,571. Contract = Sales Price = $665,000. 27 existing invoices needed manual back-link (auto-link didn't fire). Property Acquisition wire $201,049.77 split into 02100 ($199k) + 14020 ($2,049.77). Cooney $39,385 split into 6 scope children mapping to Foundation, Site Work, DEMO, with septic/power/water rolled into the inclusive site-work line per user direction (Phase 2 §2D worked example).

---

## Reference Tables

### CRF Manufactured Home / Spec Flip Cost Code Reference

Resolved Intacct cost code IDs as of May 2026 — confirmed in production for CRF flips/manufactured-home jobs. Use these directly when found; the IDs save a lookup round-trip. **Re-verify with `smartsuite_get_app_schema` + a search before relying on a hardcoded ID.** Cost codes get added/relabeled.

| Code | Description | Record ID | GL |
|---|---|---|---|
| 01020 | Building Permit | (search) | — |
| 01510 | DEMO Dumpster/Rubbish Removal | `68dd7841cedebebfadf1816e` | 5070 |
| 02100 | Clear & Grub *(also used as Property Acquisition placeholder)* | `68dd7841cedebebfadf1813e` | 5070 |
| 02200 | Excavation *(also Foundation, Site Work Utilities, Driveway)* | `68dd7841cedebebfadf18185` | 5090 |
| 06100 | Rough Carpentry *(Garage framing)* | `68dd7841cedebebfadf1823e` | 5090 |
| 09600 | Flooring | `68dd7841cedebebfadf181a5` | 5090 |
| 11715 | Well | `69f52898211edb3f55923d90` | 5090 |
| 11730 | Septic / Sanitary Treatment Equipment | `68dd7841cedebebfadf18245` | 5090 |
| 14010 | Sales Commissions *(spec home)* | (search/create) | — |
| 14020 | Closing Costs *(spec home)* | (search/create) | — |
| 15800 | HVAC | `68dd7841cedebebfadf181c3` | 5090 |
| 16000 | Electrical | `68dd7841cedebebfadf18171` | 5090 |
| 17030 | Structural Engineer / Permits / Asbestos | `69de9fee28bb32df52f6b5ca` | (no GL set as of 5/2026 — flag) |
| 17900 | Contingency | `68dd7841cedebebfadf1814a` | 5070 |
| 18000 | Spec Home Margin *(Pattern B only)* | (search/create) | — |
| 24000 | Past Due Taxes / HOA Default | `69f520db211edb3f55923d8c` | — |
| 26000 | Locksmith | `69f5227f211edb3f55923d8d` | — |
| 28000 | Legal (notices, eviction) | `69f52296211edb3f55923d8e` | — |
| 29000 | Cash For Keys | `69f522a2211edb3f55923d8f` | — |
| 32100 | Manufactured Home + Options | `69f51fb5211edb3f55923d89` | — |
| 32110 | NV Sales Tax | `69d713d52ae448597210ee91` | — |
| 32115 | Shipping (manufactured home) | `69f51fb8211edb3f55923d8a` | — |
| 32200 | Construction Home Setting | `69f51fe8211edb3f55923d8b` | — |

### Intacct Location Records (revenue-collecting companies)

| Code | Entity | Record ID |
|---|---|---|
| `1100` | KCS Homes LLC dba BUILT | `6914fe61e127b5f69fb770da` |
| `1330` | CRF, LLC | `69c9686c08fe517f9db38db2` |

(Other Stitser BUILT operating cos — Formation Homes, etc. — search the Intacct Location Records app by name to resolve.)

### Pay App Action Codes (`s7cc2e9e20` on G-702 — Budget Actions multi-select label IDs are stored on `s8a81dec45`)

| Code | Label |
|---|---|
| `7L6Rf` | Create First Pay App |

(Other action codes exist for subsequent triggers; confirm by reading the schema enum options.)

### Invoice Type Codes (Bills & Invoices `type` field)

| Value | Use |
|---|---|
| `Invoice` | Standard sub or supplier invoice |
| `Payment` | Payment record (less common in Pay App context) |

### Trigger Codes

| Trigger | Slug | Value | Effect |
|---|---|---|---|
| Create First Pay App | `s8a81dec45` (multi-select on G-702) | `["7L6Rf"]` | Stamps Pay App #1 |

### G-702 Record ID Appendix (known projects)

| Project | G-702 ID |
|---|---|
| Squeeze Inn TI | `69dd6609083aa48c9b0421e6` |
| Squeeze Inn Shell | `69ddc961d1dd1cc42a7c3b9b` |

(Walk the G-702 app for the full list as needed; this appendix is illustrative.)

### Key Entity IDs

| Entity | Record ID |
|---|---|
| BUILT/KCS Homes (vendor in Companies/Entities) | `68b9e31b723f6dd75bbd5c6b` |
| Squeeze Inn Shell Project | `687fbd6dca32ec50cc52d449` |
| Squeeze Inn Shell PA-1 | `69ddc97916425a4094054ab5` |
| Squeeze Inn Shell PA-2 | `69deaa76b7872df40c8df604` |
| Squeeze Inn TI Project | `687fbda0097f7311a3912603` |
| Squeeze Inn TI PA-1 | `69dd7f9bd7287b2ad6fdf4bc` |
| Squeeze Inn TI PA-2 | `69deaa86a7ec8f5cd6df788b` |
| Main Electric Supply (unlinked from Shell PA-1) | `69de86951b837ea6f4440c27` |

---

## Lessons Learned — running list

Track unresolved patterns here. Promote to a numbered rule when seen 3+ times. Items are tagged with their source/observation date.

### From Pay App SKILL v1 (April–May 2026, newest)

- **CO Line Item ↔ G-703 reverse-link recalc:** when CO Line Items are created linking to G-703 rows via the CO LI's increase/deduct fields, the reverse link on the G-703 side syncs bidirectionally only for the first pair in some batches. Manually patching `sbf823a077` / `sdcd13ebea` on the G-703 side does set the link but does **not** trigger the Adjusted Actual Value formula recalc. Recalc requires a UI touch or an event path we haven't isolated. (See Phase 4 §4.4.)
- **Bills & Invoices auto-link to G-702 fires inconsistently** (Phase 1 §1.14 documents the workaround).
- **Property Acquisition needs its own Intacct cost code.** As of May 2026 we use 02100 Clear & Grub as a placeholder with semantic mismatch. Ask accounting to add a dedicated `02100` or `02050 Property Acquisition` code.
- **Asbestos work convention:** lives on 17030 Engineering and Permits per Clint, not on 24000 PreCon. Update prior runs that allocated asbestos to PreCon.
- **County permits convention** (Washoe County Bldg Dept, AQMD, etc.): route to 17030 by default unless a project explicitly carries an `01020 Building Permit` line.
- **Home Depot / Reno Paint Mart on manufactured-home jobs:** route to 32200 Construction Home Setting per Clint, not 27000 Materials (which doesn't exist on those SOVs).
- **Inclusive vs granular line consolidation** (Cooney pattern, Phase 5 §5.4) — not yet a hard rule; keep observing.

### From invoice-import SKILL (April 2026)

- **Subagent partial PA update** — when a subagent creates N of M invoices and updates the PA, then more invoices are created manually, the PA's `s5bea81732` doesn't automatically include the new ones. Always re-update PA with full ID array after every batch (codified in Phase 2 §2I).
- **Retention release automation didn't fire on PA-6** — manual retention release stamping required (codified in Phase 2 §2E).
- **`s5fd21f93c` link silently drops malformed URLs** — codified in Common Errors.
- **`smartsuite_bulk_create_records` 500s on multi-paragraph descriptions** — codified in Common Errors and Phase 2 §2B.

### From budget-import SKILL (April 2026, oldest)

- **Revenue-collecting company was left blank on Shell G-702** — corrected by codifying Phase 1 §1.11. Going forward, populate at create time and verify it propagates to the Pay App.
- **"Create First Pay App" trigger was not invoked on Shell** — the Pay App was a user-provided Excel input, so the trigger step was skipped. On future imports, always use the trigger (Phase 1 §1.12) rather than creating a Pay App record directly. Strengthened §1.12 to call this out explicitly.
- **Stale baselines from prior partial imports are common** — codified in Phase 1 §1.8 and Phase 5 §5.2.
- **Baseline Budget Item title is auto-formed from `s32eed8560` (Account text field) + project name** — codified in Phase 1 §1.8.

### Older / superseded notes

- Historic Phase enum behavior (now textfield, see Phase 1 §1.3).
- Drive parents query API gap — fall back to `search_files` or Chrome browser enumeration.
