# Restart: S-BOS

> **Purpose:** Capture exactly where we stopped so the next session picks up without re-explaining.
> **Update:** Before ending every session. Overwrite entirely — current stopping point only.

---

## Session Info

- **Date:** 2026-06-17
- **Project:** S-BOS
- **Status:** In Progress — PDD at Gate 1

---

## What We Were Doing

Back-filling the **Product Design Doc** (`S-BOS_Product_Design_Doc.md`), Ryan's first design doc. Scope confirmed: **whole S-BOS platform, with the Biz Dev CRM module as Phase 1.** Also built the roadmap integration (App Items + per-Kind doc checklists — see below).

---

## Where We Stopped

PDD **Sections 1–2 drafted and Gate 1 checklist passed** — awaiting Clint's explicit sign-off:
- **Problem Statement** ✅ — SmartSuite API limits + no-code bottleneck (only Clint changes structure) + no licensing path.
- **Target Users** ✅ — Internal Staff (all roles) = Phase-1 primary; the **CRM is the platform's shared backbone** (People/Companies are polymorphic: customer/vendor/staff/investor-lender — roles are *contextual, not fixed types* → drives Core Entities). Clint = admin/builder; external + franchisees = future.
- **Access model captured:** internal **CRU + 60-day audit/restore**, **delete = admin-only**, external **view-only on scoped elements**. (Updates recovery-plan window 30→60; refines auth roles.)

Everything is on GitHub (`SBos-Knowledge-Base/projects/s-bos/build-docs/`).

---

## Next Steps (in order)

1. [ ] **Gate 1 sign-off** — Clint approves Problem + Users (checklist already passes in the PDD). This is the immediate next action.
2. [ ] **Core Entities (PDD §3)** — back-fill from the live Supabase schema (9 CRM tables + junctions). Model the **polymorphic-role** insight: People/Companies relate to projects via *role-bearing relationships*, not hard types.
3. [ ] Core Features → User Workflows (Gate 3) → Scope/Metrics/Timeline/Open Questions (Gate 4).
4. [ ] Then DB Schema, Technical Spec, Decisions Log (ADRs from decisions already made — see memory.md).

---

## Open Questions / Decisions Pending

- Automation rebuild approach (103 automations captured via screenshots; not API-extractable).
- Remote CRUD MCP host: Supabase Edge Functions vs Railway (decide at build time).
- *(Resolved: PDD scope = platform + CRM Phase 1. Resolved: build modules now represented as App Items on the v2.4 roadmap.)*

---

## Environment Notes

- **GitHub connector is live** in claude.ai (2026-06-16) — sessions read build-docs directly from the repo.
- **Two write surfaces** (Claude Code + claude.ai/iPhone) → **sync before edit, one surface per file.** The roadmap app (`sb-planning-tools` repo: `roadmap/index.html`, `server.js`) is **claude.ai-owned** — don't edit from Claude Code without syncing.
- **Roadmap integration done:** the 6 build modules now exist as **App Item Project records** (IT/Systems dept) on the v2.4 roadmap, each with a **Build Docs checklist** of per-Kind required-doc tasks. See `S-BOS_App_Item_Doc_Requirements.md` for the Kind→docs mapping.

---

## Current File Status

> Lives in `S-BOS_Design_Context.md` → File Inventory. PDD = 🔄 In Progress (Gate 1 awaiting sign-off); all other design docs ⏳ Not Started.

---

## How to Resume

1. Read `S-BOS_Operating_Agreement.md`, then `memory.md`, `restart.md`, `S-BOS_Design_Context.md`.
2. Open `S-BOS_Product_Design_Doc.md` to the Gate 1 block.
3. Say "let's go" — first action is **Gate 1 sign-off**, then Core Entities.
