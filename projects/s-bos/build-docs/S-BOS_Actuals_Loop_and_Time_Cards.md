# S-BOS 2.0 — Project Actuals Loop & Time Cards

**Doc class:** design · **Captured:** 2026-07-04 · **Status:** built (migrations 045–046)

## Decision
S-BOS is the **operational layer** that records what actually happens on a project and **exports to whatever accounting system a licensee runs** — it is *not* a general ledger. This keeps it a business/life operating system, not an accounting product, and keeps it licensable across tech stacks.

## The loop
Estimate (already built) → **Buyout** → **Change Orders** → **Bills / Pay Apps** → **Sync/Export**. Each stage feeds the budget line's `committed_amount` / `actual_amount`, which the existing `v_uw_cost_effective` (actual ?? committed ?? bid ?? estimate) already consumes — so the estimating UI lit up with no view changes.

## Data model (migration 045)
- **commitments / commitment_lines** — buyout (subcontract/PO); lines point at `uw_cost_items`. `executed` status → committed.
- **change_orders** — carry **both** `cost_delta` and `revenue_delta`. Convention: **owner COs move revenue / contract sum; subcontract COs move cost** (both fields available on any CO). CO **line items are enriched `tasks`** (`change_order_id`, `co_cost_delta`, `co_line_kind`) — consistent with "a line item is a task with special data."
- **invoices / invoice_lines** — AP (`payable`) and AR (`receivable`) share one shape via `direction`. Retainage defaults **5%, editable on every invoice**.
- **pay_apps / pay_app_lines** — SOV-driven **G-702/703 generated from the budget lines**; `v_pay_app_702` computes the cover sheet (contract sum, completed, retainage, prior payments, current due).
- **export_profiles** — a licensee defines the **format (file: csv/iif/json) or API connector** and a `field_map`; every doc carries `export_state` + `export_ref`. No hardcoded ERP.

## Time cards (migration 046)
- **person_rates** (person × entity × effective_date) → cost_rate / bill_rate.
- **time_entries** — plain and obvious: a person logs hours against a project/task/budget line. A kid logging 2 hours for an allowance task is a valid entry. No ritual/scoreboard coupling.
- **Billable time charges against GC lines** — PM time is charged to the General Conditions budget line (e.g. the "PM" 703 line), per the disbursement model.

## Accumulation engine (migration 046)
Triggers keep the budget columns authoritative from the docs:
- `committed_amount` = executed commitment lines + approved **subcontract**-CO deltas
- `actual_amount` = approved/paid **payable** invoice lines + approved time (hours × cost_rate)

## Validated
Buyout $90k + approved sub-CO $10k = committed **$100k**; payable invoice $40k = actual **$40k**; 100 hrs × $150 PM time = **$15k** actual on the GC line.

## Open / future
GL export adapters per entity (Intacct/QBO) wired last; owner-AR pay-app → revenue-actual accumulation; expense capture.
