# Memory: S-BOS

> **Purpose:** Running log of key facts, decisions, and context. Read at the start of every session.
> **Update:** Whenever a decision is made or context shifts. Rewritten at session end. Clint reviews.

---

## Project Identity

- **Project Name:** S-BOS (Stitser Business Operating System)
- **App Description:** A **universal operator shell** — domain-structured operating system + trained assistant — replacing SmartSuite + Softr with a proprietary stack. S-BOS (internal) and the four Kompass verticals (Developer/Contractor/Agent/**Personal=TSW**) are **configurations of one shell**, isolated per `org_id` (licensable/franchisable).
- **Goal (V1):** Biz Dev CRM module fully live on the new stack at parity with Softr; then migrate remaining modules.
- **Philosophy:** Claude-serviceable, expansion-ready, no vendor lock-in, **parallel-run via a bidirectional mirror** (no whole-company big-bang). Calibrated honesty over confident answers.
- **Current Phase:** Migration in progress (POC live). **PDD: Gate 1 PASSED (2026-06-29); §2.5 Vision written; Core Entities next.**

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
- **Accounting (deferred):** family-trust accounting → QuickBooks (departments + projects); business on Intacct (inaccessible now); possible Intacct→QB multi-company consolidation. Accounting rides the same Entity→Category→Project tree (Entity=GL company, Category=class/product line, Project=job, Budget atom from QB/Intacct). Not Phase 1.

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
