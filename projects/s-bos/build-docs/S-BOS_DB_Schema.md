# S-BOS DB Schema (Supabase) + SmartSuite Mapping
**Version:** 0.1 (back-fill in progress) | **Maintained by:** S-BOS build (Claude Code) | **Last Updated:** 2026-07-02

> **Purpose:** The live Supabase schema for the S-BOS rebuild, **plus the field-level SmartSuite↔Supabase mapping** that keeps the dual-ops sync and the eventual "point the tools/dashboards at Supabase" cutover seamless (not archaeology).
> **Provenance marks:** `[SS]` = back-filled from a SmartSuite field · `[native]` = Supabase-native (no SmartSuite source) · `[decided]` = a design decision · `[view]` = computed, not stored · `[pending]` = column exists but not yet populated/linked.
> **Sync key:** every mirror table carries `smartsuite_record_id` (unique) — the pairing key for Supabase↔SmartSuite sync. `departments.cutover_mode` (`bidirectional | supabase_native`) is the category-scoped cutover flag.
> **Scope:** covers what's built through migration 018. Blueprints (item 2), Knowledge Library (item 3), Skill/Dispatcher (item 5) added as built.

---

## Migration registry (applied to live Supabase)

| # | File | What |
|---|---|---|
| 001 | initial_schema | companies, people, projects, notes_comments, stakeholder_bridge, project_budget_items, project_dates, check_lists, check_list_tasks |
| 002 | seed_data | seeded companies/people/projects/etc. from SmartSuite (⚠️ **no smartsuite_record_id captured** — see Open Backfill) |
| 003–011 | (CRM POC + trackers + migration menu) | Biz Dev CRM polish, notes write-back hub, automation/migration trackers |
| 012 | credit_desk | loans, loan_transactions, document_library, `v_loan_servicing` view; check_list_tasks.loan_id + source_type/source_id |
| 013 | smartsuite_ids | added `smartsuite_record_id` to companies, people, projects, check_list_tasks, document_library |
| 014→015 | project_pillars (reverted) | pillars-as-table mistake; dropped — pillars are project sections fed by existing tables |
| 016 | departments | departments (Category tier) + projects.department_id |
| 017 | tasks_and_scheduling | **check_list_tasks → tasks**; v1 scheduling; polymorphic parent + parent_types registry; task_dependencies; decision-gate fields; check_lists.kind |
| 018 | comments | universal `comments` (polymorphic parent + follow_up_task_id) |

---

## Mirror tables (SmartSuite source of truth during dual-ops)

### `departments`  ←  SmartSuite **Stitser BUILT Departments** (`6858d867136a525adac28543`)
The Category tier of Entity→Department→Project. Product-line + functional depts in one table.

| Supabase column | Source |
|---|---|
| `name` | `[SS]` title |
| `description` | `[SS]` description (rich→preview) |
| `sage_id` | `[SS]` s23df82ca2 (Sage ID) |
| `cutover_mode` | `[decided]` bidirectional \| supabase_native |
| `org_id`, `entity_id` | `[decided]` multi-tenant / Entity tier — nullable, no FK yet |
| `smartsuite_record_id` | `[SS]` id |

### `loans`  ←  SmartSuite **Loans** (`69aba52da3fa0e7ebb7424f7`)
| Supabase column | Source |
|---|---|
| `title`, `description` | `[SS]` title, description |
| `interest_rate` | `[SS]` s84b2fa853 | 
| `points` | `[SS]` s2df60fff7 |
| `fees` | `[SS]` s4b050362f |
| `origination_date` | `[SS]` sc37744a27 |
| `payment_frequency` | `[SS]` s1f339aa19 (choice→label) |
| `first_payment_due_date` | `[SS]` s46896246a |
| `interest_only_months` | `[SS]` s98158fab7 |
| `amortizing_io` | `[SS]` s0bebf2413 (choice) |
| `amortization_length_years` | `[SS]` s568af5a08 |
| `duration_months` | `[SS]` s4e7e2bded |
| `manual_due_date` | `[SS]` sd55036ad8 |
| `late_fee` / `late_fee_calc` | `[SS]` sc41270a96 / sf0c498ffd |
| `date_paid_off` | `[SS]` s4671e8b6a |
| `renewal_date` | `[SS]` s97565e40a |
| `sb_asset_liability` | `[SS]` se3c1ade92 (choice: SB Asset/Liability/Serviced) |
| `is_primary` | `[SS]` s2a86152f3 (Primary?) |
| `loan_docs_url` / `loan_statements_url` | `[SS]` s139a0c9b5 / sd77473f92 (G-Drive linkfields) |
| `lender_id` | `[SS][pending]` safc15e3e0 (→Companies) — needs companies.smartsuite_record_id backfill |
| `borrower_id` | `[SS][pending]` s1d9e2c8b1 (→Companies) |
| `project_id` | `[SS][pending]` s5ebee7604 (Project/Property) |
| `parent_loan_id` | `[SS][pending]` sfcdb949d7 (Sub-Note) / s399c8dc1b (Parent ID) |
| `smartsuite_record_id` | `[SS]` id |
| *(SmartSuite formula fields: Status, Direct Principal Balance, Total Payments, Maturity, Next Payment Due…)* | `[view]` computed in `v_loan_servicing`, not stored |

### `loan_transactions`  ←  SmartSuite **Loan Transactions** (`69aba98a2d32fb2007e5290f`)
| Supabase column | Source |
|---|---|
| `title`, `description` | `[SS]` title, description |
| `transaction_type` | `[SS]` se767ab13d (Principal Funding/Paydown, Interest Accrued/Payment) |
| `due_date` / `date_paid` | `[SS]` s96e72a956 / s7b58f63ef |
| `amount` | `[SS]` s71e98448c |
| `interest_component` / `principal_component` | `[SS]` s6e1768d09 / s54d2df952 |
| `net_principal_impact` | `[SS]` s4889e6862 (authoritative principal delta → drives balance) |
| `late_fees` | `[SS]` s1879ce1fc |
| `loan_id` | `[SS]` se0fe13381 (Loan link) → resolved via loans.smartsuite_record_id |
| `payment_voucher_url` | `[SS][pending]` s583fc235a (filefield — files not yet migrated to Storage) |
| `smartsuite_record_id` | `[SS]` id |

### `tasks`  ←  SmartSuite **Check List Tasks** (`68a8e17251dc814e8c529f3f`)
The one universal Task table (renamed from check_list_tasks in 017).

| Supabase column | Source |
|---|---|
| `what_to_do` | `[SS]` title |
| `status` | `[SS]` status |
| `due_date` | `[SS]` due_date |
| `phase` | `[SS]` s229ef0768 |
| `assigned_to` | `[SS]` s93fe32a4b (Who → People, stored as name) |
| `project_id` | `[SS]` se5f41aa17 (Link to Project) → resolved |
| `checklist_id` | `[SS]` sz08dosv (Link to Check Lists) → resolved |
| `breadcrumb` | `[SS]` s1ab1a7516 (Breadcrumb formula) |
| `is_rating_task` / `assessment_rating` | `[SS]` s17e64ac91 / s3dcc09bbd (decision-gate ratings) |
| `loan_id` | `[decided]` Credit Desk link (no direct SS field; via title #NNN) |
| `parent_type` / `parent_id` | `[decided]` polymorphic parent (folded from old source_type/source_id — BD-01) |
| `duration_days`, `start_date`, `end_date`, `scheduling_mode`, `constraint_type`, `constraint_date` | `[native]` MS Project v1 scheduling |
| `smartsuite_record_id` | `[SS]` id |

### CRM back-fill tables (from migration 001/002 — Biz Dev CRM POC)
| Supabase table | SmartSuite source | Notes |
|---|---|---|
| `projects` | Projects `68216a706900e8eaf75a05a7` | `project_type` `[SS]` s4687ad08c (sub-classifier); `department_id` → departments `[decided]`; legacy `department` text kept; `smartsuite_record_id` `[pending]` |
| `companies` | Companies/Entities `68216a706900e8eaf75a05c0` | polymorphic (customer/vendor/lender/equity); `smartsuite_record_id` `[pending]` |
| `people` | People `68216a706900e8eaf75a05af` | `smartsuite_record_id` `[pending]` |
| `check_lists` | Check Lists `6a060dadc513b3329b7d4485` | `kind` `[native]` (task_checklist \| decision_gate); linked_decision_gate/final_decision `[SS]` |
| `stakeholder_bridge` | Project Stakeholder Bridge `6996a3079f04b5f34a06ad88` | project↔person + role/stage (the Team pillar) |
| `project_budget_items` | baseline budget | ⚠️ partial — full G-702/G-703/CO/Invoices not built |
| `project_dates` | (baseline dates) | the Schedule pillar's baseline |

---

## Native tables (no SmartSuite source)

| Table | Purpose |
|---|---|
| `parent_types` | `[native]` registry (parent_type → table) backing the polymorphic link on `tasks` + `comments`; validated by `validate_parent_ref()` |
| `task_dependencies` | `[native]` MS Project v1 (predecessor/successor, FS/SS/FF/SF, lag_days) |
| `comments` | `[native]` canonical universal comments (polymorphic parent + `follow_up_task_id`). **Legacy `notes_comments`** ← Notes & Comments `6894e64f621641b3ef90fa99` (multi-attach junctions) — reconciliation pending, BD-02 |
| `document_library` | `[native]` net-new; loan docs/statements/vouchers were G-Drive links + file fields in SmartSuite. Templates (`is_template`) in Storage bucket `documents` |
| `v_loan_servicing` | `[view]` computes the SmartSuite Loans formula fields (balance, paid, next due, maturity, status) from the ledger |

---

## Open backfill / reconciliation (for the seamless cutover)

1. **`smartsuite_record_id` on companies / people / projects / tasks** — columns exist (013), existing rows are NULL (seeded pre-column in 002). Backfill by name-match or re-seed → unblocks two-way sync for those tables.
2. **`loans.lender_id` / `borrower_id` / `project_id` / `parent_loan_id`** — need #1 (companies/projects smartsuite ids) first, then map the SmartSuite link fields.
3. **`notes_comments` → `comments`** merge + multi-attach decision — BD-02.
4. **Budget pillar** — G-702 / G-703 / Change Orders / CO Line Items / Bills & Invoices tables not built (project_budget_items is baseline only).
5. **File migration** — payment vouchers + loan docs (SmartSuite file/link fields) into Storage.
