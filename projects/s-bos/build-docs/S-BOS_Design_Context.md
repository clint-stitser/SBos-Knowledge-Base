# Context: S-BOS

> **Purpose:** Orients Claude to this project. Read alongside `memory.md` and `restart.md` at the start of every session.
> **Update:** When scope, stack, or working rules change. Living document.
> **Working agreement:** `S-BOS_Operating_Agreement.md` holds the collaboration model and rules. This file is project-specific orientation only.

---

## What This App Is

**Name:** S-BOS (Stitser Business Operating System)

**One-sentence description:** The internal operating platform for Stitser BUILT — CRM, project management, budgets, financials, training, and more — used by the internal team and (later) external stakeholders.

**Problem it solves:** The current stack (SmartSuite as database + Softr as user portal) is hitting SmartSuite API limits, can't be fully managed/serviced by Claude, and can't be licensed/franchised. S-BOS rebuilds it on a proprietary, Claude-serviceable, expansion-ready stack.

**Primary user:** Internal Stitser BUILT team (biz dev, PMs, accounting) working day-to-day in the portal.

**Secondary users:** Clint (admin/builder); external stakeholders later (investors, brokers, clients); future franchisees/licensees.

**Core value:** Surface the right data to the right person with per-user permissions, while being a system Claude can document, service, and extend without a human in the loop.

---

## Current State

**Phase:** Migration in progress.
- **Live on new stack:** Biz Dev CRM module POC — Projects / People / Companies with detail views, write-back (Notes & Comments hub with follow-ups), and a rebuilt SmartSuite formula (People "Comment Status") as a live Postgres view.
- **In design / triage:** the remaining modules across 27 SmartSuite solutions; 103 automations being captured; tables being triaged via the Migration Menu tool.

> File-level status lives in the **File Inventory** at the bottom.

---

## Tech Stack

| Layer | Technology | Version | Rationale |
|---|---|---|---|
| Frontend | Next.js (App Router) + React + TypeScript | 16 / 19 / 5 | Mainstream, hireable, server/client split |
| Styling | Tailwind CSS | v4 | Fast, consistent |
| Backend | Next.js Server Actions | (Next 16) | Secret key stays server-side; no separate API tier |
| Database | Supabase (Postgres) | — | No API limits; standard Postgres = portable, not locked in |
| Auth | Supabase Auth (Google OAuth + magic link) | — | **Planned.** Invite/allowlist + roles + RLS; expansion-ready (see Decisions Log) |
| Storage | Supabase Storage | — | File/screenshot storage (e.g., automation tracker) |
| Hosting | Railway | — | Hosts the Next.js app; Supabase is the data/auth/storage platform |
| Data access (Claude) | `scripts/db.mjs` (pg over pooler) + Supabase MCP | — | Direct SQL execution — no dashboard, no copy-paste |

---

## Key Constraints & Non-Negotiables

- **Users must not perceive the change.** The new portal must look/behave like the current Softr portal (pixel-match, same URLs/flow). This is a backend infrastructure upgrade, invisible to the team.
- **Claude-serviceable.** Schema, formulas, and automations must be changeable by Claude directly (via `scripts/db.mjs` / migrations) — no human-in-the-loop dashboard steps.
- **Expansion-ready.** Architecture must support licensing/franchising → multi-tenant "instances" via `org_id` + RLS (single-tenant optional premium tier).
- **No vendor lock-in.** Supabase is standard Postgres; data is portable by design.
- **Parallel run, no big-bang cutover.** Old system stays live until each module is proven.

---

## Third-Party Integrations

> Source: the System Atlas connection web. These are seams the migrated modules touch.

| Service | Purpose | Status |
|---|---|---|
| SmartSuite | Current source of truth during migration (schema/data export via MCP) | Active (being migrated off) |
| Sage Intacct | Accounting mirror (Customer/Vendor/Location/Invoice tables) | Active integration |
| Google Drive / Workspace | File storage, Google login domain (`stitserbuilt.com`) | Active |
| DocuSign | Agreements/envelopes | Active (MCP present) |
| Zapier | Existing automation glue (to be assessed during automation rebuild) | Active |

---

## What We're NOT Building (Scope Boundaries)

| Feature / Capability | Why excluded | Reconsider when |
|---|---|---|
| Big-bang full-system cutover | Too risky; parallel run instead | Never — phased by module |
| External-user portal (investors/brokers) | Not in POC scope | Auth Phase B |
| Multi-tenant franchise instances | Not needed yet | First license/franchise deal (Auth Phase C) |
| *(more to define in the PDD back-fill)* | | |

---

## Open Questions

| Question | Blocks | Priority | Owner |
|---|---|---|---|
| Automation rebuild approach (103 automations, not API-extractable) | Coding-phase parity | High | Clint |
| Module/cutover sequencing across 27 solutions | Roadmap | High | Clint |
| Which tables to bring/drop/merge | Schema scope | High | Clint (via Migration Menu) |
| Title/Type field + other un-seeded fields | Data completeness | Med | Clint |

---

## Project-Specific Working Rules

- **Back-fill, don't invent.** Design docs are reverse-engineered from the running system + existing planning docs. Mark anything reverse-engineered (vs. a stated decision) so it's auditable.
- **Two repos:** design/build docs here (`projects/s-bos/build-docs/`); code in `sb-crm-poc`. Don't mix.
- **Existing planning docs are inputs:** `sb-crm-poc/docs/` (Atlas, auth-plan, recovery-and-restore-plan, formula-audit, automation guides) and the live trackers. Fold these into the formal docs rather than duplicating.

---

## File Inventory

> Single source of truth for build-doc status. (The raw template library lives in `templates/`.)

| File | Purpose | Status |
|------|---------|--------|
| **— Continuity (read at session start) —** | | |
| `S-BOS_Operating_Agreement.md` | Working agreement | ✅ Active |
| `S-BOS_Design_Context.md` | This file — project orientation | ✅ Active |
| `memory.md` | Running facts, decisions, context | ✅ Active |
| `restart.md` | Where we stopped, next steps | ✅ Active |
| **— Design (to back-fill) —** | | |
| `S-BOS_Product_Design_Doc.md` | Problem, users, entities, features, workflows | ⏳ Not Started — **next** |
| `S-BOS_DB_Schema.md` | Data model (back-fill from live Supabase schema) | ⏳ Not Started |
| `S-BOS_Technical_Spec.md` | Architecture, stack, services, state machines, events | ⏳ Not Started |
| `S-BOS_UI_UX.md` | Design system, screens (back-fill from built UI) | ⏳ Not Started |
| `S-BOS_UI_Strings.md` | User-visible copy | ⏳ Not Started |
| `S-BOS_Sample_Data.md` | Canonical sample rows | ⏳ Not Started |
| `S-BOS_Decisions_Log.md` | Design-phase ADRs (back-fill from decisions already made) | ⏳ Not Started |
| `S-BOS_Cross_Doc_Validation_Checklist.md` | Cross-doc consistency check | ⏳ Not Started |
| **— Coding Prep —** | | |
| `S-BOS_Coding_Kickoff_Checklist.md` | Pre-coding gate | ⏳ Not Started |
| `S-BOS_Module_Breakdown.md` | Modules, dependencies, build order | ⏳ Not Started |
| `S-BOS_API_Contract.md` | Request/response schemas | ⏳ Not Started |
| `S-BOS_Database_Migration_Checklist.md` | Migration registry (back-fill: 001–010 already applied) | ⏳ Not Started |
| `S-BOS_Component_Service_Layer_Map.md` | Components & services (back-fill from code) | ⏳ Not Started |
| `S-BOS_Testing_Strategy.md` | Test types, coverage, CI | ⏳ Not Started |
| `S-BOS_Deployment_Config.md` | Environments, build, rollback (back-fill: Railway live) | ⏳ Not Started |
| `S-BOS_Pre_Build_Validation_Checklist.md` | Cross-mesh validation | ⏳ Not Started |
| **— Build —** | | |
| `S-BOS_Build_Decisions_Log.md` | Typed build-phase decisions (BD-XX) | ⏳ Not Started |
| `S-BOS_Progress_Checklist.md` | Per-module progress | ⏳ Not Started |
| `S-BOS_Mid_Build_Review_1.md` | Drift check | ⏳ Not Started |
| **— Closeout —** | | |
| `S-BOS_Phase_Closeout_1.md` | Per-phase closeout | ⏳ Not Started |
| `S-BOS_Project_Closeout.md` | Final closeout | ⏳ Not Started |
| `S-BOS_Template_Update_Worklog.md` | Template back-feed | ⏳ Not Started |
| **— As needed —** | | |
| `S-BOS_Discussion_[topic-slug].md` | Per-topic discussion (on demand) | ⏳ Not Started |

---

## How to Start a Session

1. Read `S-BOS_Operating_Agreement.md` (working agreement).
2. Read this file, `memory.md`, and `restart.md` (in that order).
3. Confirm what we're working on before acting.
4. Flag any missing/stale continuity file immediately.
5. Say "let's go" or name the task — pick up from `restart.md` → Next Steps.
