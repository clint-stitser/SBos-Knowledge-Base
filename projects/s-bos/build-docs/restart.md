# Restart: S-BOS

> **Purpose:** Capture exactly where we stopped so the next session picks up without re-explaining.
> **Update:** Before ending every session. Overwrite entirely — current stopping point only.

---

## Session Info

- **Date:** 2026-06-30
- **Project:** S-BOS
- **Status:** In Progress — Gates 1 & 2 PASSED; §2.5 + §3 locked. **§4 Core Features started (screen-by-screen back-fill); §6/§9 Phase 1/2 split set.** Next: Clint layers his per-screen write-ups (+ AI/automation per page) onto §4.

---

## What We Were Doing

Consolidated the two efforts (the SmartSuite/TSW work + the Supabase `sb-crm-poc` build) into **one Claude Code chat** (the other chat is paused). Back-filled the PDD's **Product Vision & Architecture (§2.5)** from a long design conversation, and **Clint signed off Gate 1.**

---

## Where We Stopped

- **Gate 1 ✅ APPROVED (Clint, 2026-06-29).** Problem + Target Users done.
- **PDD §2.5 Product Vision & Architecture written** (🔄, deepens as features develop): the **universal shell** (one codebase, configured per `org_id`; TSW = Personal Kompass config), four pillars — (A) Execution/Org incl. **Master Property** persistent anchor + two-track roll-up, (B) the **Brain** (6 stores; 3 exist, Vendor Ratings + Knowledge Library to build), (C) the **Kompass assistant + Feed** (first surface = Task-Level AI Assistant), (D) **Execution Tools** — plus the **Blueprint/Template Catalog** (Category-anchored, activatable; grounded in the CrossMod land-dev stage-gate example) and the **TSW module catalog** (Kevin's Rule + Misogi removed; habit kept).
- **Recovery resolved:** the morning incident did NOT lose automation captures — 4 documented/in-progress + 9 screenshots intact, 0 orphans (verified against live Supabase).

---

## Next Steps (in order)

1. [ ] **§4 Core Features — layer in Clint's per-screen write-ups.** Claude pre-drafted each Phase-1 screen (Launch Pad, CRM Home, Project detail+facets, The Game, My Responsibilities, Account Pyramid, Time Card) with What/Reads-Writes/Who/Keep-change-drop + a **proposed AI/automation** line. Clint adds his own capture per screen, **especially the "New features / automations / AI integrations" section.** Then → User Workflows + Gate 3.
2. [ ] User Workflows: **bidirectional-mirror** sync spec + feedback-triage/onboarding support layer.
3. [ ] DB Schema — reconcile live `sb-crm-poc` schema to §3: add **Loans, Time Cards, Contracts, accounting reference tables, Decision Gates/Ratings, Property-as-link, Game entities, one universal Task table**; RLS policies for the Access model (entity-link + Stakeholder Bridge).
4. [ ] Tech Spec, Decisions Log (ADRs).

---

## Open Questions / Decisions Pending

- Automation rebuild approach (103 captured via screenshots — intact; not API-extractable).
- Remote CRUD MCP host: Supabase Edge Functions vs Railway (decide at build time).
- **Bidirectional sync mechanics** (ID pairing, field-level last-write-wins, sync ledger, loop prevention) — spec when Workflows is worked.
- **Cutover pace:** Clint wants to go faster than a cautious read; CRM-module forcing-function is the candidate (NOT whole-company big-bang). Gated on parity + the support layer.
- App cleanup: strip "Kevin's Rule" + "Misogi" from TSW app files (`03-stitser-way/messaging.md`, `web/README.md`, + sweep fixtures).
- Accounting structure (family-trust QB; Intacct→QB consolidation) — deferred input.

---

## Environment Notes

- **Build is now driven from the Claude Code chat**, working `sb-crm-poc` **in place** at `/Users/clintstitseroffice/Documents/sb-crm-poc` (the other claude.ai chat is paused — single write surface). `.env.local` has Supabase creds; `node scripts/db.mjs` + the Storage API both verified.
- Build docs: `SBos-Knowledge-Base/projects/s-bos/build-docs/` (read/write via GitHub). Code: `sb-crm-poc`.
- Roadmap app (`sb-planning-tools` repo) is claude.ai-owned — sync before editing.

---

## How to Resume

1. Read `S-BOS_Operating_Agreement.md`, then `memory.md`, `restart.md`, `S-BOS_Design_Context.md`.
2. Open `S-BOS_Product_Design_Doc.md` — §3 + Gate 2 ✅ locked; §4 Core Features is drafted screen-by-screen (Phase-1), §6/§9 carry the Phase 1/2 split.
3. Say "let's go" — first action is **layering Clint's per-screen write-ups (+ AI/automation) into §4**, then User Workflows + Gate 3.
