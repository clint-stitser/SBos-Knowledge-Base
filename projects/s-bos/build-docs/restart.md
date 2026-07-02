# Restart: S-BOS

> **Purpose:** Capture exactly where we stopped so the next session picks up without re-explaining.
> **Update:** Before ending every session. Overwrite entirely — current stopping point only.

---

## Session Info

- **Date:** 2026-07-02
- **Project:** S-BOS
- **Status:** In Progress — Gates 1 & 2 PASSED; §2.5 + §3 locked. §4 Core Features started (screen-by-screen back-fill) and still queued as Next Step below — **paused for today**, not reordered. Today's active work is governed by `S-BOS_Plan_2026-07-02-entry-level-housing-launch-day.md`: sequenced Entry-Level Housing Setup + AI Integration build (Layer 0 foundation → Layer 1 populate/activate → Layer 2 surface), plus a fully parallel Q2 Loan Reconciliation track that doesn't touch the app at all.

---

## What We Were Doing

Consolidated the two efforts (the SmartSuite/TSW work + the Supabase `sb-crm-poc` build) into **one Claude Code chat** (the other chat is paused). Back-filled the PDD's **Product Vision & Architecture (§2.5)** from a long design conversation, and **Clint signed off Gate 1.** This session then documented the Kompass Dispatch Architecture and the Category-Scoped One-Way Cutover pilot, and finally produced **today's execution plan** sequencing the actual build work across Entry-Level Housing Setup + AI Integration.

---

## Where We Stopped

- **Gate 1 ✅ APPROVED (Clint, 2026-06-29).** Problem + Target Users done.
- **PDD §2.5 Product Vision & Architecture written** (🔄, deepens as features develop): the **universal shell** (one codebase, configured per `org_id`; TSW = Personal Kompass config), four pillars — (A) Execution/Org incl. **Master Property** persistent anchor + two-track roll-up, (B) the **Brain** (6 stores; 3 exist, Vendor Ratings + Knowledge Library to build), (C) the **Kompass assistant + Feed** (first surface = Task-Level AI Assistant), (D) **Execution Tools** — plus the **Blueprint/Template Catalog** (Category-anchored, activatable; grounded in the CrossMod land-dev stage-gate example) and the **TSW module catalog** (Kevin's Rule + Misogi removed; habit kept).
- **Recovery resolved:** the morning incident did NOT lose automation captures — 4 documented/in-progress + 9 screenshots intact, 0 orphans (verified against live Supabase).
- **Kompass Dispatch Architecture documented** (`S-BOS_Discussion_kompass-dispatch-architecture.md`): Compass-owns-the-catalog / Claude-as-stateless-compute model; document-generation build approach (code execution + Files API + Skills API); Realtor Kompass multi-tenant/licensing decisions; the adopted skill review-gate workflow; a clarified data-residency answer.
- **Category-Scoped One-Way Cutover pilot decided and specced** (`S-BOS_Discussion_category-scoped-cutover.md`): Entry-Level Housing flagged `cutover_mode: supabase_native`; new projects live/edit only in the new app; Details/Schedule/Tasks/Team push one-way to SmartSuite via an ID-pairing table; Day-1 soft lock on the SmartSuite side. Budget/Pay App deferred entirely (workable because these are brand-new projects with no existing SmartSuite financial data — does not generalize beyond that condition).
- **NEW — Today's execution plan written** (`S-BOS_Plan_2026-07-02-entry-level-housing-launch-day.md`): sequences the full day's build — Layer 0 (generic foundation: Category+cutover flag, Blueprint Catalog schema, Knowledge Library schema, Project Schedule facet, Skill/Routine catalog + Dispatcher path, Select-Entities settings screen) → Layer 1 (populate Blueprint + Knowledge Library, wire live Anthropic API calls into the Dispatcher, activate the 4 chosen projects, run the cutover push+lockout) → Layer 2 (product-line dashboard, end-to-end surface-blend pass). Q2 Loan Reconciliation runs as a fully separate, parallel track (not a Supabase build task — Credit Desk is Phase 2). **Two inputs still needed from Clint before their respective steps: the unstated Track-2 scheduling question, and the empty "AI Integration #3" item** — plus the 4 project names before Layer 1 activation.

---

## Next Steps (in order)

1. [ ] **ACTIVE TODAY — execute `S-BOS_Plan_2026-07-02-entry-level-housing-launch-day.md`** in the Claude Code session. Layer 0 first (generic foundation), then Layer 1 (populate/activate), then Layer 2 (surface). Get the two flagged open inputs + the 4 project names from Clint before their respective steps. Q2 Loan Reconciliation runs in parallel on existing tools, independently of this sequencing.
2. [ ] **§4 Core Features — layer in Clint's per-screen write-ups.** Paused for today, not blocked. Claude pre-drafted each Phase-1 screen (Launch Pad, CRM Home, Project detail+facets, The Game, My Responsibilities, Account Pyramid, Time Card) with What/Reads-Writes/Who/Keep-change-drop + a **proposed AI/automation** line pointing to the Dispatcher (per the PDD §4 implementation note). Clint adds his own capture per screen when this resumes.
3. [ ] User Workflows: **bidirectional-mirror** sync spec (for categories/projects with existing live data — see the cutover doc's §4 boundary) + feedback-triage/onboarding support layer.
4. [ ] DB Schema — reconcile live `sb-crm-poc` schema to §3: add **Loans, Time Cards, Contracts, accounting reference tables, Decision Gates/Ratings, Property-as-link, Game entities, one universal Task table**; RLS policies for the Access model (entity-link + Stakeholder Bridge).
5. [ ] Tech Spec, Decisions Log (ADRs).
6. [ ] **(Parallel track, pick up when ready — not blocking #2–5):** remaining Kompass Dispatch Architecture open items per `S-BOS_Discussion_kompass-dispatch-architecture.md` §7 not already covered by today's plan — Option A→B trigger conditions, ToS confirmation for licensing paperwork, delegated-approval decision for licensees, doc-gen build-vs-buy call.

---

## Open Questions / Decisions Pending

- Automation rebuild approach (103 captured via screenshots — intact; not API-extractable).
- Remote CRUD MCP host: Supabase Edge Functions vs Railway (decide at build time).
- **Bidirectional sync mechanics** (ID pairing, field-level last-write-wins, sync ledger, loop prevention) — spec when Workflows is worked.
- App cleanup: strip "Kevin's Rule" + "Misogi" from TSW app files (`03-stitser-way/messaging.md`, `web/README.md`, + sweep fixtures).
- Accounting structure (family-trust QB; Intacct→QB consolidation) — deferred input.
- Kompass Dispatch Architecture pending decisions — see `S-BOS_Discussion_kompass-dispatch-architecture.md` §7.
- Category-Scoped Cutover open items — see `S-BOS_Discussion_category-scoped-cutover.md` §6 (SmartSuite conditional-permission verification, exact field/table ID mapping, Budget/Pay App parity build, whether the pattern extends beyond this pilot).
- **Today's plan open inputs** — see `S-BOS_Plan_2026-07-02-entry-level-housing-launch-day.md` bottom section: Track 2's unstated scheduling question, the empty AI Integration #3 item, the 4 chosen project names, and which tool generates Q2 loan invoices.

---

## Environment Notes

- **Build is now driven from the Claude Code chat**, working `sb-crm-poc` **in place** at `/Users/clintstitseroffice/Documents/sb-crm-poc` (the other claude.ai chat is paused — single write surface). `.env.local` has Supabase creds; `node scripts/db.mjs` + the Storage API both verified.
- Build docs: `SBos-Knowledge-Base/projects/s-bos/build-docs/` (read/write via GitHub). Code: `sb-crm-poc`.
- Roadmap app (`sb-planning-tools` repo) is claude.ai-owned — sync before editing.

---

## How to Resume

1. Read `S-BOS_Operating_Agreement.md`, then `memory.md`, `restart.md`, `S-BOS_Design_Context.md`.
2. If today's plan is still active, open `S-BOS_Plan_2026-07-02-entry-level-housing-launch-day.md` directly and pick up at the next unfinished layer/item.
3. Otherwise, open `S-BOS_Product_Design_Doc.md` — §3 + Gate 2 ✅ locked; §4 Core Features is drafted screen-by-screen (Phase-1), §6/§9 carry the Phase 1/2 split.
4. Say "let's go."
