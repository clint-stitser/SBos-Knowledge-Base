# Memory: S-BOS

> **Purpose:** Running log of key facts, decisions, and context. Read at the start of every session.
> **Update:** Whenever a decision is made or context shifts. Rewritten at session end. Clint reviews.

---

## Project Identity

- **Project Name:** S-BOS (Stitser Business Operating System)
- **App Description:** A **universal operator shell** — domain-structured operating system + trained assistant — replacing SmartSuite + Softr with a proprietary stack. S-BOS (internal) and the four Kompass verticals (Developer/Contractor/Agent/**Personal=TSW**) are **configurations of one shell**, isolated per `org_id` (licensable/franchisable).
- **Goal (V1):** Biz Dev CRM module fully live on the new stack at parity with Softr; then migrate remaining modules.
- **Philosophy:** Claude-serviceable, expansion-ready, no vendor lock-in, **parallel-run via a bidirectional mirror** (no whole-company big-bang). Calibrated honesty over confident answers.
- **Current Phase:** Migration in progress (POC live). **PDD: Gate 1 + Gate 2 PASSED (2026-06-29) — Core Entities locked for now; Core Features (§4) next.**

---

## Tech Stack

| Layer | Technology | Notes |
|---|---|---|
| Frontend | Next.js 16 / React 19 / TypeScript / Tailwind v4 | App Router, server actions |
| Backend | Next.js Server Actions | Secret key server-side only |
| Database | Supabase (Postgres) | Portable; RLS + pgvector (for Knowledge Library) |
| Auth | Supabase Auth (Google + magic link) | Planned — invite/roles/RLS; `org_id` multi-tenant |
| Storage | Supabase Storage | Public bucket for tracker screenshots |
| Hosting | Railway | `sb-crm-poc-production.up.railway.app` |
| Claude DB access | `scripts/db.mjs` (pg pooler) | Direct SQL; no dashboard |

---

## Decisions (design-phase / cross-cutting)

- **Supabase over SmartSuite** (API limits + Claude-serviceability). Railway hosts the app; Supabase = data/auth/storage. **Supabase Auth over Cloudflare Access** (ties to RLS).
- **Invite/allowlist + roles + RLS** access model; `org_id` multi-tenant instances for licensing/franchise. Recovery: soft-delete + audit-log + PITR.
- **Formulas → Postgres views/computed columns.** **Automations NOT API-extractable** (103 across 8 solutions; screenshot-captured, rebuilt as triggers/Edge Functions).
- **Access model:** internal **C·R·U + audit + 60-day restore**; **delete = admin-only**; external **view-only scoped**.
- **CRM is the platform's shared backbone** — People/Companies are **polymorphic (contextual roles, not fixed types)**.
- **Project Prioritization = a Feature** (decision-gates), not a migrated table (2026-06-28).
- **Remote CRUD MCP (PLANNED)** — remote HTTPS MCP so claude.ai/iOS can CRUD Supabase; enforces the permission model server-side. Host TBD (Edge vs Railway).

### New decisions — 2026-06-28/29 (Vision back-fill + consolidation)
- **Universal shell.** One codebase; S-BOS + the four Kompass verticals are **configurations** isolated by `org_id`. **TSW (The Stitser Way) = the Personal Kompass config — not a separate app.** Its frameworks (Capture, Today/Day-Mode, Horizon, dashboards = universal; Journals/Rituals, GYR Spiral, Scoreboard, **Habit** = personal modules) carry into the shell. **Removed: "Kevin's Rule" + "Misogi" (confusing). Habit building kept.**
- **Execution/Org model:** `Entity → Category → Project` (Project carries People·Tasks·Budget·Schedule·Outcome). **Two tracks** — Strategic (Goal→Priority/Sprint→Milestone) + Operational (Project→atoms) — joined by **roll-up = contribution tag, not parent-child**. `project_type` + `department` are the current roll-up dims.
- **Master Property = a Brain-layer persistent anchor** (confirmed). A parcel/asset outlives any single project; many projects anchor to one Property over time; the Property accumulates full longitudinal history (projects/tasks/comments). Only its dead "Prioritization Tool" link was dropped.
- **The Brain — six intelligence stores.** ✅ exist: People Profiles, Decision Matrices, Decision Log. 🔄 build: Vendor Ratings (reviews→Companies), **Knowledge Library** (scoped Postgres + RLS + pgvector, scoped by entity/category/skill/vertical, multi-user). Project Data = built substrate. Master Property lives here.
- **Kompass operator assistant + the Feed.** Domain-trained CoS with Brain access + skill execution + context triggers; the **Feed** is the human-in-the-loop outbox (Approve/Edit/Skip/Defer; full run history = audit log). **First surface = the Task-Level AI Assistant** ("person in the room": Gmail-MCP search by project people/vendors → status → draft/send email on approval → checks notes/comments → shared project brain).
- **Execution Tools** = lightweight purpose-built apps (budget builder, study app, pay-app runner, scorecards) distinct from skills (LLM playbooks); read/write the Brain; part of the licensable "Software" layer.
- **Blueprint / Template Catalog** = a Brain-layer store (sibling to Knowledge Library; reference vs. structure). Holds activatable bundles (task lists, budget items, schedule durations, roles, tool refs, skill refs), **anchored at the Category level**, scoped by category/vertical (RLS). **Activation = instantiation** onto a Project. Grounded in the **CrossMod land-dev stage-gate example** (G0–G5; each gate = entry/exit + decision questions + team/AHJs + tasks + cost items + durations + if-then branches + jurisdiction overlays).
- **Migration = bidirectional mirror, NOT single-source-of-truth.** The system is too intertwined for one-way; both DBs stay current during transition via two-way sync (ID pairing, field-level last-write-wins, sync ledger, loop prevention). Per-module cutover; rollout Clint → +testers → company. Whole-company July-4 big-bang rejected (parity not there); CRM-module forcing-function is the candidate.
- **Build consolidated into the Claude Code chat** (2026-06-29). `sb-crm-poc` worked **in place** at `/Users/clintstitseroffice/Documents/sb-crm-poc`; the other claude.ai chat is paused (single write surface).
- **Recovery (2026-06-29):** the morning incident did NOT lose automation captures — 4 documented/in-progress + 9 screenshots, 0 orphans (verified vs live Supabase). Storage objects survive rewrites.
- **Accounting (deferred):** family-trust accounting → QuickBooks (departments + projects); business on Intacct (inaccessible now); possible Intacct→QB multi-company consolidation. Accounting rides the same Entity→Category→Project tree (Entity=GL company, Category=class/product line, Project=job, Budget atom from QB/Intacct). Not Phase 1. *(Note: the Conrad Stitser Family Trust is just a lender Company, NOT the family-trust-accounting trust referred to here.)*

### Core Entities + live-app research — 2026-06-29 (drives PDD §3; Gate 2 next)
- **Live-app research** (`app.stitserbuilt.com`): two Softr apps surface ONE DB per vertical/role — **SB Production** (`/sb-crm-home`: dev/construction; Projects/People/Companies, the Game, Time Card, Accounting, Credit Desk) and **Stitser Properties** (`/sp-home`: brokerage; Agency→Property Contracts, contract stages, ACH match, its own Game homepage). **Decision: merge SP into the same shell/tables** — brokerage is a Project *type/Category* (different facets/checklists/budget items), not a separate app.
- **Three drivers = People · Companies · Projects.** **Companies are polymorphic across 4 financial-statement roles — Revenue(customer)/Cost(vendor)/Lender/Equity —** and hold multiple accounting IDs at once (vendor/customer/location); roles contextual per project/department, never hard-typed.
- **Master Property = a LINK/lens, not a parent.** Address/parcel (lots are Properties too); Projects/Notes/Contracts/Loans link to it; querying an address returns all linked history (the 2030-"123 Main St" value story).
- **Project = the hub** with facets (Details, Decisions/Ratings, Reporting/Planning, Team, Project Drive, Schedule, Tasks/Checklists, Budgets & Pay Apps, Project CRM) + Parent/Child + comments.
- **Multi-discipline projects:** one Project can run **land-dev + construction + brokerage in parallel**, each discipline with its own **Pillars, Blueprint+mini-apps/catalogs, G-702, and QC/required-doc checklist & audit cycles** → a Project can have **multiple 702s and multiple checklist/audit cycles**, unified by **one Schedule with parent-child views**. Applies to the **platform operator** (multi-discipline); single-vertical tenants don't need it (config-gated). A multi-discipline Project activates *multiple* blueprints (one per discipline).
- **Loans (Credit Desk)** = first-class: Lender/Borrower (Companies), Rate, Principal, Due/Maturity, Status; secured (Project/Property-linked) or operating (Financing dept).
- **Time Cards** dims: Cost Code · Dept Code · Project ID · Customer ID · Account Code · Hours (direct → project; overhead → dept + customer served).
- **Accounting reference layer:** Cost/Dept/Account codes + entity IDs live in reference tables **mirrored from the accounting system (Intacct now, QB future) = single source of truth.** Budgets/Time/Financials all draw from it.
- **The Game (Strategic):** Company Goal (Purpose/Metric/Target/Progress/GYR/Date) → **Situations** (operational pipeline fronts: Biz Dev→Underwriting→Construction→Customer Service + strategic) → **Strategic Priorities** (target a stat in a Situation by a date). Roll-up/contribution (Projects→Goals/Priorities mirrors Priorities→Situations).
- **Phase gating → control tools evolve estimate→baseline** (discovery/refinement → "game on"/commitment); Blueprints make estimates accurate-early + estimate→baseline fast (added to PDD Pillar A).
- **More research (My Responsibilities / Account Pyramid / Decisions-Ratings / Budget):**
  - **My Responsibilities = S-BOS twin of TSW Horizon** — per-user working list (Goals/Priorities/Milestones + universal Tasks + Notes follow-ups + GYR follow-ups). A *feature/view* → PDD §4.
  - **Account = Authority Pyramid:** People relationship tiers (Channel Account → Referral Partner → Top-50/Newspaper), **driven by status-as-customer, referral count, or a tag** (not hand-sorted). The Pyramid is a **mini-app (Execution Tool, Pillar D) for biz-dev efficiency & context** — surfaces relationships/tiers/cadence + an Audience-Health target ("Authority Lock"). Tiers/tags = People data; the Pyramid = the tool. Ties to TSW Stay-in-Flow/PN outreach.
  - **One universal Task table:** the table feeding Goals/Priorities = the same as project/checklist/meeting tasks. Multiple SmartSuite task tables **consolidate into one robust Task table** (typed/linked, not split).
  - **Decision Gates / Ratings** (Project facet) = the prioritization engine: per-gate ratings (Strategic Fit · Market/Product Fit · Financial Viability · Constructability · Jurisdictional & Legal · Operational Capacity → Overall Assessment → Final Decision) → "Project Prioritization = a feature."
  - **Budgets & Pay Apps** (schema + pay-app skills; live tab wouldn't render): G-702 (App-for-Payment summary; multiple per multi-discipline project) → G-703 (schedule of values by cost code; estimate→baseline, baseline edits change-order-gated) → Pay Apps (PA-N, retention release, lock) → Bills & Invoices (parent/child splits) → per-invoice PM audit/compliance checklist → pay-app print → Senior Mgt sign-off → AP approval (PA Drive package: G-702/G-5 + NRS lien waiver). Change Orders→CO Line Items feed 702/703.
- **Access model — RESOLVED 2026-06-29 (in PDD §3):** Groups = Customer/Investor/Vendor/Production Staff/Production Manager/Department Head/Senior Management (+ System Admin). RLS-enforced. Highlights: Meetings = invitees OR Sr Mgt (project meetings = all participants); private user items (tasks/journals/notes not linked to shared entity) visible to owner; CRU on Project/People/Companies + supporting tables → **Management driven by entity link, Production Staff driven by the Project Stakeholder Bridge**; baseline edits (CO to schedule/budget/scope) need Sr PM / Construction Dept Head approval; invoices/project budgets/AR-AP/project financials → Production Staff; department financials → Dept Heads + Sr Mgt; entity financials (loans, time cards, entity P&L) → Accounting Dept Head + Sr Mgt; admin (fix/structure/delete/restore) → System Admin; dashboards → per dashboard-level selection.

---

## Conventions Established

- **`scripts/db.mjs`** = single entry point for DB changes (`migrate`, `sql`, `query`, `migrate-all`).
- **Migrations** in `sb-crm-poc/supabase/migrations/` (001–011).
- Writes via Next.js server actions (Supabase secret key, never browser). PII gitignored, regenerable from SmartSuite. Secrets in `.env.local`.
- **Docs separate from code:** build docs in `SBos-Knowledge-Base/projects/s-bos/build-docs/` (read/write via GitHub); code in `sb-crm-poc`.
- **Vision summaries reference the canonical platform docs** (`projects/kompass/platform/`) rather than re-paraphrasing them — one source of truth.

---

## Existing Artifacts (inputs to back-fill from)

- **Code:** `sb-crm-poc` (live CRM POC + Automation Tracker + Migration Menu).
- **Planning docs:** `sb-crm-poc/docs/` (atlas/, auth-plan, recovery-and-restore, formula-audit) + `kompass/docs/biz-dev-crm-poc-spec.md`.
- **Canonical platform docs:** `kompass/platform/platform-pdd.md` + `shared/` (brain-model, intelligence-stores, operator-assistant, feed-model, …) + `research/marketingsecrets-analysis.md`.
- **Example blueprint:** CrossMod land-dev stage-gate framework (G0–G5) — Clint's draft for the Entry-Level Housing Category.
- **System Atlas:** Chapter 1 (9 Biz Dev CRM tables) complete.

---

## Open Items

- [ ] Core Entities (PDD §3) — back-fill from schema + §2.5 Vision.
- [ ] Capability walkthrough of `app.stitserbuilt.com` (live input).
- [ ] Bidirectional-mirror sync spec (Workflows section).
- [ ] Strip Kevin's Rule + Misogi from TSW app files.
- [ ] Automation rebuild approach; remote CRUD MCP host; cutover sequencing.

---

## Notes

- Decision-maker is **Clint**. Ryan (CS friend) authored the build-doc methodology.
- In-browser automation tool has been flaky — verify via `db.mjs` + server-rendered HTML.
- Work style: small sprints; deep dives when there's space; async phone review.
