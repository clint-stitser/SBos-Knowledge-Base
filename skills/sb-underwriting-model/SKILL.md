---
name: sb-underwriting-model
description: "Full lifecycle underwriting skill for Stitser BUILT deal analysis. Triggers when a user uploads or references a Harmony Mesa / Formation Homes underwriting workbook, mentions \"underwriting model\", \"pro forma\", \"release P&L\", \"capital stack\", \"NAHA\", \"Spread TX\", or asks to build, populate, or analyze a deal model. Covers the complete 9-step workflow from project identification through report generation, with structured dialogue at each stage. Also triggers on phrases like \"let's underwrite a deal\", \"help me model this project\", \"populate the model\", \"build the capital stack\", \"generate the reports\", or \"make sense of the Spread TX\"."
---

# SB Underwriting Model — Skill

## Purpose
Guide Claude through the full lifecycle of a Stitser BUILT deal underwriting session — from reading a Google Sheets workbook through capital stack structuring, FinancingFlow script deployment, and report package generation. Ensures accuracy, consistency, and trust at each step.

---

## Entry Points

**A — New deal, blank model:** Steps 1–9 in sequence.
**B — Existing model, Spread TX not yet run:** Read Items & Amounts, then Steps 5–9.
**C — Spread TX already run (most common re-entry):** Read Spread TX → print summary → check financing → offer options.
**D — Reports only:** Read Spread TX, build package per report spec below.
**E — Update specific sheet:** Identify sheet, read current state, make targeted edit.
**F — Script debugging:** Read all relevant .gs files before touching any code.
**G — Capital stack restructure:** Simulate new stack numerically first, then map to Financing Options inputs.

---

## Step 1 — Deal Identification

Read the model and identify:
- Project name, location, seller, deal type
- Unit count and mix (Type A / B / etc.)
- ARV per type (flag if only 1 comp — second comp needed)
- Phase structure from PhasesOptions tab

**Always flag:** ARV with only 1 comp is an open item. Never treat as confirmed.

---

## Step 2 — Spread TX Read

Read `Spread TX by Period`. Parse every row:
- Col 0: Date, Col 1: Phase Option, Col 2: Description, Col 4: Cash Flow Section, Col 7: Amount, Col 8: Month (datetime)

Strip Month column safely — it is always a datetime object. Use `month_val.year` and `month_val.month`.

Categorize rows:
- `draw -` + NAHA in phase → `naha_draw`
- `draw -` → `eq_draw`
- `loan repayment` + NAHA → `naha_repay`
- `loan repayment` → other repayment
- `equity contribution` → `eq_contrib` (PartnerLogic)
- `equity distribution` → `eq_distrib` (PartnerLogic)
- Section `1-Operating` → revenue
- Section `2-Operating` → cogs
- Section `3-Operating` → overhead

**Print 5-section summary:** Revenue, COGS, OH, Financing Debt (Sec 5), Financing Equity (Sec 6), Net.

Net should equal the operating reserve amount (typically $150K) or $0 if no reserve configured.

---

## Step 3 — Capital Stack Status

Check Financing Options tab for active sources. Note:
- Source names, amounts, repayment modes, funding order, applied phases
- Whether `Repayment Mode` column exists (column 8 in current template)

Flag if PartnerLogic rows (`equity contribution`, `equity distribution`) are in the Spread TX — they are from `calculatePartnerWaterfall()` and are separate from FinancingFlow draws. They must NOT be stripped from reporting but must be understood separately.

---

## Step 4 — FinancingFlow Script Architecture

**Script execution order in `runFinancingAndCashFlow()`:**
1. `updateFinancingQC()` — syncs active phases to Financing Options
2. `generateFinancingModel()` — draws and repayments (v8+)
3. `generateProgrammaticCashFlow()` / `generateCashFlowStatement()`
4. `calculatePartnerWaterfall()` — equity calls and distributions

**v8 FinancingFlow rules (critical):**
- Repayment Mode column: `Pro-Rata`, `Full at Sellout`, or `Term`
- Pro-Rata: fires at every close, NO phase match required — `repayAmt = limit × (units_this_batch / total_units)`
- Full at Sellout: fires only on last sale month, requires phase match
- Applied Phases controls DRAWS only, not repayment timing for Pro-Rata sources
- Guard: equity cannot fund debt repayments (`isDebtRepay` guard)
- Guard: debt cannot fund equity repayments (`isEqRepay` guard)
- Cleanup strips: `draw - `, `loan repayment - `, `interest payment - `, `naha repayment`, `land owner repayment`, `sb equity return`, `stitser built equity return`, `sponsor equity return`, `equity distribution`, `equity contribution`, `required equity`
- `readBatchUnits_()` has hardcoded fallback: `{release aug-26: 3, ..., release feb-27: 1}` — ensures pro-rata math works even if Variable 1 column not found

**Common failure modes:**
- PartnerLogic equity contribution rows not stripped → they inflate NOI → NAHA under-draws → surplus NAHA draws at sellout
- Pro-Rata repayment phase-matched (old behavior) → only fires when sale phase matches funding phase → NAHA never repays if funded infra but selling releases
- Equity source drawing to fund debt repayments → SB equity balance inflated → wrong sellout return

---

## Step 5 — Capital Stack Dialogue

Before writing any Financing Options inputs, ask:

**Q1 — Equity structure:**
- Is land seller staying in as LP or straight purchase?
- If straight purchase: land cost moves to Items & Amounts as COGS (Section 2), funded from equity line
- If LP: separate equity row, Pro-Rata or Full at Sellout?

**Q2 — Equity amount and draw rule:**
- Single equity source or multiple?
- Up-front lump or draw-as-needed?
- Operating reserve floor? (e.g., draw equity to maintain $150K)

**Q3 — Debt:**
- NAHA limit and rate
- Applied phases (all phases safe in v8 — Pro-Rata repayment ignores phase match)
- Funding order: equity first (order 1) or NAHA first?

**Q4 — Repayment:**
- NAHA: Pro-Rata (standard)
- Equity: Full at Sellout (standard for clean stack)
- Any term-based loans?

**Output:** Financing Options tab inputs, exact row by row:

| Source Name | Select | Type | Funding Order | Applied Phases | Repayment Mode | Cap Type | Amount |
|---|---|---|---|---|---|---|---|
| SB Sponsor Equity | TRUE | Equity | 1 | All phases | Full at Sellout | Fixed Amount | $X |
| NAHA — NV Revolving | TRUE | Debt | 2 | All phases | Pro-Rata | Fixed Amount | $2,500,000 |

---

## Step 6 — Operating Reserve

Two separate controls — both needed for a true $150K floor:

**FinancingFlow `RESERVE` variable (controls draws):**
- In `generateFinancingModel()`: `var RESERVE = 150000`
- Draw equity/NAHA whenever account would drop below this floor
- Controls INFLOWS — when capital enters the project

**Equity Options tab (controls distributions):**
- Label `Operating Reserve Value`: 150000 → carves out $150K at first close
- Label `Minimum Working Capital`: 150000 → holds $150K back from every distribution month
- Label `Ignore Retention on Sale?`: No → keeps reserve intact through individual closes

**Triggered by:** `calculatePartnerWaterfall()` — runs as part of `runFinancingAndCashFlow()`

These systems are complementary:
- FinancingFlow: protects floor from COST pressure (draws capital when needed)
- PartnerLogic: protects floor from DISTRIBUTION pressure (holds back before paying partners)

---

## Step 7 — Simulation Before Deployment

Before changing any Financing Options inputs, simulate the new stack in Python:
1. Read operating-only monthly cash flows from Spread TX (strip all script and PartnerLogic rows)
2. Walk the timeline month by month applying the proposed draw rules
3. Print: monthly draws, running balance, NAHA balance, equity drawn, repayments
4. Confirm: no shortfalls, NAHA fully repaid, equity returns correctly at sellout

**Key simulation check:** Final ending cash = operating reserve amount (or $0 if no reserve). If it doesn't, the stack has a leak.

---

## Step 8 — Report Package Generation

### Pre-generation checklist
Before writing any code:
1. Read the Spread TX and categorize every row
2. Verify section totals sum to expected net
3. Derive all monthly values from actual data — do not estimate or hardcode without verification
4. Run assertions: `sum(rev) == expected_rev`, `sum(cogs) == expected_cogs`, `final_end_cash == reserve_amount`

### Report package structure (5 sheets)

**Sheet 1 — Executive Summary**
- Deal snapshot panel (left): project name, location, deal type, land structure, units, ARVs, timeline
- P&L summary panel (left): Revenue, COGS, Gross Profit, OH, Operating Profit — with Amount and % Revenue columns
- Capital stack panel (right): source names, committed amounts, repayment mode
- Return metrics panel (right): gross margin, op profit, equity contributions, distributions, IRRs, reserve
- Open items panel (right): flagged items in amber (missing comps, unconfirmed costs, pending quotes)
- **Do not mix IS detail with deal metrics — these are separate panels**

**Sheet 2 — Income Statement**
- Full project P&L, common-sized to revenue
- TOTAL column with formulas (never hardcode totals)
- % Revenue column: `=-C{row}/C{REV_ROW}` for cost rows, `=C{row}/C{REV_ROW}` for revenue rows
- $/Unit column: `=C{row}/19` (or total units)
- Land acquisition appears as first COGS line if direct purchase structure
- Below-the-line financing reference section (amber background, italic) — not deducted from operating profit
- **Formula REV_ROW reference must be set after the row is written**

**Sheet 3 — Release P&L**
- One column per release batch (label, units, close date)
- PROJECT TOTAL column with `=SUM(C{r}:I{r})` formulas
- Revenue rows by type (Type A, Type B)
- COGS rows: vertical COGS + infra/land/pre-dev amortized per unit
- Infra amortization = (total COGS − release-phase COGS) / total units
- GROSS PROFIT row with `=SUM(C{r}:I{r})` and green background
- Gross Margin % row below
- NAHA repayment schedule at close (amber section, below GP line)
- Equity distribution schedule at close

**Sheet 4 — Monthly Cash Flow**
Structure (rows in order):
1. Column headers (period labels)
2. Blank row
3. OPERATING CASH FLOWS section header
4. Revenue — Unit Sales (per period)
5. COGS — All Project Costs (per period)
6. Overhead (per period)
7. [Blank / rule]
8. **NET OPERATING CASH FLOW** — explicit values + TOTAL = `=SUM(detail rows in TOTAL col only)`
9. Blank row
10. FINANCING CASH FLOWS section header
11. Equity Contributions — LP+GP (+)
12. NAHA Draws (+)
13. NAHA Repayments Pro-Rata (−)
14. Partner Equity Distributions (−)
15. [Blank / rule]
16. **NET FINANCING CASH FLOW** — explicit values + TOTAL = `=SUM(detail rows in TOTAL col only)`
17. Blank row
18. **NET CASH FLOW (Period)** — explicit values for every period including zeros. TOTAL = `=SUM(C{r}:last_period_col{r})`
19. Blank row
20. CASH BALANCE ROLL-FORWARD section header
21. **Beginning Cash Balance** — explicit values, no formula. TOTAL col = `—`
22. **Ending Cash Balance** — formula per period: `={col}{BEG_ROW}+{col}{NET_ROW}`. TOTAL col = last period's ending balance
23. Reserve floor reference row (light gray, 8pt italic)
24. Note row (merged, explaining roll-forward logic)

**Critical cash flow rules:**
- NET OPERATING and NET FINANCING TOTAL columns: sum the detail rows, NOT the period totals (avoids double-counting)
- NET CASH FLOW TOTAL: `=SUM` across all period columns in that row
- Ending Balance TOTAL: not a sum — it's the final period's ending balance
- Beginning Balance TOTAL: `—` (not meaningful to sum)
- Every period must have an explicit value in NET CASH FLOW row (including zeros) — never leave blank
- Derive net_v from: `net_exact[mo]` from actual Spread TX monthly totals, not from op_net + fin_net (rounding accumulates)
- Verify: `sum(net_v) == operating_reserve_amount` before writing
- Verify: `end_v[-1] == operating_reserve_amount` before writing

**Sheet 5 — Capital Stack & Stats**
- Capital stack table: source, committed, actually drawn, repayment mode
- TOTAL row with formulas
- Leverage & returns metrics table
- NAHA draw/repay schedule (period by period, running balance)
- ARV sensitivity (base, −5%, −10%, +5%)

### Post-generation review (mandatory before delivery)
Run these checks every time:
```python
assert no formula errors (#REF!, #DIV/0!, etc.)
assert IS Revenue == expected_revenue
assert IS COGS == expected_cogs
assert CF op_net sum == expected_op_profit
assert CF net_v sum == operating_reserve_amount
assert CF end_v[-1] == operating_reserve_amount
assert Release P&L has 7 columns (one per release)
assert Release GP sum ~= expected_gross_profit (within $1,000)
assert Exec Summary op profit == expected_op_profit
assert Capital Stack has all 3 sources (Equity, NAHA, Reserve)
```
If any check fails, fix before presenting. Never deliver a report that hasn't passed all checks.

---

## Step 9 — Deal-Specific Reference (Harmony Mesa)

| Item | Value |
|---|---|
| Project | Harmony Mesa (Wilson Landing), Sun Valley NV |
| Deal type | Ground-Up For-Sale CrossMod/MH SFR |
| Land | Direct purchase from Hero Land Holdings |
| Land cost | $400,000 (COGS, Jun 2026) |
| Units | 19 total: 10 Type A + 9 Type B |
| Type A ARV | $379,900 (1 comp only — needs second) |
| Type B ARV | $424,900 |
| Gross Revenue | $7,623,100 |
| Total COGS | $5,938,493 (incl. $400K land) |
| Operating Profit | $1,634,607 (21.4% margin) |
| Infra/Land amortized | $2,278,400 ÷ 19 = $119,916/unit |
| Batch schedule | 3-3-3-3-3-3-1 (Aug-26 through Feb-27) |
| Capital stack | SB Equity $1.2M (order 1, Full at Sellout) + NAHA $2.5M limit (order 2, Pro-Rata) |
| NAHA peak | $2,053,358 in Oct 2026 |
| NAHA total drawn | $2,220,746 |
| NAHA repaid | 5 closes: Nov-26, Dec-26, Jan-27, Feb-27, Mar-27 |
| SB equity contributions | $1,562,644 total (LP 95% + GP 5% via PartnerLogic) |
| Operating reserve | $150,000 (Equity Options tab) |
| First close | Nov 30, 2026 |
| Final close | Jul 10, 2027 |
| FinancingFlow version | v8 (isDebtRepay guard, hardcoded batch fallback) |

**Open items:**
- ARV second comp needed
- Off-site civil (Harmony Lane) ~$50K — confirm by 5/15
- NV Energy facilities quote ~$85K — confirm by 5/15
- Land purchase PSA — confirm Jun 2026 close date

---

## Common Mistakes to Avoid

1. **Stripping PartnerLogic rows before building reports** — they're needed for the financing section of the cash flow. Strip only for IS/P&L operating calculations.

2. **Hardcoding total row values** — always use `=SUM(...)` or `=C{row1}+C{row2}` formulas in Excel. The only exception is when values are sourced directly from verified Spread TX data.

3. **Summing the TOTAL column for Beg/End cash balance** — Beginning balance total is meaningless (don't sum). Ending balance total = last period's ending balance (not a sum).

4. **Double-counting in subtotal TOTAL column** — when a subtotal row's TOTAL is `=SUM` of period columns, AND those period columns were themselves sums of detail rows — the detail rows' totals must use `=SUM(detail rows in TOTAL col)`, not `=SUM(period columns in TOTAL row)`.

5. **Deriving net_v from op_net + fin_net** — small rounding errors accumulate and the final cash doesn't match. Use `net_exact` derived directly from Spread TX monthly totals.

6. **Missing Release P&L sheet** — always include it. For-sale deals always need a per-batch P&L showing gross profit by release.

7. **Mixing IS detail with executive summary** — these are separate sheets/panels. The executive summary is a one-page snapshot; the income statement is the detailed waterfall.

8. **Deploying a FinancingFlow fix without verifying version** — use `verifyVersion()` (logs to Executions tab, not alert) and confirm the script name before running the waterfall.