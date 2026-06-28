# Memory: S-BOS

> **Purpose:** Running log of key facts, decisions, and context. Read at the start of every session.
> **Update:** Whenever a decision is made or context shifts. Rewritten at session end. Clint reviews.

---

## Project Identity

- **Project Name:** S-BOS (Stitser Business Operating System)
- **App Description:** Internal operating platform for Stitser BUILT (CRM, project mgmt, budgets, financials, training), replacing SmartSuite + Softr with a proprietary stack.
- **Goal (V1):** Biz Dev CRM module fully live on the new stack at parity with Softr, invisible to users; then migrate remaining modules.
- **Philosophy:** Claude-serviceable, expansion-ready, no vendor lock-in, parallel-run (no big-bang cutover). Calibrated honesty over confident answers.
- **Current Phase:** Migration in progress (POC live; remainder in design/triage).

---

## Tech Stack

| Layer | Technology | Notes |
|---|---|---|
| Frontend | Next.js 16 / React 19 / TypeScript / Tailwind v4 | App Router, server actions |
| Backend | Next.js Server Actions | Secret key server-side only |
| Database | Supabase (Postgres) | Standard Postgres = portable |
| Auth | Supabase Auth (Google + magic link) | **Planned** — invite/roles/RLS |
| Storage | Supabase Storage | Public bucket for tracker screenshots |
| Hosting | Railway | App host; live at sb-crm-poc-production.up.railway.app |
| Claude DB access | `scripts/db.mjs` (pg pooler `aws-1-us-east-1`) | Direct SQL; no dashboard |

---

## Decisions (design-phase / cross-cutting)

- **Supabase over SmartSuite** as the database platform (API limits + Claude-serviceability drove this).
- **Railway hosts the app; Supabase is the data/auth/storage platform.** Railway Postgres considered and rejected — it's just a DB; we'd lose Supabase Auth/Storage/API.
- **Supabase Auth over Cloudflare Access** — ties into RLS for per-row permissions; drops Cloudflare entirely.
- **Invite/allowlist + roles + RLS** is the access model (not domain-restriction). Staff via Google (domain-restricted), external via magic link. Expansion-ready: `org_id` multi-tenant "instances" for licensing/franchise. (See `sb-crm-poc/docs/auth-plan.md`.)
- **Recovery model:** soft-delete (recycle bin) + audit-log (field history) + Supabase PITR. (See `sb-crm-poc/docs/recovery-and-restore-plan.md`.)
- **Formulas → Postgres views/computed columns**; proven with People "Comment Status" (live view, recomputes on read). (See `sb-crm-poc/docs/formula-audit-people-comment-status.md`.)
- **Automations: NOT API-extractable** (confirmed). 103 across 8 solutions; captured via screenshots in the live Automation Tracker, then rebuilt from intent as Postgres triggers / Edge Functions.
- **Notes & Comments is a hub** — one note links to many People/Companies/Projects + assigns follow-ups to team members (many-to-many via junction tables).
- **Adopting Ryan Falke's build-doc methodology** (this folder) to formalize the migration design, mid-stream via back-fill.
- **Access & permissions model (from PDD work).** Internal users = **C·R·U + audit trail + 60-day restore**; **delete = system-admin-only**; external stakeholders = **view-only on scoped elements**. Updates the recovery-plan window (30→60 days); refines auth roles (admin / internal / external).
- **CRM is the platform's shared backbone — not a biz-dev silo.** People & Companies touch every project and play **customer / vendor / internal-staff / investor-lender** roles depending on context → roles are **contextual, not fixed types**. This polymorphic-role principle drives Core Entities (relate via role-bearing relationships, not hard-typed).
- **Build modules represented as App Items on the v2.4 roadmap** (6 App Item Project records, IT/Systems dept, each with a "Build Docs" checklist). The Kind→required-docs convention lives in `S-BOS_App_Item_Doc_Requirements.md`. Roadmap app is **claude.ai-owned** (`sb-planning-tools` repo) — don't edit its files from Claude Code without syncing.
- **Project Prioritization = a Feature, not a table** (decision 2026-06-28). Delivered via decision-gates/checklists. The legacy "Project Prioritization Tool" table is NOT migrated (built long ago, never launched); marked Drop in the Migration Menu. Captured in PDD §4.
- **Remote CRUD MCP (PLANNED, coding-phase module).** To let claude.ai (web/iOS) perform CRUD on Supabase — not just Claude Code via `db.mjs` — we will host a **remote MCP server** at an HTTPS URL (claude.ai only supports remote connectors, not local MCP). It must: require auth on the endpoint, hold the service key server-side, and **enforce the permission model in the server** (internal C·R·U, delete admin-only, audit-log every change, 60-day soft-delete). This is the "Claude-serviceable from anywhere" capability and a building block for the licensable product. **Host: TBD** (Supabase Edge Functions vs Railway — decide at build time). Will get a formal ADR in the Decisions Log + a Tech Spec section.

---

## Conventions Established

- **`scripts/db.mjs`** is the single entry point for DB changes: `migrate <file>`, `migrate-all`, `sql "..."`, `query "..."`. No dashboard, no copy-paste.
- **Migrations** live in `sb-crm-poc/supabase/migrations/` (001–010 applied; see Database Migration Checklist when back-filled).
- **Writes go through Next.js server actions** using the Supabase secret key — never exposed to the browser.
- **Real CRM data (PII) is gitignored**; regenerable from SmartSuite.
- **Secrets** (`SUPABASE_SECRET_KEY`, DB password) live in `.env.local` (gitignored).
- **Docs separate from code:** build docs in `SBos-Knowledge-Base/projects/s-bos/build-docs/`; code in `sb-crm-poc`.
- **`TRACKER_ONLY=true`** deploy mode: middleware blocks all non-`/admin` routes + omits CRM read key → safe no-auth deployment for the automation tracker.

---

## Existing Artifacts (inputs to back-fill from)

- **Code:** `sb-crm-poc` repo (GitHub `clint-stitser/sb-crm-poc`) — live Biz Dev CRM POC + admin tools (Automation Tracker, Migration Menu).
- **Planning docs (`sb-crm-poc/docs/`):** `atlas/01-biz-dev-crm.md`, `atlas/automation-capture-guide.md`, `atlas/daughter-quickstart.md`, `auth-plan.md`, `recovery-and-restore-plan.md`, `formula-audit-people-comment-status.md`, `biz-dev-crm-poc-spec.md` (in kompass/docs).
- **Live tools:** Automation Tracker (`/admin/automations`, 103 slots, ~3 documented) and Migration Menu (`/admin/migration-menu`, 197 tables triage) at the Railway URL.
- **System Atlas:** Chapter 1 (9 Biz Dev CRM tables) complete; automation counts per solution known.

---

## Open Items

- [ ] Back-fill the PDD (next session) — reverse-engineer from the running POC + planning docs.
- [ ] Decide automation rebuild approach once screenshots are captured.
- [ ] Module/cutover sequencing across the 27 solutions.
- [ ] Triage the 197 tables in the Migration Menu (bring/drop/merge).

---

## Notes

- Decision-maker is **Clint**. Ryan (CS friend) authored the build-doc methodology and vetted Supabase.
- The in-browser automation tool has been flaky this effort — verify via `db.mjs` queries + server-rendered HTML rather than screenshots when it fails.
- Work style: small sprints; dive deep when there's time/space; review async on phone where possible.
