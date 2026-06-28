# Product Design Doc: S-BOS

> **Scope:** The S-BOS platform vision, with the **Biz Dev CRM module as Phase 1**.
> **Method:** Back-filled mid-stream from the running POC + planning docs. Items marked 🔎 are *reverse-engineered* from the running system and need Clint's confirmation; items marked ❓ are open gaps.

---

## Status & Next Steps

| Section | Status | Notes |
|---------|--------|-------|
| Problem Statement | 🔄 In Progress | Drafted — awaiting Gate 1 sign-off |
| Target Users / Personas | 🔄 In Progress | Personas + access model done — awaiting Gate 1 sign-off |
| 🚦 Gate 1 | ❓ Needs Discussion | Checklist passed — awaiting Clint sign-off |
| Core Entities | ⏳ Not Started | (back-fill from live schema) |
| 🚦 Gate 2 | ⏳ Not Started | |
| Core Features | ⏳ Not Started | |
| User Workflows | ⏳ Not Started | |
| 🚦 Gate 3 | ⏳ Not Started | |
| Out of Scope | ⏳ Not Started | |
| Success Metrics | ⏳ Not Started | |
| Technical Constraints / Assumptions | ⏳ Not Started | |
| Timeline / Phases | ⏳ Not Started | |
| Open Questions | ⏳ Not Started | |
| 🚦 Gate 4 | ⏳ Not Started | |

**Status scheme:** ⏳ Not Started → 🔄 In Progress → ❓ Needs Discussion → ✅ Done

---

## 1. Problem Statement

**Problem:** Stitser BUILT runs its operations on SmartSuite (database) + Softr (user portal). Three structural problems with that stack:
1. **SmartSuite API limits** throttle how much Claude can read/write — and that ceiling was hit early, capping Claude's usefulness as the system grows.
2. **The no-code tooling makes Clint the bottleneck** — schema, formulas, and automations can only be changed by hand in SmartSuite's UI, so the system can't be serviced/extended by Claude without a human in the loop.
3. **It can't be owned, licensed, or franchised** — building on two third-party SaaS products means no path to productize the system as a proprietary offering.

**Affected users:**
- 🔎 **Clint (builder/admin)** — the person who builds and maintains the structure; currently the single point of change for all schema/formula/automation work.
- 🔎 **The internal team** — biz dev, PMs, accounting; they consume and update data through Softr and are unaffected by *how* it's built, but depend on it working.

**Consequence:** API limits cap automation and AI assistance; every structural change funnels through Clint; ongoing dependence on two vendors' constraints and pricing; and no route to license/franchise the system as IP.

**Current workaround:** SmartSuite (DB + no-code builder) → Softr (portal, iframing Railway-built dashboards with per-user permissions) → Claude via SmartSuite MCP for data work. Insufficient because of the API ceiling, the human-in-the-loop bottleneck, and the lack of ownership.

> 🔎 *Reverse-engineered from Clint's stated drivers across the migration effort. Confirm this captures the real problem.*

---

## 2. Target Users / Personas

> **The CRM is the platform's shared backbone — not a biz-dev silo.** People and Companies touch *every phase of every project*, filling the roles of **customer, vendor, internal staff, and investor/lender** depending on context. So **all internal roles are primary users** of the CRM, and an entity's "role" is **contextual, not a fixed type**. → This polymorphic-role principle drives **Core Entities (Section 3)**: People and Companies are related to projects through *role-bearing relationships*, not hard-typed.

### Persona: Internal Staff (all roles) — **Phase 1 primary**
Covers Relationship Managers / Biz Dev, Project Managers, Accounting, and Execs. They all connect to and depend on the CRM because people/companies span customer, vendor, staff, and investor/lender roles across the org.
- **Type:** Primary
- **Primary goal:** Manage relationships and project progress — find people/companies/projects, keep history current, and act on their piece of the work.
- **Most common action:** Working a list of records, opening a record, and **filing notes/comments with follow-ups**.
- **Key constraint / frustration:** Must not be disrupted by the migration — the new portal has to feel exactly like Softr. Works fast through lists.
- **Technical comfort:** 🔎 Medium — comfortable in the portal, not technical. *(Confirm.)*

### Persona: Clint (Admin / Builder) — **secondary, system-wide**
- **Type:** Secondary
- **Primary goal:** Build, service, and extend the system — schema, formulas, automations, data — **through Claude**, without hand-editing a no-code UI.
- **Most common action:** Directing Claude to make schema/data/automation changes; reviewing docs and the running app.
- **Key constraint / frustration:** Today every structural change is manual and only he can do it. Wants Claude to own that loop.
- **Technical comfort:** High (directs the build; relies on Claude rather than hand-coding).

### Persona: External Stakeholder (investor / broker / client) — **future (Auth Phase B)**
- **Type:** Secondary / future
- **Primary goal:** See only their own relevant records (their deals, their documents).
- **Most common action:** 🔎 Read-mostly access to shared records. *(Define when we reach Phase B.)*
- **Key constraint:** Comes in via a different login path (magic link), sees only explicitly-shared data.
- **Technical comfort:** Low–Medium.

### Persona: Franchisee / Licensee — **future (Auth Phase C)**
- **Type:** Future
- **Primary goal:** Run their own isolated instance of S-BOS for their business.
- **Most common action:** Same as the internal team, scoped to their own org/tenant.
- **Technical comfort:** Varies.

### Access & Permissions Model (confirmed)

This is a Phase-1 design input that flows into the Auth plan and the Recovery plan.

| Audience | Access | Notes |
|---|---|---|
| **Internal users (all roles)** | **C·R·U** (Create, Read, Update) | + **audit trail** on changes and **restore available for 60 days**. No hard delete. |
| **System admin (Clint)** | CRU + **Delete** | Delete is admin-only. |
| **External stakeholders** (investors, lenders, vendors, …) | **View-only**, scoped | Read access to **certain elements only** — defined per audience when we reach Auth Phase B. |

> **Cross-doc implications (to reconcile):**
> - **Auth plan** (`sb-crm-poc/docs/auth-plan.md`): roles become `admin` (CRUD) / `internal` (CRU + restore) / external view-only roles scoped to shared elements.
> - **Recovery plan** (`sb-crm-poc/docs/recovery-and-restore-plan.md`): soft-delete restore window = **60 days** (updates the earlier 30-day default), restore available to all internal users; **hard delete admin-only**; audit log applies to all CRU actions.

---

## 🚦 Gate 1 — Problem + Users Sign-Off

**Checklist:**
- [x] Problem Statement describes a problem, not a solution
- [x] Current workaround is documented
- [x] Consequence of not solving is documented
- [x] All named user types have a goal, a primary action, and a key constraint
- [x] Primary persona is identified (Internal Staff, all roles)
- [x] No two personas have directly conflicting needs without a resolution noted

**Sign-off:**
> 🚦 **Gate 1** — Problem Statement and Target Users are complete and internally consistent. Ready to define Core Entities.
>
> **Clint sign-off:** ☐ Approved — proceed to Core Entities

---

## 3. Core Entities
⏳ Not Started — will back-fill from the live Supabase schema (9 CRM tables + junctions already exist).

## 4. Core Features
⏳ Not Started

> **Captured during migration (pre-fill — to formalize when this section is worked):**
> - **Project Prioritization** — a *feature*, not a table. Delivered through the **decision-gates** system (stage-based checklists that determine what advances/prioritizes at each stage). The legacy "Project Prioritization Tool" table (SB UW & Estimating solution) is **not migrated** — built long ago, never launched. The Master Property → Project Prioritization link is dropped (marked `x-`). *(Decision: 2026-06-28.)*

## 5. User Workflows
⏳ Not Started

## 6. Out of Scope
⏳ Not Started

## 7. Success Metrics
⏳ Not Started

## 8. Technical Constraints / Assumptions
⏳ Not Started

## 9. Timeline / Phases
⏳ Not Started — Phase 1 = Biz Dev CRM module; later phases per the 27-solution migration.

## 10. Open Questions / Decisions Needed
⏳ Not Started
