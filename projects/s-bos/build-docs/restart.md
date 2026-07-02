# Restart: S-BOS

> **Purpose:** Capture exactly where we stopped so the next session picks up without re-explaining.
> **Update:** Before ending every session. Overwrite entirely — current stopping point only.

---

## Session Info

- **Date:** 2026-07-02
- **Project:** S-BOS
- **Status:** In Progress — Gates 1 & 2 PASSED; §2.5 + §3 locked. §4 Core Features started (screen-by-screen back-fill) and still queued as Next Step #1 below — **unchanged this session.** This session ran two parallel threads (neither reorders §4): (1) the Kompass Dispatch Architecture — how Pillar C/D dispatch work to Claude; (2) a new, **active-today** build item — the **Category-Scoped One-Way Cutover pilot** for Entry-Level Housing, refining the bidirectional-mirror decision for new-Category work with no legacy data.

---

## What We Were Doing

Consolidated the two efforts (the SmartSuite/TSW work + the Supabase `sb-crm-poc` build) into **one Claude Code chat** (the other chat is paused). Back-filled the PDD's **Product Vision & Architecture (§2.5)** from a long design conversation, and **Clint signed off Gate 1.** This session, separately, explored and documented the Kompass Dispatch Architecture end-to-end (see `S-BOS_Discussion_kompass-dispatch-architecture.md`).

---

## Where We Stopped

- **Gate 1 ✅ APPROVED (Clint, 2026-06-29).** Problem + Target Users done.
- **PDD §2.5 Product Vision & Architecture written** (🔄, deepens as features develop): the **universal shell** (one codebase, configured per `org_id`; TSW = Personal Kompass config), four pillars — (A) Execution/Org incl. **Master Property** persistent anchor + two-track roll-up, (B) the **Brain** (6 stores; 3 exist, Vendor Ratings + Knowledge Library to build), (C) the **Kompass assistant + Feed** (first surface = Task-Level AI Assistant), (D) **Execution Tools** — plus the **Blueprint/Template Catalog** (Category-anchored, activatable; grounded in the CrossMod land-dev stage-gate example) and the **TSW module catalog** (Kevin's Rule + Misogi removed; habit kept).
- **Recovery resolved:** the morning incident did NOT lose automation captures — 4 documented/in-progress + 9 screenshots intact, 0 orphans (verified against live Supabase).
- **New this session — Kompass Dispatch Architecture documented** (`S-BOS_Discussion_kompass-dispatch-architecture.md`): Compass-owns-the-catalog / Claude-as-stateless-compute model; document-generation build approach (code execution + Files API + Skills API); Realtor Kompass multi-tenant/licensing decisions (shared workspace now, dividers built for later separation; BYOK as an optional path); the adopted skill review-gate workflow; and a clarified data-residency answer (Supabase = source of truth, Anthropic Skills API = execution mirror only). Full detail and open items live in that file — not duplicated here.
- **New this session — Category-Scoped One-Way Cutover pilot decided and specced** (`S-BOS_Discussion_category-scoped-cutover.md`), **ACTIVE — build today in Claude Code:** Entry-Level Housing gets flagged `cutover_mode: supabase_native`; new projects under it live/edit only in the new app; Details/Schedule/Tasks/Team push one-way to SmartSuite via a new ID-pairing table; SmartSuite side gets a Day-1 soft lock (banner + view removal) on synced facets, with a real permission lock and a watchdog automation as fast-follows. Budget/Pay App facet deferred entirely (no build, no dual-entry) — workable because these are brand-new projects with no existing SmartSuite financial data; **does not generalize** to categories with live budget data. This refines, does not replace, the bidirectional-mirror decision — see the doc's §4 for the boundary. Clint is naming the specific launching projects separately.

---

## Next Steps (in order)

1. [ ] **ACTIVE TODAY — Category-Scoped One-Way Cutover pilot (Entry-Level Housing).** Build in the Claude Code session per `S-BOS_Discussion_category-scoped-cutover.md` §5: Category cutover flag, ID-pairing table, one-way push (Details/Schedule/Tasks/Team), Day-1 SmartSuite lockout, updated New-Project flow for this Category. Get the specific launching project names from Clint before/at build start. This does not block or reorder the items below.
2. [ ] **§4 Core Features — layer in Clint's per-screen write-ups.** Claude pre-drafted each Phase-1 screen (Launch Pad, CRM Home, Project detail+facets, The Game, My Responsibilities, Account Pyramid, Time Card) with What/Reads-Writes/Who/Keep-change-drop + a **proposed AI/automation** line. Clint adds his own capture per screen, **especially the "New features / automations / AI integrations" section.** Then → User Workflows + Gate 3.
3. [ ] User Workflows: **bidirectional-mirror** sync spec (for categories/projects with existing live data — see the cutover doc's §4 boundary) + feedback-triage/onboarding support layer.
4. [ ] DB Schema — reconcile live `sb-crm-poc` schema to §3: add **Loans, Time Cards, Contracts, accounting reference tables, Decision Gates/Ratings, Property-as-link, Game entities, one universal Task table**; RLS policies for the Access model (entity-link + Stakeholder Bridge).
5. [ ] Tech Spec, Decisions Log (ADRs).
6. [ ] **(Parallel track, pick up when ready — not blocking #2–5):** Kompass Dispatch Architecture open items per `S-BOS_Discussion_kompass-dispatch-architecture.md` §7 — Dispatcher invocation contract, Skill/Routine/Plugin Package table schema, Option A→B trigger conditions, ToS confirmation for licensing paperwork, delegated-approval decision for licensees, doc-gen build-vs-buy call.

---

## Open Questions / Decisions Pending

- Automation rebuild approach (103 captured via screenshots — intact; not API-extractable).
- Remote CRUD MCP host: Supabase Edge Functions vs Railway (decide at build time).
- **Bidirectional sync mechanics** (ID pairing, field-level last-write-wins, sync ledger, loop prevention) — spec when Workflows is worked.
- **Cutover pace:** Clint wants to go faster than a cautious read; CRM-module forcing-function is the candidate (NOT whole-company big-bang). Gated on parity + the support layer.
- App cleanup: strip "Kevin's Rule" + "Misogi" from TSW app files (`03-stitser-way/messaging.md`, `web/README.md`, + sweep fixtures).
- Accounting structure (family-trust QB; Intacct→QB consolidation) — deferred input.
- Kompass Dispatch Architecture pending decisions — see `S-BOS_Discussion_kompass-dispatch-architecture.md` §7.
- Category-Scoped Cutover open items — see `S-BOS_Discussion_category-scoped-cutover.md` §6 (SmartSuite conditional-permission verification, exact field/table ID mapping, Budget/Pay App parity build, whether the pattern extends beyond this pilot). **Which specific projects launch — Clint to name.**

---

## Environment Notes

- **Build is now driven from the Claude Code chat**, working `sb-crm-poc` **in place** at `/Users/clintstitseroffice/Documents/sb-crm-poc` (the other claude.ai chat is paused — single write surface). `.env.local` has Supabase creds; `node scripts/db.mjs` + the Storage API both verified.
- Build docs: `SBos-Knowledge-Base/projects/s-bos/build-docs/` (read/write via GitHub). Code: `sb-crm-poc`.
- Roadmap app (`sb-planning-tools` repo) is claude.ai-owned — sync before editing.

---

## How to Resume

1. Read `S-BOS_Operating_Agreement.md`, then `memory.md`, `restart.md`, `S-BOS_Design_Context.md`.
2. Open `S-BOS_Product_Design_Doc.md` — §3 + Gate 2 ✅ locked; §4 Core Features is drafted screen-by-screen (Phase-1), §6/§9 carry the Phase 1/2 split.
3. For the parallel Kompass Dispatch Architecture thread, open `S-BOS_Discussion_kompass-dispatch-architecture.md` directly.
4. Say "let's go" — first action is **layering Clint's per-screen write-ups (+ AI/automation) into §4**, unless Clint directs otherwise (e.g., continuing the Dispatch Architecture thread).
