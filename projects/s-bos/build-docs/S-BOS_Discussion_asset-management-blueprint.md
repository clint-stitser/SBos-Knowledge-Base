# Discussion: Asset Management Blueprint (7 Pillars, Thresholds Engine, Property Overlays)

**Status:** Drafted 2026-07-06 — pillar structure locked, structural placement locked, Thresholds Engine architecture proposed, three property overlays seeded pending loan-doc abstraction. Not yet built. Corresponds to **Pillar A (Execution/Org)** and **Pillar B (the Brain / Knowledge Library)** from the PDD §2.5 — this doc is the first full worked example of a non-construction, non-land-dev domain riding the same Blueprint/Goal/Knowledge-Library primitives.

**This is the canonical source for the Asset Management Blueprint.** If any other doc, note, or session (including Claude Code) shows a different pillar count or list, this file is the one to trust and reconcile against.

> **RECONCILED 2026-07-07 (Claude Code):** The Thresholds Engine is built by **extending the existing `goals` table** (migration `069_targets_thresholds.sql`), NOT as separate `thresholds`/`thresholds_versions`/`threshold_metrics`/`threshold_scope_dimensions` tables. This follows Clint's reuse directive and is *more* consistent with Decision #2 (pillars are goals): a pillar's enforcement metrics are the target/threshold **goals under that pillar**. §4's core-table proposal is superseded below; its precedence + evaluation semantics still apply.

---

## 1. What this is

A standardized annual operating framework for managing owned real estate assets (Cal Ave, Canyon's Edge, Mayberry Gardens, and future acquisitions), grounded in IREM's real estate asset management research rather than an internal ad-hoc list. Three components, confirmed in conversation:

1. **7 Pillars** — the annual goal structure asset management is organized around.
2. **Recurring Cycles** — the annually-activated Projects that do the actual work within/across pillars (budget season, insurance renewal, investor reporting, etc.).
3. **Configurable Thresholds Engine** — a generalized, cross-domain business-rules system (not asset-management-specific) that gives each pillar its enforcement teeth (DSCR covenants, capex authority limits, reserve minimums, etc.).

## 2. Decisions locked this session

| # | Decision | Notes |
|---|---|---|
| 1 | **7 pillars, industry-grounded via IREM.** Revenue; Customer Service & Retention; Asset Maintenance & Capital Planning (incl. Reserves); Financial Management, Budget & OpEx; Capital Markets & Debt Management; Investor Reporting & Relations; Risk, Insurance & Compliance. | Revises Clint's original rough 8 — merges Reserves into Maintenance, merges OpEx into Financial Mgmt, splits/elevates Risk & Insurance out of Compliance, renames Cap Stack → Capital Markets & Debt. Full research trail in the two prior research artifacts this session (IREM 10-function model; Dr. Dustin Read, 2017). |
| 2 | **Pillars are annual goals, not Projects.** They do not carry their own tasks/schedule/budget directly. | Matches the existing Goal/Priority/Situation strategic-layer pattern already locked in the PDD §2.5 Pillar A — a Pillar here plays the same structural role as a Company Goal / Strategic Priority, not a Project. |
| 3 | **Recurring Cycles are Projects, activated annually, carrying tasks/schedule/budget/people.** | This is the *operational* half of the existing Strategic/Operational two-track pattern. A Cycle (e.g., "2027 Budget Season," "Cal Ave Insurance Renewal," "Q3 Investor Reporting") is instantiated as a Project each year — via the existing Blueprint Catalog activation mechanism — and its outcomes **roll up (contribution, not parent-child)** to one or more Pillars, exactly like Projects roll up to Situations/Goals today. |
| 4 | **Knowledge Base content links to the owning legal entity, not to Pillars.** | Pillars are goals; goals don't hold reference knowledge. KB entries (lease abstracts, loan documents, PCA/PCNA reports, insurance policies, vendor contracts) scope to **Stitser BUILT** (platform-level KB) and to **each asset-owning entity** (Cal Ave LLC, Canyon's Edge LLC, Mayberry Gardens LLC, etc.) per the existing Knowledge Library pattern (`entity_id` scope, rolls up via query filter — same mechanism already used for Assets in `S-BOS_Discussion_asset-schedule.md`). |
| 5 | **Configurable Thresholds Engine is a generalized, cross-domain system — not asset-management-specific.** | Must serve construction/land-dev/brokerage thresholds (change-order approval limits, schedule variance alerts, safety incidents) with zero schema change, using the same scope-dimension registry pattern already used for Comments/Tasks (`parent_types` registry). Architecture proposed in §4 below. |
| 6 | **No MH-owned assets currently — build overlays only for what's actually managed.** | Three property overlays seeded in §5: Cal Ave (Fannie Mae multifamily), Canyon's Edge (HUD 223(f)), Mayberry Gardens (Class A office + NNN + executive suites, private seller note). |
| 7 | **Threshold seed values are starting points, not covenants — to be finalized from actual loan documents and underwriting history per asset.** | Every numeric threshold in §5 is flagged `status: pending_loan_docs`. **Mayberry's seller-note covenants specifically must not be seeded until the promissory note is reviewed** — seller-note terms are non-standardized by nature (§5.3). |

## 3. The 7 Pillars (industry-grounded)

| Pillar | Scope | Primary KPIs (seed) |
|---|---|---|
| 1. Revenue | Effective gross income, occupancy, renewal trade-outs, ancillary income | NOI, economic/physical occupancy, loss-to-lease, ancillary income % |
| 2. Customer Service & Retention | Resident/tenant experience, renewals, turnover | Retention rate, turnover rate, turn cost/unit, satisfaction/NPS |
| 3. Asset Maintenance & Capital Planning (incl. Reserves) | Preventive maintenance, capex planning/execution, reserve funding, vendor mgmt | Reserve funding/unit/yr, capex budget vs. actual, deferred maintenance backlog |
| 4. Financial Management, Budget & OpEx | Annual budget, variance analysis, expense optimization, cash mgmt | OER, expense/unit, budget-to-actual variance, NOI margin |
| 5. Capital Markets & Debt Management | Debt service/covenant monitoring, maturity/rate-cap tracking, refi/hold/sell | DSCR, debt yield, LTV, loan maturity, rate-cap expiration |
| 6. Investor Reporting & Relations | LP/owner/lender reporting, distributions, capital accounts | Net returns vs. projections, distribution cadence, capital account accuracy |
| 7. Risk, Insurance & Compliance | Insurance renewal, regulatory compliance, COI tracking, emergency prep | ITV coverage %, COI compliance rate, days-to-renewal, open violations |

Full pillar detail (cadence, ownership, recurring tasks, tools, if-then branches, property-type overlays) is in the research artifact from earlier this session — to be transcribed into this doc's §6 (Knowledge Library seed content) in a follow-up pass, not duplicated here to avoid drift between two sources.

## 4. Configurable Thresholds Engine — proposed architecture

**Positioning:** a sibling catalog to the Blueprint Catalog and the Kompass skill catalog — all versioned, Supabase-owned, `org_id`-scoped catalogs the Dispatcher and Kompass read as data.

### Core model — REUSE `goals` (reconciled 2026-07-07; supersedes the new-table proposal)
Realized by **extending `goals`** (migration `069_targets_thresholds.sql`) — a threshold IS a goal with an operator + severity. No new catalog tables. `goals` now carries:
- `kind` (goal | target | threshold | covenant); `metric` + `target_unit` serve as the metric registry (no separate table).
- `operator` (`< <= > >= between = !=`), `target_value` / `value_high` (low/high bounds), `unit` via `target_unit`.
- `severity` (info | alert | escalate | block) and `action` jsonb (`{create_task, notify, escalate_to}`).
- `scope` jsonb for any dimension beyond the `entity_id`/`department_id` columns (asset_type, product_line, project_type, …) — **zero schema change to add a domain**, so construction / land-dev / brokerage thresholds work as-is. The scope-dimension "registry" is the documented set of recognized `scope` keys, not a table.
- `source` / `source_ref` / `review_status` (provenance + `pending_docs` flagging); `is_template` / `blueprint_id` (blueprint templating); `metadata`.
- Versioning/audit via the existing `fn_audit` backstop (migration 066) — no `_versions` table for v1.

Pillars = goals (Decision #2); each KPI = a target/threshold goal under its pillar via a nullable `parent_goal_id`; recurring cycles (Projects) contribute via `priority_contributions`. GYR rolls up leaf KPI → pillar → entity.

### Precedence resolution — "most-specific-scope-wins"
A specificity score is computed from how many (and which) scope dimensions a row matches; the highest-specificity matching row wins. Equal-specificity ties resolve via an explicit `precedence` integer or a documented fixed dimension-priority order (proposed default: entity > asset_type > project_type > product_line > department). This mirrors Salesforce Hierarchy Custom Settings (Org → Profile → User) and Claude Code's own settings-scope resolution — an established pattern, not novel.

### Evaluation → alerting/workflow wiring
Stateless evaluator (matches the Kompass Dispatch model — Claude/logic invoked as a compute call, not a standing process): a fact arrives (new operating statement, capex request, change order) → load matching active threshold rows → resolve precedence → emit the winning row's `action` → `alert`/`escalate` write into the existing universal `tasks` table or a Comment via the polymorphic `parent_type`/`parent_id` registry; `block` returns a hard failure to the calling workflow.

### Open build questions (not yet decided)
- Whether to add GIN indexes / generated columns on hot scope keys (`entity_id`, `asset_type`) now or when performance requires it.
- Whether Kompass gets write access to propose threshold changes (draft → Clint approval, per the existing skill review-gate pattern) or read-only access at first.
- UI surfacing of "which scope won" for a given threshold evaluation — flagged as important for trust but not blocking v1.

## 5. Property Overlays (seeded, pending loan-doc confirmation)

> **Every numeric value below is an industry-standard seed, not a confirmed covenant.** Status = `pending_loan_docs` on every row until Clint's actual loan documents, PCA/PCNA, and (for Mayberry) the promissory note are abstracted and override these as more-specific entries.

### 5.1 Cal Ave — 36-unit studio community (Fannie Mae multifamily + PE)

| metric_key | seed value | severity | source note |
|---|---|---|---|
| dscr | < 1.25 alert; < 1.15 escalate | alert/escalate | Fannie Tier 2 underwriting floor; confirm actual covenant from Loan Documents |
| reserve_per_unit | < $250/unit/yr | alert | Agency floor; PCA may require $250–300/unit/yr |
| occupancy_physical | < 90% | alert | Agency stabilized norm |
| expense_variance_pct | > 10% vs. budget | alert | Ties to quarterly Form 4254 review cadence |
| operating_stmt_due | quarterly + annual | escalate if late | MAMP reporting cadence |

### 5.2 Canyon's Edge — 48-unit mixed-size community (HUD 223(f) + PE)

| metric_key | seed value | severity | source note |
|---|---|---|---|
| dscr | < 1.15 alert; < 1.11 escalate | alert/escalate | Market-rate 223(f) per Mortgagee Letter 2025-03 — **confirm whether Canyon's Edge closed under 1.15x or the older 1.176x standard** |
| reserve_per_unit | < $250/unit/yr | alert | HUD floor; actual PCNA-set deposit likely higher |
| audited_afs_due | 90 days post-FYE | escalate if approaching | REAC/FASSUB hard deadline |
| reac_inspection | score-driven cadence | info | Track last score + next inspection window |
| surplus_cash_distribution | > surplus cash | block | HUD Regulatory Agreement limits distributions to surplus cash |

### 5.3 Mayberry Gardens — Class A office park, NNN + executive suites (private seller note + PE)

| metric_key | seed value | severity | source note |
|---|---|---|---|
| occupancy_office | < 90% | alert | Class A stabilized target |
| walt_years | < 3 | alert | Rollover-risk trigger; deal-specific, adjust to actual rent roll |
| tenant_retention | < 75% | alert | Industry benchmark (80%+ considered strong) |
| cam_reconciliation_due | 90–120 days post-FYE | escalate if late | Lease-governed; check each NNN lease's actual window |
| exec_suite_occupancy | < 75% | alert | Flex/exec-suite break-even proximity |
| exec_suite_churn | > 5%/mo | alert | Flex-office health benchmark |
| **seller_note_*** | **NOT SEEDED** | — | **Do not create debt-covenant thresholds until the promissory note is reviewed and abstracted — seller-note terms are non-standardized and entirely deal-specific (no agency rulebook exists for this debt type).** |

## 6. Open items / next steps

- [ ] Transcribe full per-pillar detail (cadence, ownership, recurring tasks, tools, if-then branches) from this session's research artifact into a Knowledge Library seed doc, scoped to Stitser BUILT (platform-level, not per-entity).
- [ ] Abstract Cal Ave's actual Fannie Mae Loan Documents; overwrite §5.1 seeds with confirmed covenants.
- [ ] Abstract Canyon's Edge's actual HUD 223(f) Regulatory Agreement + PCNA; confirm DSCR vintage (1.15x vs. 1.176x); overwrite §5.2 seeds.
- [ ] **Abstract Mayberry Gardens' seller promissory note before seeding any debt-related threshold** — currently blocked, by design (Decision #7).
- [ ] Decide Kompass's write access to the Thresholds catalog (propose vs. read-only) — not yet decided (§4).
- [x] Thresholds Engine realized via the `goals` extension (migration `069_targets_thresholds.sql`), reconciled 2026-07-07 with Clint's reuse directive — NOT new tables. Remaining: `parent_goal_id` pillar linkage, the precedence resolver, and wiring the GYR health engine as the evaluator.
- [ ] Model each Recurring Cycle (budget season, insurance renewal, investor reporting, CAM reconciliation) as an activatable Blueprint, per Decision #3.
