---
name: baseline-capture
description: >-
  Conversational intake for capturing baseline budget and timeline data on
  any Stitser BUILT project — construction, flip, development, fractional,
  or TI. Use this skill whenever the user mentions setting up a project
  baseline, capturing budget line items, entering project dates, building
  a baseline budget, "getting a project into S-BOS," "baseline this project,"
  or any variation of "what does the money and timeline look like for this
  deal." Also trigger when the user provides a deal overview (address, price,
  deal type) and expects structured records to be created in SmartSuite —
  even if they don't use the word "baseline." If someone says "new lot,"
  "new project," "set up Cold Creek," "capture the budget for X," or
  "let's baseline," this is the skill.
---

# Project Baseline Capture

A conversational intake workflow that collects baseline budget and timeline
data from a project owner, then writes structured records into S-BOS
(SmartSuite). Built from the Lot 14B case study (13222 Cold Creek Circle)
and designed for reuse across all Stitser BUILT deal types.

## When to Use

Trigger when the user wants to:

- Set up baseline budget items for a new or existing project
- Capture project milestone dates (Estimate / Baseline / Actual)
- Map revenue and cost flows to legal entities
- Build a financial baseline for a construction, flip, fractional, or TI deal
- Create the money-and-timeline skeleton that feeds downstream workflows
  (Pay Apps, GYR reports, investor reporting)

If the user just wants to add a single budget line item or update one date,
skip the full intake — use direct `smartsuite_create_record` instead.

---

## Phase 0 — Ownership Structure Gate

**Ask this first, before anything else.**

> "Is this unit whole ownership or fractional?"

This answer shapes the entire intake. The difference is structural:

| Dimension | Whole Ownership | Fractional |
|---|---|---|
| Sale structure | Single retail buyer at market price | Unit deeded to SPE, then 1/8 interests sold to fractional buyers |
| Revenue tracks | Track 1 (SP commission) + Track 2 (development spread) | Track 1 (SP commission + BUILT revenue) + Track 2 (SPE sale + fractional closings) |
| Entity complexity | PC-1 sells to retail buyer | PC-1 sells to SPE, SPE sells interests to fractional buyers |
| Budget line count | Fewer — no SPE costs, no fractional timeline | More — SPE setup, furnishing, carry costs, fractional absorption |
| Timeline milestones | Construction → List → COE | Construction → SPE Transfer → Staged → Fractional Closings → Sold Out |
| True Up treatment | Standard land cost true-up at closing | True-up reduced by investor note discount (e.g., 15% reduction) |
| Financing | Construction loan, standard payoff at COE | Construction loan + SPE carry financing + fractional buyer closings |

**Why this matters:** Whole-ownership lots (like Lots 1, 4, 6, 8, 9, 14 at
Cold Creek) have a simpler budget — fewer entities, fewer milestones, and
a standard retail sale exit. Fractional lots (like the cul-de-sac units
14B–27B) require SPE setup costs, furnishing budgets, carry costs, and a
phased closing timeline. Asking upfront prevents backtracking through
20 minutes of intake when the fractional flag changes everything.

### Reference Material

The `references/` folder contains two key documents:

- **Decision_Making_Framework_Jan2026.xlsx** — The decision matrix that
  drove the whole-ownership vs. fractional strategy for Cold Creek Commons.
  Tab 3 contains the cost analysis model with per-lot economics. Tab 4
  has the final decision rationale. Use this as context when the user
  references the "discount strategy," "fire sale lots," or "cul-de-sac
  fractional program."

- **deal_type_library.md** — The full Deal Type Library with interview
  questions, milestone templates, budget line item templates, and
  qualification gates for each deal type. Read the relevant deal type
  section before starting Phase 3 (Revenue Streams).

- **screenshots/** — Working spreadsheet views of the Cold Creek lot-level
  economics showing actual price, concessions, commissions, closing costs,
  warranty, true-up, WIP + BTF values, and balance-to-finish by lot.

---

## Phase 1 — Project Identification & Deal Type

**Goal:** Establish the project, confirm it exists in SmartSuite, and
identify the deal type.

Questions (one at a time):

1. "What project are we working on?" — Get project name or address.
   Search SmartSuite Projects table to find the record. If not found, ask
   whether to create one.

2. "What deal type is this?" — Present the options:
   - Refresh & Sell (owner retains, BUILT renovates, SP lists)
   - Fix & Sale / CM (BUILT as CM, owner contracts subs)
   - Flip (platform acquires, renovates, sells)
   - Ground-Up Development (land acquisition through vertical construction)
   - Manufactured Home Subdivision
   - Midsized Multifamily Value-Add
   - Commercial TI (Hardbid or CM/GMP)

3. "What's the current pipeline stage?" — Where is this project right now?
   Pre-development, under construction, in closeout, listed for sale, etc.

**After Phase 1**, read `references/deal_type_library.md` — specifically the
section for the identified deal type. The deal type library contains the
interview questions, milestone templates, and qualification gates that shape
Phases 2–6.

---

## Phase 2 — Timeline Anchor Points

**Goal:** Build a skeleton of key milestone dates, each tagged as Actual,
Baseline, or Estimate.

The three data states:

- **Actual** — Already happened. Use `s7c51ac6b5` (Actual Date field).
- **Baseline** — Committed, verified, high-probability. Use `s8ca756976`.
- **Estimate** — Preliminary, unverified. Use `s147d5462c`.

Milestones vary by deal type. For a fractional residential deal, typical
milestones include:

| Milestone | Event ID | Stage |
|---|---|---|
| Construction Start | `PPaox` | 3-WIP (`77s21`) |
| Construction End | `exoZI` | 3-WIP |
| Closeout & Punch List | `kc4xy` | 4-Closeout (`CmpEp`) |
| Purchase Close Date (SPE transfer) | `f8xrD` | 3-WIP |
| COE/Lease Commence (fractional closings) | `K302b` | 4-Closeout |
| List Date | `48dr3` | varies |

For whole-ownership lots, the milestones are simpler — no SPE transfer,
no fractional closings. Capture only:

| Milestone | Event ID | Stage |
|---|---|---|
| Closeout & Punch List (NOC) | `kc4xy` | 4-Closeout (`CmpEp`) |
| COE/Sale Close | `K302b` | 4-Closeout (`CmpEp`) |

**Do NOT capture Construction Start or Construction End for whole-ownership
Cold Creek lots** — these are completed units. The operationally meaningful
dates are the NOC (Closeout) and the COE.

**COE confidence:** If the close is pending California DRE clearance, use
the spreadsheet's target close month as the Baseline date and leave the
Actual field empty until close occurs.

**Ask:**
- "When was the NOC filed?" → record as Actual Closeout & Punch List
- "What's the baseline close date? Is there anything blocking it (DRE, financing)?"
  → record as Baseline COE/Lease Commence

**Rule:** If a milestone has already happened, record it as Actual. If it's
committed, record as Baseline. If it's tentative, record as Estimate.
One date field per record — don't fill multiple confidence columns on the
same record.

---

## Phase 3 — Revenue Streams (Deal Type Conditional)

**Goal:** Identify every inflow and outflow, who receives/pays it, and the
confidence level.

Questions are conditional on deal type (Phase 1) AND ownership structure
(Phase 0). Read the relevant deal type section from
`references/deal_type_library.md` for the full question set.

### Fractional Residential (Cold Creek cul-de-sac pattern)

Revenue inflows to map:

| Line Item | Entity | Track | Cash Flow Section |
|---|---|---|---|
| SP Listing Commission (2%) | Stitser Properties | Track 1 | Operating |
| BUILT Construction Revenue | BUILT | Track 1 | Operating |
| Sale Price to SPE | PC-1 | Track 2 | Investing |

Cost outflows to map:

| Line Item | Entity | Track | Cash Flow Section |
|---|---|---|---|
| Buyer Agent Co-op (2.25%) | PC-1 | Track 2 | Operating |
| True Up to Land Seller | PC-1 | Track 2 | Operating |
| BUILT Construction Cost (BTF) | PC-1 | Track 2 | Operating |
| Closing Costs | PC-1 | Track 2 | Operating |
| Warranty Reserve (1%) | PC-1 | Track 2 | Operating |
| SPE Total Cost (setup + furnish + carry) | PC-1 | Track 2 | Operating |

**Key fractional-specific questions:**

- "What is the retail sale price for this unit?" (drives commission calcs)
- "What is the true-up amount to the land seller? Is there an investor
  note discount?" (Cold Creek: 15% reduction from $110,847.85 to $94,220.67)
- "What is the BUILT construction budget (build-to-finish amount)?"
  - The BUILT margin is INTERNAL to this amount — not on top of it.
    If BTF is $138,327 and margin is 9%, BUILT's challenge is to deliver
    under $125,878 in actual cost. Revenue to BUILT = BTF amount.
    Cost to PC-1 = same BTF amount. The margin is BUILT's to protect.
- "What is the estimated SPE total cost (setup, furnishing, carry)?"
  If the user doesn't have a detailed SPE budget yet, capture as a single
  Estimate line item.

### Whole Ownership (Cold Creek discount lots pattern)

Simpler structure — no SPE, no fractional timeline:

| Line Item | Entity | Track | Cash Flow Section |
|---|---|---|---|
| SP Listing Commission | Stitser Properties | Track 1 | Operating |
| BUILT Construction Revenue | BUILT | Track 1 | Operating |
| Sale Price (retail buyer) | PC-1 | Track 2 | Investing |
| Buyer Agent Co-op | PC-1 | Track 2 | Operating |
| True Up to Land Seller | PC-1 | Track 2 | Operating |
| BUILT Construction Cost (BTF) | PC-1 | Track 2 | Operating |
| Closing Costs | PC-1 | Track 2 | Operating |
| Warranty Reserve | PC-1 | Track 2 | Operating |

### Commission Structure

SP earns a listing commission — this is Track 1 operating revenue that SP
can project and rely on. The rate and structure depend on whether there is
a buyer's agent:

**With buyer's agent (standard split):**
- SP Listing Commission: 2% → Track 1, Operating (SP entity)
- Buyer Agent Co-op: 2.25% → Track 2, Operating cost (PC-1 entity)
- Total commission drag on PC-1: 4.25%

**No buyer's agent (SP represents both sides):**
- SP collects the full commission — typically 4.25% of sale price
- Record as a single Track 1 Operating line: "SP Listing Commission 4.25%"
- There is NO buyer agent co-op cost line for PC-1 in this scenario

**Key question to ask:** "Is there a buyer's agent, or is SP representing
both sides?" This determines the commission structure and whether to
record a co-op cost.

Do NOT record the buyer agent co-op as SP revenue in any scenario — it is
either an outflow for PC-1 (with buyer agent) or it doesn't exist (no
buyer agent).

---

## Phase 4 — Entity Revenue Mapping

**Goal:** Confirm which legal entity captures each revenue stream and which
entity pays each cost. Do NOT assume — ask the user.

Key entities in the Stitser BUILT ecosystem:

| Entity | Role | Intacct Location ID |
|---|---|---|
| PC-1 Developers | Development entity, holds assets | `6914fe61e127b5f69fb770e1` |
| Stitser Properties (SP) | Brokerage, earns commissions | `6914fe61e127b5f69fb770df` |
| BUILT | GC, earns construction revenue | `6914fe61e127b5f69fb770da` |
| Formation Homes | MH dealer, asset management | look up if needed |

The `SB Company Receiving/Paying` field on Baseline Budget Items
(`s2f27d033f`) links to the Intacct Locations table
(`6914fb94e53085946b899cb0`). Use the Intacct Location record IDs above.

**Questions:**
- "Which entity receives this revenue / pays this cost?"
- "Is there a new project-specific entity (e.g., an SPE) that needs to
  be created?"

If a new entity is needed (like Cold Stream Properties LLC for the true-up
payee), create records in both:
1. Companies/Entities table (`68216a706900e8eaf75a05c0`)
2. Intacct Vendor ID table (if they'll receive payments via Intacct)

Then draft an email to the accounting team (Lisa Coleman, lisa@built-nv.com)
requesting the vendor be added in Sage Intacct.

---

## Phase 5 — Estimate vs. Baseline Clarification

**Goal:** Walk through each captured data point and confirm the confidence
level.

For each budget item and date, ask:
- "Is this amount/date an Estimate or a Baseline?"
- If Estimate: "What would need to happen to convert this to Baseline?"

Record the answer. Items tagged as Estimate get a flag in the Confidence
Report (Phase 7 output) with a note on the conversion criteria.

**Rule:** Actual amounts/dates are only for things that have already
occurred. Don't let the user tag a future date as Actual.

---

## Phase 6 — Financing & Capital Structure

**Goal:** Capture the capital stack — debt, equity, preferred return
obligations, and cost of financing.

**Questions:**
- "How is this project being financed — construction loan, partner
  lending, platform equity, investor capital?"
- "What is the loan amount, interest rate, and maturity date?"
- "Is there a preferred return obligation to investors?"
- "What is the monthly carry cost (interest + taxes + insurance +
  utilities)?"

For the Cold Creek lots, the financing is via Partner Lending (e.g.,
Lot 14B: $426,474 at 7%, maturing March 2028). This data lives in the
Loans table in SmartSuite — confirm whether a loan record already exists
before creating one.

---

## Phase 7 — Completeness Check & Summary

**Goal:** Present everything captured in a structured summary table and
get user confirmation before writing any records.

### Summary Format

Present two tables:

**Baseline Budget Items:**

| # | Line Item | Amount | Confidence | Track | Cash Flow | Entity |
|---|---|---|---|---|---|---|
| 1 | SP Listing Commission 2% | $22,585 | Baseline | Track 1 | Operating | SP |
| 2 | Buyer Agent Co-op 2.25% | $25,408 | Baseline | Track 2 | Operating | PC-1 |
| ... | ... | ... | ... | ... | ... | ... |

**Project Dates:**

| # | Milestone | Date | Confidence | Event Type | Stage |
|---|---|---|---|---|---|
| 1 | Construction Start | 2024-06-15 | Actual | Construction Start | 3-WIP |
| 2 | Construction End | 2026-10-30 | Baseline | Construction End | 3-WIP |
| ... | ... | ... | ... | ... | ... |

**Ask:** "Does this look right? Any corrections before I create the
records?"

Wait for explicit confirmation. If corrections are needed, adjust and
re-present.

---

## Phase 8 — Record Creation

Once the summary is confirmed, create records in SmartSuite.

### Baseline Budget Items

**App ID:** `69bb89ebf6a195c2c73a3b3e`

Always call `smartsuite_get_app_schema` first to confirm field slugs.

| Field | Slug | Type | Notes |
|---|---|---|---|
| Account | `s32eed8560` | text | Used in auto-title |
| Estimated Budget | `sc507e6b54` | currency | Use for Estimate confidence |
| Baseline Budget | `s818f40f1d` | currency | Use for Baseline confidence |
| Actual Amount | `scfca058ab` | currency | Use for Actual confidence |
| Project | `s2ba7b261b` | linked record | Link to Projects table |
| Cash Flow Section | `s8ee35f579` | single-select | Operating=`x0IWR`, Investing=`ozQle`, Financing=`wExoS` |
| Revenue Track | `sb54d9092a` | single-select | Track 1=`pO6Hh`, Track 2=`UhSZv` |
| SB Company | `s2f27d033f` | linked record | → Intacct Locations table |

**Record creation pattern:**

```json
{
  "app_id": "69bb89ebf6a195c2c73a3b3e",
  "fields": {
    "s32eed8560": "SP Listing Commission 2%",
    "s818f40f1d": 22585,
    "s2ba7b261b": ["PROJECT_RECORD_ID"],
    "s8ee35f579": {"value": "x0IWR"},
    "sb54d9092a": {"value": "pO6Hh"},
    "s2f27d033f": ["6914fe61e127b5f69fb770df"]
  }
}
```

**Confidence mapping:** Put the dollar amount in the field that matches
the confidence level — `sc507e6b54` for Estimate, `s818f40f1d` for
Baseline, `scfca058ab` for Actual. Only fill ONE of these three per record.

### Project Dates

**App ID:** `69bb7d64740e0e696d88c47f`

| Field | Slug | Type | Notes |
|---|---|---|---|
| Project | `sed6d961dc` | linked record (single) | |
| Estimated Date | `s147d5462c` | datefield | Plain string: `"2026-06-15T00:00:00Z"` |
| Baseline Date | `s8ca756976` | datefield | Plain string format |
| Actual Date | `s7c51ac6b5` | datefield | Plain string format |
| Event | `sc632a4d66` | single-select | See event ID table below |
| Stage | `s44db639c4` | single-select | See stage ID table below |

**Event IDs:**

| Event | ID |
|---|---|
| Construction Start | `PPaox` |
| Construction End | `exoZI` |
| Closeout & Punch List | `kc4xy` |
| COE/Lease Commence | `K302b` |
| Purchase Close Date | `f8xrD` |
| List Date | `48dr3` |

**Stage IDs:**

| Stage | ID |
|---|---|
| 3-WIP | `77s21` |
| 4-Closeout & Warranty | `CmpEp` |

**Record creation pattern:**

```json
{
  "app_id": "69bb7d64740e0e696d88c47f",
  "fields": {
    "sed6d961dc": ["PROJECT_RECORD_ID"],
    "s7c51ac6b5": "2024-06-15T00:00:00Z",
    "sc632a4d66": {"value": "PPaox"},
    "s44db639c4": {"value": "77s21"}
  }
}
```

**Date field format:** Use plain ISO strings like `"2026-10-30T00:00:00Z"`.
Do NOT use object format like `{"date": "..."}` — that throws a conversion
error. Fill only the date field matching the confidence level (Estimated,
Baseline, or Actual).

---

## SmartSuite API Gotchas

These were all discovered the hard way during the Lot 14B case study.
Following these rules will save significant debugging time.

### 1. Bulk Create Is Broken — Use Individual Creates

`smartsuite_bulk_create_records` returns HTTP 500 errors even with small
batches. Always use individual `smartsuite_create_record` calls. For 9
budget items and 7 dates, creating them one at a time is reliable and
takes about 2 minutes.

### 2. Single-Select Fields: Create vs. Update Behave Differently

When **creating** a record, single-select fields silently ignore
`{"value": "option_id"}` format — the field saves empty. Use a plain
string instead: `"option_id"` (no object wrapper).

When **updating** a record, the plain string format also works.

Pattern that works in BOTH create and update:
```
"sc632a4d66": "kc4xy"    ✅ plain string
"sc632a4d66": {"value": "kc4xy"}    ❌ silently fails on create
```

Always patch single-select fields via a follow-up `smartsuite_update_record`
call immediately after create if you used object format in the create call.

### 3. User Fields Don't Accept Member UUIDs via API

The `Assigned To` userfield type doesn't accept member UUID arrays through
the API. Workaround: use a `Link to People` field instead, passing the
People table record ID. Note that this won't trigger SmartSuite
notifications — manual assignment may be needed for that.

### 4. Date Fields Take Plain Strings

Date fields (`datefield`, `duedatefield`) accept plain ISO strings:
`"2026-06-15T00:00:00Z"`. Object format `{"date": "..."}` throws
`"can't be converted to date"` errors.

### 5. Freeform Single-Select Values Don't Save via API

Even when a single-select field has `new_choices_allowed=true`, passing a
new label string via the API doesn't create the option — the field comes
back empty. Only pass existing option IDs. If you need a new event type
(like "Interior Restart"), note it for manual entry in SmartSuite.

### 6. Always Verify After Create

Read back records after creation to confirm fields were saved correctly.
Some fields (especially text fields set during create) occasionally
return empty despite appearing to accept the value.

### 7. Schema Before Write

Always call `smartsuite_get_app_schema` before writing to any app. Field
slugs and option IDs can change. The slugs in this document are current
as of April 2026 but should be verified.

---

## Worked Example — Lot 14B (Fractional)

**Project:** 13222 Cold Creek Circle (Lot 14B)
**Deal Type:** Fractional residential (cul-de-sac unit)
**Ownership:** Fractional (unit deeded to SPE, 1/8 interests sold)
**Project ID:** `69b24d9e3e15e402a66081db`

### Budget Items Created (9 total)

| Line Item | Amount | Confidence | Track | Cash Flow | Entity |
|---|---|---|---|---|---|
| SP Listing Commission 2% | $22,585 | Baseline | Track 1 | Operating | SP |
| Buyer Agent Co-op 2.25% | $25,408 | Baseline | Track 2 | Operating | PC-1 |
| BUILT Construction Revenue | $138,327 | Baseline | Track 1 | Operating | BUILT |
| Sale Price to SPE | $1,129,239 | Baseline | Track 2 | Investing | PC-1 |
| True Up to Coldstream | $94,221 | Baseline | Track 2 | Operating | PC-1 |
| BUILT Construction Cost BTF | $138,327 | Baseline | Track 2 | Operating | PC-1 |
| Closing Costs | $5,000 | Baseline | Track 2 | Operating | PC-1 |
| Warranty Reserve 1% | $11,292 | Baseline | Track 2 | Operating | PC-1 |
| SPE Total Cost | $50,000 | **Estimate** | Track 2 | Operating | PC-1 |

### Project Dates Created (7 total)

| Milestone | Date | Confidence | Event |
|---|---|---|---|
| Construction Start | 2024-06-15 | Actual | Construction Start |
| Interior Restart | 2026-06-20 | Baseline | (manual — no existing event ID) |
| Construction End | 2026-10-30 | Baseline | Construction End |
| SPE Transfer | 2026-11-15 | Estimate | Purchase Close Date |
| First Fractional Closing | 2026-12-01 | Baseline | COE/Lease Commence |
| Sold Out | 2026-12-31 | Baseline | COE/Lease Commence |
| Closeout & Punch List | 2026-10-01 | Estimate | Closeout & Punch List |

### Key Decisions Made During Intake

1. **BUILT margin is internal:** $138,327 BTF is both BUILT revenue AND
   PC-1 cost. The 9% margin ($12,449) is BUILT's to protect by delivering
   under $125,878 in actual cost.

2. **Buyer agent co-op is PC-1 cost, not SP revenue:** The 2.25% goes to
   a third-party buyer's agent. SP only books its 2% listing commission
   as projectable Track 1 revenue.

3. **True-up reduced by investor note:** Original true-up was $110,847.85;
   reduced 15% to $94,220.67 per the investor note structure.

4. **SPE cost captured as Estimate:** The detailed SPE budget (setup,
   furnishing, carry) wasn't ready yet, so a $50,000 lump sum was
   captured as Estimate — to be broken out in a subsequent baseline
   session.

---


## Worked Example — Lot 6 (Whole Ownership)

**Project:** 13005 Winter Camp Way (Lot 6)
**Deal Type:** Ground-Up Development, whole ownership
**Ownership:** Whole ownership (retail buyer, no SPE)
**Project ID:** `69b24d9e3e15e402a66081ae`
**Buyers:** Harn-Cherng Shiue & Erin Shiue

### Budget Items Created (3 total)

| Line Item | Amount | Confidence | Track | Cash Flow | Entity |
|---|---|---|---|---|---|
| Sale Revenue | $808,000 | Baseline | Track 2 | Investing | PC-1 |
| PC-1 Gross Profit | $706,489 | Baseline | Track 2 | Operating | PC-1 |
| SP Listing Commission 4.25% | $34,340 | Baseline | Track 1 | Operating | SP |

### Project Dates Created (2 total)

| Milestone | Date | Confidence | Event |
|---|---|---|---|
| Closeout & Punch List (NOC) | 2025-04-14 | Actual | Closeout & Punch List |
| COE / Sale Close | 2026-02-28 | Baseline | COE/Lease Commence |

### Key Decisions Made During Intake

1. **No construction dates captured:** Lot 6 is a completed unit. Construction
   history is irrelevant to current decisions. Only NOC and COE matter.

2. **No buyer's agent:** SP represented both sides at 4.25% — single Track 1
   commission line. No Buyer Agent Co-op cost line for PC-1.

3. **No BUILT construction revenue/cost lines:** For whole-ownership lots,
   GP is PC-1's spread (Rev Before COGS = sale price minus commissions,
   closing costs, warranty, and true-up). BUILT construction lines are not
   captured at the lot level for whole-ownership.

4. **COE is Baseline, not Actual:** Lot 6 close is pending California DRE
   clearance. The February 2026 target from the spreadsheet was used as
   Baseline. Actual to be updated when close occurs.

5. **Flat Projects table fields zeroed out:** After creating helper table
   records, `sb903e68c2` (Baseline Revenue) and `svs6xrqj` (Baseline GP)
   on the Projects record were nulled. Helper tables are the sole source
   of truth.

### Stakeholder Bridge Records Created (6 total)

| Person | Role | Company |
|---|---|---|
| Erin Shiue | Buyer/Tenant | — |
| Harn-Cherng Shiue | Buyer/Tenant | — |
| Gino Perano | Owner Broker | — |
| Zach Roullier | Superintendent | — |
| Clint Stitser | Owner | PC-1 Developers LLC |
| Clint Stitser | Debt | Heritage Bank |

---

## Behavioral Rules

These rules govern how the agent conducts the intake conversation:

1. **One question at a time.** Never dump a list of questions. Use the
   prior answer to shape the next question.

2. **Three data states, always.** Every dollar amount and every date gets
   tagged as Estimate, Baseline, or Actual. If the user doesn't specify,
   ask.

3. **Timeline and money are always linked.** If you have a date but no
   associated dollar amount, drill down on the money. If you have an
   amount but no timing, drill down on the date.

4. **Deal type shapes questions.** Don't ask about SPE setup costs on a
   Refresh & Sell. Don't ask about listing commissions on a TI job where
   there's no sale. Use the Deal Type Library.

5. **Don't preload entity options.** Ask "who receives this revenue?" and
   then look it up — don't present a dropdown of all entities.

6. **Summarize before writing.** Present the full summary table (Phase 7)
   and wait for explicit confirmation before creating any records.

7. **Create records individually.** Never use bulk create. Create each
   record with `smartsuite_create_record` and verify the response.

8. **Flag gaps for follow-up.** If something is tagged Estimate, note
   what would convert it to Baseline. If a field can't be set via API
   (like a new Event label), note it for manual entry.

---

## Outputs Checklist

Before declaring the baseline capture complete, confirm:

- [ ] All budget items created and verified in SmartSuite
- [ ] All project dates created with correct Event IDs
- [ ] Entity mapping confirmed for every line item
- [ ] Estimate items flagged with conversion criteria
- [ ] Any new entities created (Companies + Intacct Vendor ID)
- [ ] Accounting team notified of new vendors (email drafted)
- [ ] Any API limitations noted for manual follow-up
- [ ] User has confirmed the summary matches their expectations
- [ ] Stakeholder Bridge records created for all known parties (buyers,
  seller, listing agent, super, lender)
- [ ] All Stakeholder Bridge records have Stage set to "Active Project
  Participant" (`1yWEb`) — patch via update call after create if needed
- [ ] Flat date/dollar fields on Projects record zeroed out (helper tables
  are now the source of truth — `sb903e68c2` and `svs6xrqj` should be null)
- [ ] People table checked for duplicates before creating any new person
