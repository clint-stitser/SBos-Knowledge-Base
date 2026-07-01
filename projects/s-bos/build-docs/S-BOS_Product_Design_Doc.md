# Product Design Doc: S-BOS

> **Scope:** The S-BOS platform vision, with the **Biz Dev CRM module as Phase 1**.
> **Method:** Back-filled mid-stream from the running POC + planning docs. Items marked 🔎 are *reverse-engineered* or *synthesized from session decisions* and need Clint's confirmation; items marked ❓ are open gaps.
> **Canonical references:** the Product Vision & Architecture (§2.5) is a *summary*; the source of truth is the platform design set in `SBos-Knowledge-Base/projects/kompass/platform/` — `platform-pdd.md`, `shared/brain-model.md`, `shared/intelligence-stores.md`, `shared/operator-assistant.md`, `shared/feed-model.md`. This PDD captures the S-BOS-specific decisions, not a re-paraphrase of those docs.

---

## Status & Next Steps

| Section | Status | Notes |
|---------|--------|-------|
| Problem Statement | ✅ Done | Signed off 2026-06-29 |
| Target Users / Personas | ✅ Done | Personas + access model |
| 🚦 Gate 1 | ✅ Done | **Signed off by Clint 2026-06-29** |
| Product Vision & Architecture | 🔄 In Progress | Four pillars + Blueprint Catalog + TSW module catalog — deepens as features develop |
| Core Entities | ✅ Done | **Signed off (Gate 2) 2026-06-29** — locked for now; deepens in later stages |
| 🚦 Gate 2 | ✅ Done | **Approved by Clint 2026-06-29** |
| Core Features | 🔄 In Progress | Phase-1 screens back-filled (screen-by-screen); Clint layering in + AI/automation per page |
| User Workflows | ⏳ Not Started | |
| 🚦 Gate 3 | ⏳ Not Started | |
| Out of Scope · Timeline/Phases | 🔄 In Progress | **Phase 1/2 split set** (brokerage + credit desk → Phase 2) |
| Success Metrics · Tech Constraints · Open Questions | ⏳ Not Started | |
| 🚦 Gate 4 | ⏳ Not Started | |

**Status scheme:** ⏳ Not Started → 🔄 In Progress → ❓ Needs Discussion → ✅ Done
**Working note (Clint, 2026-06-29):** features will keep developing in this PDD; we accept that each layer isn't 100% described before moving on. Capture what's decided, mark the rest 🔎/❓, advance.

---

## 1. Problem Statement

**Problem:** Stitser BUILT runs operations on SmartSuite (database) + Softr (portal). Three structural problems:
1. **SmartSuite API limits** throttle how much Claude can read/write — the ceiling was hit early, capping Claude's usefulness as the system grows.
2. **No-code tooling makes Clint the bottleneck** — schema, formulas, and automations change only by hand in SmartSuite's UI; the system can't be serviced/extended by Claude without a human in the loop.
3. **It can't be owned, licensed, or franchised** — two third-party SaaS products means no path to productize the system as proprietary IP.

**The deeper problem (sets up the Vision):** beyond escaping those limits, there is **no domain-structured operating system** — one that *knows* an operator's world (entities, projects, vendors, decisions) and can *act* on it through a trained assistant. Generic tools have no domain memory; the existing stack has memory but no intelligence layer and no ownership path.

**Affected users:** Clint (builder/admin — single point of structural change); the internal team (biz dev, PMs, accounting — depend on it working).

**Consequence:** API limits cap automation/AI; every structural change funnels through Clint; ongoing vendor dependence; no route to license/franchise the system as IP.

**Current workaround:** SmartSuite (DB + no-code) → Softr (portal, iframing Railway dashboards) → Claude via SmartSuite MCP. Insufficient on all three counts above.

> ✅ Confirmed by Clint 2026-06-29.

---

## 2. Target Users / Personas

> **The CRM is the platform's shared backbone — not a biz-dev silo.** People and Companies touch *every phase of every project*, filling **customer / vendor / internal staff / investor-lender** roles depending on context. So **all internal roles are primary users**, and an entity's "role" is **contextual, not a fixed type** → People/Companies relate to projects through *role-bearing relationships*, not hard types (drives Core Entities §3).

### Persona: Internal Staff (all roles) — **Phase 1 primary**
Relationship Managers / Biz Dev, PMs, Accounting, Execs. **Goal:** manage relationships and project progress. **Common action:** working a list → open a record → file notes/comments with follow-ups. **Constraint:** must not be disrupted by the migration — feels like Softr; works fast through lists. **Increasingly acts *through* the Kompass assistant** (see §2.5-C).

### Persona: Clint (Admin / Builder) — **secondary, system-wide**
**Goal:** build/service/extend the system *through Claude*, not a no-code UI. **Common action:** directing Claude to change schema/data/automations; reviewing docs + the running app. **Comfort:** high.

### Persona: External Stakeholder (investor / broker / client) — **future (Auth Phase B)**
View-only on scoped, explicitly-shared records; magic-link login.

### Persona: Franchisee / Licensee — **first-class, expansion (Auth Phase C)**
Runs their own isolated **`org_id` instance** of the universal shell, scoped to their business and vertical. This is the productization target (see §2.5).

### Access & Permissions Model (confirmed)
| Audience | Access | Notes |
|---|---|---|
| **Internal users (all roles)** | **C·R·U** | + audit trail + **60-day restore**. No hard delete. |
| **System admin (Clint)** | CRU + **Delete** | Delete is admin-only. |
| **External stakeholders** | **View-only**, scoped | Read on certain shared elements — defined at Auth Phase B. |

> Reconciles with `sb-crm-poc/docs/auth-plan.md` (roles: admin / internal / external; `org_id` multi-tenant) and `recovery-and-restore-plan.md` (60-day soft-delete, hard delete admin-only, audit log on all CRU).

---

## 🚦 Gate 1 — Problem + Users Sign-Off

**Checklist:** all passed (problem-not-solution ✓, workaround ✓, consequence ✓, every persona has goal/action/constraint ✓, primary identified ✓, no unresolved conflicts ✓).

> 🚦 **Gate 1 — ✅ APPROVED by Clint, 2026-06-29.** Problem + Target Users complete and consistent. Proceed to Core Entities (informed by the Vision below).

---

## 2.5 Product Vision & Architecture

> **Summary of the canonical platform docs (see references at top). Marked 🔄 — deepens as features develop.**

**Thesis.** S-BOS is one instance of a **universal operator shell**: a domain-structured operating system + a trained assistant that knows the operator's world and can act on it. The four Kompass products (Developer / Contractor / Agent / **Personal**) and S-BOS-internal are **configurations of the same shell**, isolated per **`org_id`** (multi-tenant → licensable/franchisable). **The Stitser Way ("TSW") is the Personal Kompass configuration — not a separate app.** The moat is the **domain-structured Brain** (vs. MarketingSecrets.ai's flat Brain — see `research/marketingsecrets-analysis.md`).

### Pillar A — Execution / Org design
- **Universal hierarchy:** `Entity → Category → Project`. Each Project carries **People · Tasks · Budget · Schedule · Outcome/Purpose**.
- **Master Property (persistent anchor, brain-layer — confirmed Clint 2026-06-29):** a parcel/asset that **outlives any single project**. Many projects come and go against one Property over time; whenever the parcel re-enters an operator's world they see its **entire history** (projects, tasks, comments) against it. Projects *anchor to* a Property; the Property accumulates longitudinal history independent of any one project. *(Only the dead "Project Prioritization Tool" link was dropped — not the Property entity.)*
- **Two parallel tracks** (`shared/brain-model.md`): **Strategic** (working *on* the business: Goal → Priority/Sprint → Milestone) and **Operational** (working *in* it: Project → Outcome/Phase → atoms), joined by **roll-up = a contribution tag, not parent-child** — a project contributes to multiple priorities across periods without being owned by any. Today's roll-up dims (`project_type` + `department`) are the seed of this.
- **Winning the Game (the Strategic-layer measurement model).** The Goal isn't pursued in the abstract — **you win one Critical Situation, one Project, one Project Pillar at a time.** The Goal decomposes into **Key/Critical Situations** (strategic fronts — markets, product lines, business lines), each with a **production target**; each Situation holds **Projects**; each Project advances through **Pillars** (gated milestones, complete/partial) — and **Pillars are discipline-scoped**: one Project may run land-dev, construction, and brokerage pillars in parallel, each its own track (see §3 multi-discipline projects). A **Goal Scoreboard** grades progress GYR and rolls **Pillar → Project → Situation → Goal** via a **3-bucket projected score** (Billed = G-702 actuals · Scheduled/committed · Pipeline) plus **Pipeline-by-Stage** movement (each stage with a deadline). This is how `project_type`/`department` roll-up + the contribution tag become a **scoreboard, not just a filter** — the operator always knows the single next critical situation/project/pillar that moves the goal. *(Modeled on the live 3rd-party Construction Scorecard — `sb-planning-tools/dashboards/construction-scorecard.html`: Goal Scoreboard tiles, Production Targets by Key Situation, Pipeline by Stage, pillar-complete/partial chips.)*
- **Phase gating drives discovery → commitment.** Stage advancement evolves a project's **control tools (schedule · spec · budget)** from *estimates* → *baseline*. Pre-baseline = **discovery & refinement**; once a baseline locks (a phase reached, a contract signed, a decision made), **the game is on** — posture shifts to **competition & commitment** (actuals vs. baseline). This is a core reason the **Blueprint Catalog** matters: it makes early estimates as accurate as possible, and the **estimate → reliable baseline** path as fast as possible.

### Pillar B — The Brain (domain-structured memory; `shared/intelligence-stores.md`)
Six intelligence stores the assistant draws from. **Status:**
- ✅ **People Profiles**, **Decision Matrices**, **Decision Log** — exist in SmartSuite with real data.
- 🔄 **Vendor Ratings** — build via a `reviews`→`companies` rollup (or a field on Notes & Comments linked to Companies).
- 🔄 **Knowledge Library** — *to build.* Currently scattered (Drive / GitHub / Gmail). Target: a scoped Postgres store (RLS + pgvector), **scoped by entity / category / skill / vertical**, multi-user-buildable. Reference (“what it means / why it matters”).
- ✅ **Project Data** — the operational substrate (largely built in the POC).
- **Master Property** lives in this layer (the persistent-anchor store above).

### Pillar C — The Kompass operator assistant + the Feed (`shared/operator-assistant.md`, `shared/feed-model.md`)
- A **domain-trained Chief of Staff** with persistent Brain access + skill execution. Surfaces intelligence via **context triggers** (record / skill / explicit query) — not a bare chat box.
- The **Feed** is its outbox: every action is **drafted → Approve / Edit / Skip / Defer**; nothing external ships without a tap. Full run history = the **audit log** (answers the team's "is the system logging actions?" ask).
- **First surface = the Task-Level AI Assistant ("person in the room"):** at a task, search connected sources (Gmail via MCP) by the project's people/vendors, report status, draft/send the email on approval, check task notes/comments, surface a shared project brain.

### Pillar D — Execution Tools
Lightweight, purpose-built apps that *facilitate execution* (distinct from skills, which are LLM playbooks): **budget builder, study app, pay-app runner, scorecards, underwriting model.** They read/write the Brain. This is the platform's "Software" layer; it is part of how the product becomes licensable (an operator installs a vertical's tools).

### The Blueprint / Template Catalog
A Brain-layer store, **sibling to the Knowledge Library**, holding **reusable, activatable bundles** a user drops onto a record. Knowledge Library = *reference*; Blueprint Catalog = *structure to instantiate*.
- **What a blueprint bundles** (per the CrossMod land-dev example, `crossmod_land_dev_stage_gate_framework.html`): a **stage-gate framework (G0–G5)** where each gate carries **entry/exit criteria, go/no-go decision questions, team & AHJs (roles + consultants + jurisdictions with criticality), task lists, cost items (with fixed/variable/contingent type), durations, if-then branches, and jurisdiction overlays.**
- **Home = the Category.** A Category (e.g. "Entry-Level Housing / CrossMod product line") carries a default blueprint; new Projects under it **inherit/activate** it. Scoped by category/vertical via RLS (same pattern as the Knowledge Library), so different users build out their own.
- **Activation = instantiation:** a server action stamps the bundle (tasks, budget lines, schedule, roles, tool refs, skill refs) onto the target Project; the Feed shows what was created for approval.
- **A multi-discipline Project activates *multiple* blueprints** — one per discipline (land-dev, construction, brokerage) — each stamping its own pillars, G-702, checklists/audit cycles, mini-apps, and skills, all rolled into the Project's single parent-child schedule (§3).

### The TSW module catalog (carried into the shell)
TSW's frameworks become **modules of the universal shell** — *universal* (Capture, Today/Day-Mode, Horizon rings, dashboards-by-Category/Department/product-line) and *personal-config* (Journals/Rituals, GYR Spiral, Scoreboard, **Habit building**), whose patterns generalize to any operator's reflective practice. **Removed per Clint 2026-06-29: "Kevin's Rule" and "Misogi" (confusing).** Habit building stays.

> 🔎 Items in §2.5 trace to Clint's session decisions (2026-06-28/29) + the canonical platform docs; confirm/adjust as features deepen.

---

## 3. Core Entities
🔄 In Progress. Back-filled from the live Supabase schema (`sb-crm-poc`) + §2.5 Vision + the 2026-06-29 live-app research (Clint's answers). 🔎 = inferred, confirm. Field-level detail back-fills when DB Schema is worked.

### The three drivers — People · Companies · Projects
Everything links to these three.

- **Companies are polymorphic across the four financial-statement roles — Revenue (customer) · Cost (vendor) · Lender (debt) · Equity Partner —** and one Company holds **multiple accounting identities at once** (`vendor_id`, `customer_id`, `location_id`): the same company can be a customer on one project, a vendor on another, and an owned entity/location on a third. **Roles are contextual per project/department, never hard-typed.** (Live `companies` already carries `intacct_vendor`, `intacct_customer_id`, `intacct_location_id`.)
- **People** likewise play contextual roles (team, agent, contact, signer…) via role-bearing links, not types.
- **People carry relationship tiers** (Channel Account → Referral Partner → Top-50/Newspaper List), **driven by status-as-customer, referral count, or a specific tag** — not hand-sorted. The **Account/Authority Pyramid is a mini-app (Execution Tool, Pillar D) for business-development efficiency & context** — it surfaces the right relationships, tiers, outreach cadence, and an Audience-Health target ("Authority Lock") so biz-dev runs fast and informed. (The *tiers/tags* are People data; the *Pyramid* is the tool that works them. Ties to TSW "Stay in Flow" / Printed-Newspaper outreach.)

> **One universal Task table.** The tasks feeding **Goals/Priorities/Milestones** are the **same table** as project/checklist/meeting-follow-up tasks. SmartSuite currently has several task tables; they **consolidate into one robust Task table** (a task is *typed/linked* to either the strategic ecosystem or a project/checklist/meeting — not split across tables).

### Project — the hub, one entity across verticals
One **Project** spans **construction/development AND brokerage** — the SP (Stitser Properties) app **merges into the same shell/tables.** A brokerage project is simply a Project of a brokerage **type/Category** with different facets (Agency Contract, Property Contracts), different **checklists** (required docs & disclosures), and different **budget items** (commission splits, signs, …). Live Project facets: Details · Decisions/Ratings · Reporting/Planning · Team · Project Drive · Schedule · Tasks/Checklists · Budget(s) & Pay App(s) · Project CRM, + Parent/Child relations + comments. (Formation Projects = a sub-type.)

**A Project can span multiple disciplines / entities / teams at once.** An entry-level-housing deal can carry **land development, construction, AND brokerage** work in parallel, each discipline contributing **its own Pillars**, its own **Blueprint + mini-apps/catalogs**, its own **Budget (G-702)**, and its own **QC / required-doc checklists & audit cycles** — so a single Project legitimately has **multiple G-702s and multiple checklist/audit cycles** (note the live facets are already plural: "Budget(s) & Pay App(s)"). A **single Schedule with parent-child views** unifies them so the full cross-discipline flow is visible in one place. *(This matters most for the **platform operator** running several disciplines on one deal; a third-party tenant using a single vertical won't need the multi-discipline layering — it's enabled by config, not forced.)*

### Decision Gates / Ratings (Project facet)
The go/no-go **scoring engine** that drives prioritization + phase-gating. Per-gate weighted ratings — **Strategic Fit · Market/Product Fit · Financial Viability · Constructability · Jurisdictional & Legal · Operational Capacity → Overall Assessment → Final Decision** — tied to a **Decision Gate** (live in the Check Lists schema as those rating formula fields). This **is** "Project Prioritization = a feature" and the live counterpart to the Blueprint's per-gate decision questions; a project advances stages by clearing its gates.

### Master Property — a LINK / lens, not a parent
A **Property** (address/parcel; **lots are Properties too**) is a **reference anchor, not a hierarchical owner.** Projects, Notes/Comments, Contracts, and Loans **link to** a Property; querying an address returns **all linked history.** *(E.g. type "123 Main St" in 2030 → instantly surface every CRM note, both project records with their decision/DD checklists, the contract docs.)* Property is the **history-value lens** — consistent with roll-up-not-parent.

### Contracts (brokerage)
**Agency Contract → Property Contract → Close of Escrow.** Property Contracts link a Property + (as a brokerage Project's facet) the Project. The **agent and the brokerage are Companies linked to the Project** — which **drives access** (see below). "Match My ACH Deposits" reconciles commission deposits.

### Loans (Credit Desk)
**Loan** = first-class entity: Lender · Borrower(s) (Companies) · Rate · Principal · Due/Maturity · Status. **Secured loans** link to a Project/Property; **operating loans** are standalone (Financing department). Lenders are Companies (e.g. CSFT).

### Time Cards → costing
**Time Card** dimensions — **Cost Code · Department Code · Project ID · Customer ID · Account Code · Hours.** Direct roles (G-703 line like "Superintendent") cost to a Project; overhead roles (e.g. accountant) allocate to a **Department + the entity/customer served.**

### Accounting reference layer — mirrored; accounting = source of truth
Cost Codes, Department Codes, Account Codes, and entity IDs (vendor/customer/location) live in **reference tables mirrored from the accounting system (Intacct today, QB future) — the accounting software is the single source of truth** for the codes that tie back to the GL. Budgets (G-702/G-703), Time Cards, and financials all draw codes from this one mirrored set.

### Budgets & Pay Apps (the financial spine + AP workflow)
🔎 *From the live schema (Layer A breadcrumb work) + the `pay-app` / `pay-app-audit-checklist` skills; the live Budget tab wouldn't render via automation — confirm UI specifics.*
- **G-702** (Project Budget Mgmt) — the Application-for-Payment summary (contract sum, change orders, completed/stored, retention, balance). **A multi-discipline project can have multiple G-702s.**
- **G-703** (Budget Line Items) — the schedule of values by **cost code** (e.g. "Superintendent"); scheduled value, work completed this period/to-date, retention, balance. Evolves **estimate → baseline** at the gating phase (Pillar A); **baseline edits go through a change-order / revision log** (approval-gated — see Access).
- **Pay Apps** — periodic (monthly) pay applications stamped **PA-N**; current-period values roll up from G-703; retention release; **lock workflow**.
- **Bills & Invoices** — sub/vendor invoices linked to Project + G-702 + line item + Pay App, with **parent/child invoice splits** (live: `Parent Invoice Being Split` / `Child Invoices`; e.g. `[SPLIT] Property Acquisition`).
- **Invoice processing + compliance checklist** — a per-invoice **PM audit** (standard audit tasks: value, completion, documentation, lien waiver) linked to each invoice.
- **Pay App print → sign-off → AP** — generate the **Application-for-Payment package** (G-702/G-5 + NRS conditional lien waiver + draft email) to the PA Drive folder for **Senior Management sign-off** and **Accounts Payable approval**.
- **Change Orders → CO Line Items** feed G-702/G-703 (revised contract sum). Cost/account codes come from the **mirrored accounting reference layer**.

### The Game — Strategic layer (Goal → Situation → Priority)
**Company Goal** (Purpose · Metric · Target · Progress · GYR · Date) → **Situations** (operational pipeline-planning fronts the value chain flows through — Biz Dev → Underwriting → Construction → Customer Service, plus strategic situations) → **Strategic Priorities** (target a stat in a Situation by a date). The relationship is **roll-up/contribution**: *Projects → Goals/Priorities* mirrors *Priorities → Situations* — a project **contributes**, it isn't owned. `org_id` keys every row (multi-tenant / licensing).

### Access model (confirmed 2026-06-29 — Clint)
Enforced in **Supabase RLS** (policy-as-code), not UI toggles. **Groups:** Customer · Investor · Vendor · Production Staff · Production Manager · Department Head · Senior Management (· System Admin).

**Rules:**
- **Meetings:** visible if the user was **invited** OR is **Senior Management**. If the meeting is **project-specific** (e.g., an OAC meeting) → **all project participants** see it.
- **User-level items not linked to a shared Project/Person/Company** (tasks, journals, notes/comments) → visible to **that user only** (private).
- **CRU on Project · People · Companies + all supporting tables** (notes/comments, decisions, tasks, GYR reports, 702/703, change orders, budgets, checklists):
  - **Management** access is **driven by the entity link** → people linked to that entity get access.
  - **Production Staff** access is **driven by the Project Stakeholder Bridge** (you're a stakeholder on the project → you get the project + its supporting records).
- **Baseline editing** (change orders to **schedule, budget & scope** via the change-order/revision log) requires **Senior Production Manager or higher** (Senior PM / Construction Dept Head) **approval**.
- **Invoices · project budgets · AR/AP reports · project financials** → accessible to **Production Staff**.
- **Department financials** → **Department Heads + Senior Management**.
- **Entity financial tables** (loans, time-card reports, entity financials) → **Accounting Dept Head + Senior Management** only.
- **Admin tasks** (system fix, data-structure changes, record deletion, record restore) → **System Admin** only.
- **Entity / Department / Project Dashboards** → access based on **selections made at the dashboard level**.

> These map to RLS policies keyed off entity links + the Stakeholder Bridge + group/role flags; detailed in `sb-crm-poc/docs/auth-plan.md` + the Tech Spec.

> 🔎 §3 traces to the live schema + Clint's 2026-06-29 answers + the app research. **Gate 2 signed off 2026-06-29 — locked for now; entities deepen as later stages unlock.**

## 4. Core Features
🔄 In Progress — back-filled **screen-by-screen** from the live Softr app (2026-06-29/30). This is the **Phase-1** surface (Phase-2 brokerage / credit-desk screens deferred, §9). **🔎 = Claude's observation — Clint confirms/extends;** **AI/Automation** lines are *proposals to shape* (each = the Kompass assistant + Feed scoped to that page). Per-screen format: **What · Reads/Writes · Who · Keep/change/drop · AI & automation.**

### Launch Pad (`/`)
- **What:** app switcher — SB Production (CRM), Stitser Properties (Phase 2), links out (Gemini, NotebookLM, Sage Intacct). 🔎
- **Reads/Writes:** none (nav). **Who:** all internal. **Keep/change/drop:** keep → becomes the authenticated, role-based landing.
- **AI/Automation:** *proposed* — a one-line "what needs you today" digest (from the Feed) on the landing.

### CRM Home — Projects · People · Companies · Formation (`/sb-crm-home`)
- **What:** tabbed lists with filters (SB Company · Status · Department · Project Type), New Project, an **"Ask AI"** button, the Account Pyramid tab; left-nav to My Responsibilities, The Game, Time Card, Accounting, Knowledge Base, Credit Desk, Claude's Activity. 🔎
- **Reads/Writes:** reads Projects/People/Companies; New Project writes. Columns carry Intacct Project / Sage Job ID (accounting link). **Who:** all internal (RLS-scoped). **Keep/change/drop:** keep; "Use Filters to Work" → saved views per role.
- **AI/Automation:** *proposed* — "Ask AI" = natural-language filter/query ("show my at-risk WIP projects"); auto-surface stale / overdue / GYR-red rows; one-tap "start a project from a blueprint" (§2.5 Catalog).

### Project detail + facets (`/sb-crm-projects-list-details`)
- **What:** the hub. Header (SB Company · Status · Priority · Department · Property Record · Project Type · Property/Agency Contract) + actions (Link to Property, Create Drive Folders) + facet tabs — **Details · Decisions/Ratings · Reporting/Planning · Team · Project Drive · Schedule · Tasks/Checklists · Budget(s) & Pay App(s) · Project CRM** — + Related (parent/child) + Comments. 🔎
- **Reads/Writes:** the project + all linked facets. **Who:** Production Staff via the Stakeholder Bridge; Management via entity link (§3 Access). **Keep/change/drop:** keep; supports multi-discipline (multiple 702s/checklists) on one parent-child schedule (§2.5, §3).
  - *Decisions/Ratings* = go/no-go scoring (Strategic Fit → Overall → Final Decision) = prioritization.
  - *Budget(s) & Pay App(s)* = G-702/G-703 → Pay Apps → invoices (parent/child splits) → per-invoice audit → pay-app print → Sr-Mgt sign-off → AP.
- **AI/Automation:** *proposed* — the **Task-Level AI Assistant** ("person in the room": search Gmail/sources by project people/vendors → status → draft/send email); auto-draft the GYR/status report from recent comments; auto-populate baseline budget/schedule/checklists from a Category blueprint; invoice-compliance auto-check; "brief me on this project" from the full record.

### The Game (`/the-game-homepage`)
- **What:** Company Goals (Purpose · Metric · Target · Progress · GYR · Date) → **Situations** (value-chain fronts); second tab: Measurable Goals → Priorities. 🔎
- **Reads/Writes:** goals/priorities/situations; progress rolls up from projects. **Who:** Management + owners. **Keep/change/drop:** keep — the strategic scoreboard (§2.5 Pillar A).
- **AI/Automation:** *proposed* — auto-roll-up progress from contributing projects (contribution tag); Feed card when a Goal flips GYR or a target goes at-risk; "what's the one next move on this Situation?"

### My Responsibilities / Working List (`/sb-my-responsibilities`) — ≡ TSW Horizon
- **What:** the per-user working list — owned Goals · Priorities/Milestones · My Tasks (strategic) · My Project Tasks (checklists + meeting follow-ups) · Notes/Comments you're tagged in · GYR follow-ups. 🔎
- **Reads/Writes:** the one universal Task table + notes + GYR, filtered to the user. **Who:** every user (their own). **Keep/change/drop:** keep — the daily-driver view (a *feature*, not an entity).
- **AI/Automation:** *proposed* — Daily Horizon Scan (surface what's alive, propose the Big 3 / Hit List); auto-triage + snooze suggestions; draft replies to tagged comments.

### Account / Authority Pyramid — biz-dev mini-app
- **What:** relationship-development tool — tiers **Channel Account → Referral Partner → Top-50/Newspaper** + an **Audience-Health target** ("Authority Lock"). Tiers driven by status-as-customer / referral count / tag. 🔎
- **Reads/Writes:** People (tiers/tags), interaction dates. **Who:** biz-dev + Management. **Keep/change/drop:** keep as an **Execution Tool** mini-app (§2.5 Pillar D).
- **AI/Automation:** *proposed* — overdue / never-contacted outreach queue with daily targets (TSW "Stay in Flow" pull); draft the outreach message / newspaper entry; auto-tier from CRM signals.

### Time Card (`/sb-time-card-form`)
- **What:** time reporting — Cost Code · Dept Code · Project · Customer · Hours, with a 160-hr/mo coverage check + calendar view. 🔎
- **Reads/Writes:** writes time entries; dimensions from the mirrored accounting reference tables (§3). **Who:** all staff (own); Dept Heads review. **Keep/change/drop:** keep — feeds job-costing + interco allocation.
- **AI/Automation:** *proposed* — auto-suggest entries from calendar/activity; infer cost-code/dept/customer from the project; nudge when the month is under 160h.

> **Method note:** Clint layers in his own per-screen write-ups — especially the **New features / automations / AI integrations** section on each. Claude translates them into precise §4 entries + success criteria. Phase-2 screens (brokerage, credit desk) captured when Phase 2 opens.

## 5. User Workflows
⏳ Not Started — incl. the migration's **bidirectional mirror** (two-way sync SmartSuite ↔ Supabase during transition; not single-source-of-truth — Clint, 2026-06-29) and the **feedback-as-triage** support model (answer / route-to-training / accept-as-fix) + MarketingSecrets-style onboarding.

## 6. Out of Scope
🔄 In Progress.
- **Brokerage** (Agency/Property Contracts, contract stages, ACH match) and **Credit Desk** (loans) — **deferred to Phase 2 (§9)**; out of scope for the Phase-1 build. Their entities remain defined in §3.
- **Accounting structure** — family-trust QB + possible Intacct→QB consolidation — a deferred input, not Phase 1.

## 7. Success Metrics
⏳ Not Started

## 8. Technical Constraints / Assumptions
⏳ Not Started — Supabase (Postgres + Auth + Storage), Railway host, Next.js 16/React 19, Claude via remote CRUD MCP (planned).

## 9. Timeline / Phases
🔄 In Progress. *(Phasing decided 2026-06-30.)*
- **Phase 1 — People · Companies · Project Execution Framework.** The shared backbone + the Project hub and its facets (Tasks/Checklists, Budgets & Pay Apps G-702/G-703, Schedule, Decisions/Ratings, Team, Notes/Comments, GYR, Project Drive, Project CRM), plus the cross-cutting operator surfaces: **My Responsibilities / Working List, The Game, Time Card, Account/Authority Pyramid.** This is what most internal roles touch daily.
- **Phase 2 — Brokerage + Credit Desk.** Agency Contracts → Property Contracts → Close of Escrow (contract stages, ACH match) and the Loans / Credit Desk ledger.
- **Note:** phasing is a **build-sequencing** decision — Contracts and Loans are already *defined* in Core Entities (§3), so Phase 1 doesn't foreclose them; only the build is deferred.
- **Rollout (both phases):** Clint → +testers → company, per-module cutover behind the bidirectional mirror (§5).

## 10. Open Questions / Decisions Needed
⏳ Not Started — automation rebuild approach (103 captured via screenshots, intact); remote CRUD MCP host (Supabase Edge vs Railway); cutover sequencing.
