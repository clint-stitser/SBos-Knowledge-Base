# S-BOS 2.0 — Brokerage File Type & Commission Economics

**Doc class:** design · **Captured:** 2026-07-04 · **Status:** built (migrations 040–043)

## Decision
A brokerage file is modeled as **projects** (not separate tables): an Agency Contract is a **parent** project (`brokerage_role='agency'`, `brokerage_client_type` buyer/seller/investor); each Property Contract is a **child** project (`brokerage_role='property'`, `brokerage_side` list/buy). This reuses the hierarchy, gates, tasks, budget, and portfolio machinery. Commission mechanics apply whenever a project has the brokerage aspect.

## Economics — the P&P disbursement waterfall
Replicates the Stitser Properties Policies & Procedures (rev 4/30/2025), validated to the cent against live deals (Fawn flat-fee, Hampton % + referral, Columbia client-paid fee) and the P&P mixed-cap example.

`v_brokerage_economics` (migrations 041–042):
1. Gross commission = side rate × sales price (percent) or flat + bonuses
2. − outside referral = **net pool**
3. Agent revenue = net_pool × production × company-split
4. Expenses (broker/transaction fee + TC + signs + photos + other) allocated **pro-rata by revenue share** — the **mixed-cap weighted model** (a capped agent takes more revenue, bears more expense)
5. − open payables = agent net
6. SP profit = split residual (company dollar) + transaction fee

`brokerage_deals` holds inputs; legacy imports carry a blended split, new deals carry production % + company-split % separately.

## Cap model
Calendar-year, tracked on **closing date**, on the **company dollar** (SP's share). **$35k standard** (Option 2: 70/30 until cap, then 100% less transaction fees); Option 1 = $25k in Q1. Shoulder escrows count toward next year; no rollover. `v_agent_cap_ledger` accrues per agent per year; only attributes when the production/company-split breakdown is explicit (legacy blended deals don't produce phantom cap figures). Cap amount configurable via `agent_caps`; category cap-eligibility via `split_categories.is_cap_based`.

## Tasks/docs
Contract & Disclosure MGT records folded into the unified `tasks` table (migration 040 added `is_document`, `doc_where_to_file`, `doc_audit_stage`, `doc_template_type`, `doc_final_url`). 590 records imported for active files.

## Import & UI
`scripts/import-brokerage.mjs` (active agency + property contracts → parent/child projects + deals), `import-brokerage-tasks.mjs` (docs/tasks). `/portal/brokerage` shows active files by client type, per-deal economics, and the agent cap tracker.
