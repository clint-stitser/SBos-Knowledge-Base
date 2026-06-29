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
| Core Entities | 🔄 In Progress | Next — back-fill from live Supabase schema + the Vision below |
| 🚦 Gate 2 | ⏳ Not Started | |
| Core Features | ⏳ Not Started | |
| User Workflows | ⏳ Not Started | |
| 🚦 Gate 3 | ⏳ Not Started | |
| Out of Scope · Success Metrics · Tech Constraints · Timeline · Open Questions | ⏳ Not Started | |
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

### The TSW module catalog (carried into the shell)
TSW's frameworks become **modules of the universal shell** — *universal* (Capture, Today/Day-Mode, Horizon rings, dashboards-by-Category/Department/product-line) and *personal-config* (Journals/Rituals, GYR Spiral, Scoreboard, **Habit building**), whose patterns generalize to any operator's reflective practice. **Removed per Clint 2026-06-29: "Kevin's Rule" and "Misogi" (confusing).** Habit building stays.

> 🔎 Items in §2.5 trace to Clint's session decisions (2026-06-28/29) + the canonical platform docs; confirm/adjust as features deepen.

---

## 3. Core Entities
🔄 **In Progress (next).** Back-fill from the live Supabase schema (9 CRM tables + junctions) **plus the Vision above.** Must model:
- **Polymorphic roles** — People/Companies relate to Projects via role-bearing relationships, not hard types.
- **Master Property** — persistent anchor; Project → Property (many projects per property over time).
- **Category** — the blueprint host + a roll-up dimension; carries default task-list/budget/schedule/role/tool/skill bundles.
- **Two-track + roll-up** — Strategic (Goal→Priority/Sprint→Milestone) and Operational (Project→atoms), joined by a contribution tag.
- **`org_id`** — multi-tenant instance key on every row (expansion/licensing).

## 4. Core Features
⏳ Not Started.
> **Pre-filled:** **Project Prioritization** = a *feature* via decision-gates (the legacy "Project Prioritization Tool" table is **not** migrated). *(Decision 2026-06-28.)*

## 5. User Workflows
⏳ Not Started — incl. the migration's **bidirectional mirror** (two-way sync SmartSuite ↔ Supabase during transition; not single-source-of-truth — Clint, 2026-06-29) and the **feedback-as-triage** support model (answer / route-to-training / accept-as-fix) + MarketingSecrets-style onboarding.

## 6. Out of Scope
⏳ Not Started. *(Accounting structure — family-trust QB + possible Intacct→QB consolidation — is a deferred input, not Phase 1.)*

## 7. Success Metrics
⏳ Not Started

## 8. Technical Constraints / Assumptions
⏳ Not Started — Supabase (Postgres + Auth + Storage), Railway host, Next.js 16/React 19, Claude via remote CRUD MCP (planned).

## 9. Timeline / Phases
⏳ Not Started — Phase 1 = Biz Dev CRM module; later phases per the 27-solution migration. Rollout: Clint → +testers → company (per-module cutover behind the bidirectional mirror).

## 10. Open Questions / Decisions Needed
⏳ Not Started — automation rebuild approach (103 captured via screenshots, intact); remote CRUD MCP host (Supabase Edge vs Railway); cutover sequencing.
