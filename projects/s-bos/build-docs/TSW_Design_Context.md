# Context: Stitser Way

> **Purpose:** Orients Claude to the Stitser Way App. Read alongside `TSW_memory.md` and `TSW_restart.md` at the start of every session.
> **Update:** When scope, stack, or working rules change. Living document.
> **Working agreement:** `TSW_Operating_Agreement.md` holds the collaboration model and rules. This file is project-specific orientation only.

---

## What This App Is

**Name:** Stitser Way

**One-sentence description:** A personal operating system web app for Clint Stitser and his family — surfacing goals, habits, rituals, daily planning, and life domain tracking through a mobile-first UI with Claude integrated into the experience.

**Problem it solves:** The existing SmartSuite Game App infrastructure (Goals, Priorities, Stats, GYR, Milestones) contains the full data model for personal goal tracking, but the current Softr UI is constrained — it can't integrate Claude, can't support family profiles, can't deliver day-mode-aware experiences, and can't be extended without Softr's limitations. Stitser Way rebuilds the UI layer on a proprietary stack with full LLM integration.

**Primary user:** Clint Stitser — personal operating system, daily driver.

**Secondary users:** Family members (Christie, Avery, Brynn, Max) — each with their own profile, age-appropriate autonomy, and customized experience. Kids enter their own data.

**Core value:** A single place where every domain of life (Body, Being, Balance, Business) has goals, phases, progress tracking, rituals, and Claude-assisted processing — all connected, all in context, all running on data that already exists.

---

## Relationship to S-BOS

| | S-BOS | Stitser Way |
|---|---|---|
| Purpose | Business operating system | Personal operating system |
| Primary users | Stitser BUILT internal team | Clint + family |
| Domains | CRM, project mgmt, budgets, financials | Body, Being, Balance, Business (personal) |
| Data Phase 1 | SmartSuite (migrating to Supabase) | SmartSuite Game App (migrating to Supabase) |
| Stack | Next.js / Supabase / Railway | Next.js / Supabase / Railway |
| Build docs | `S-BOS_*.md` | `TSW_*.md` (this folder) |
| Design approach | Back-fill from running POC | Greenfield UI, existing data model |

Shared infrastructure: Railway hosting, Supabase (Phase 2), SmartSuite MCP access, Kompass Claude skills.

---

## Data Strategy

**Phase 1 — SmartSuite as the live database:**
The app reads from and writes to SmartSuite Game App via the Kompass MCP. The schema is already documented (pulled 2026-06-24). No new data infrastructure required to launch.

**Phase 2 — Migrate to Supabase:**
Once S-BOS Supabase infrastructure is built, Stitser Way migrates. A Supabase schema doc will be written at Phase 2 start. The Phase 1 Data Integration doc captures field slugs and app IDs precisely so migration is clean.

**SmartSuite Game App — 10 tables (Phase 1 data sources):**

| Table | App ID | Role |
|---|---|---|
| Goals | `6824d4d1885a8769bd2dfc0d` | Top-level goal records per domain |
| Current Priorities | `68216f48f98789b5bb095a4b` | Phase-level priorities within a goal |
| Milestones | `68216f48f98789b5bb095a37` | Sub-steps within a priority |
| GYR Status Reports | `68216f48f98789b5bb095a51` | GYR Spiral processing — facts/feelings/root/focus/actions/fruit |
| Stats | `6840927ebcfa2d2bfef039e2` | Logged actuals (weight, drinks, rides, etc.) |
| Stats Menu | `68409420391d32d925740e28` | Measurable types — the stat type catalog |
| Tasks | `683e80437ee1bca637ba6fde` | Linked actions within a priority |
| Priorities Template | `68806392f4f22c070b80af40` | Template library for priority scaffolding |
| Milestone Templates | `6880637e4585c8c8b0ae9ee8` | Template library for milestone scaffolding |
| XX-Template Sample Metrics | `68216f48f98789b5bb095a5a` | Sample metric scaffolding |

**External data sources (Phase 1 integrations):**

| Source | Data | Integration method |
|---|---|---|
| Strava | Training rides, watts, elevation, TSS | Strava MCP |
| Google Calendar | Events, focus blocks, upcoming schedule | Google Calendar MCP |
| Gmail | Email triage, reply queue | Gmail MCP |
| SmartSuite Journals/Rituals | Morning/evening rituals, stacks, table talk | Kompass MCP (`68f8f8fe3757414d70d94ae0`) |

---

## Tech Stack

| Layer | Technology | Notes |
|---|---|---|
| Frontend | Next.js (App Router) + React + TypeScript | Same as S-BOS |
| Styling | Tailwind CSS v4 | Mobile-first — primary device is phone |
| Backend | Next.js Server Actions | SmartSuite API key stays server-side |
| Data (Phase 1) | SmartSuite Game App via MCP | Goals, Stats, GYR, Priorities |
| Data (Phase 2) | Supabase (Postgres) | Migrates from SmartSuite |
| Auth | Supabase Auth (Phase 2) / Simple auth (Phase 1) | Family profiles need auth |
| Hosting | Railway | Separate deployment from S-BOS |
| LLM | Claude via Anthropic API | Integrated into Spiral, coaching, suggestions |
| External | Strava MCP, Google Calendar MCP, Gmail MCP | Training, calendar, email |

---

## Key Constraints & Non-Negotiables

- **Mobile-first.** Primary use is on phone — every screen designed for mobile viewport first.
- **Kids own their data.** Each family member has their own profile and enters their own data. Age-appropriate autonomy is a design principle.
- **Claude is integrated, not bolted on.** The LLM is part of the experience — Spiral processing, day-mode suggestions, coaching, reminders — not a separate chat window.
- **Day-mode aware.** The app changes its posture based on Focus / Buffer / Free day type, suggested by Kompass and confirmed by Clint.
- **Universal Goal card.** Every initiative across every domain uses the same five-step pattern: Current Score → New Goal + Deadline → Rhythm & Reminders → Progress Tracking → Celebrate Wins. Domain-specific customization layers on top.
- **No reinventing existing infrastructure.** SmartSuite Game App already has the data model. The app surfaces it — it does not rebuild it.

---

## Core Design Decisions (from discovery session 2026-06-24)

These are confirmed decisions, not proposals. Captured here as inputs to the PDD.

1. **Four life domains:** Body / Being / Balance / Business — everything in life has a home in one of these.
2. **Day-mode model:** Focus Day (deep work, app goes silent), Buffer Day (Kompass-assisted clearing), Free Day (incubation, app gets out of the way). Suggested by Kompass from calendar + rings + recent pattern; confirmed by Clint with a gut check (uh-huh / uh-uh).
3. **Horizon Rings — dual mode:** Stacked list (default, daily driver) + Circles overview (one-tap step-back). Stacked handles triage; Circles handles orientation. Toggle in top-right nav, not a tab.
4. **Four-tab nav + Shortcuts:** Today / Horizon / Me / Shortcuts (28 Claude skills + external tools).
5. **Me tab = domain menu:** Top-level shows four domain cards. Tap any domain → Goals for that domain. Tap any Goal → universal initiative screen.
6. **Universal Goal engine (five steps):** Current Score → Goal + Deadline → Rhythm & Reminders → Progress Tracking → Celebrate Wins. Same template for every initiative; domain-specific fields layer on top.
7. **Sacral decision model:** Kompass makes one suggestion with reasoning. Clint responds gut-check style. Max two questions. Never open-ended.
8. **Horizon Rings — five rings:** Overdue / This Week / Active / Coming Soon / Parked. Sources: Tasks + Notes/Comments + GYR Follow-Ups.
9. **Table Talk ritual:** Hi / Lo / Buffalo (also: Rose / Thorn / Bud). Dinner table check-in. Family member selector. Lives on Today tab and in Journal history.
10. **Body section protocol:** Sourced from evidence-based protocol (fat loss, visceral fat, MTB training). Three phases: Foundation → Engine → Race Block. Strava integration for training log. Meal tracker, drinking log, weight/body fat tracker, health vault (DXA, blood tests, eye prescriptions, skin).

---

## What We Are NOT Building (Scope Boundaries)

| Feature | Why excluded | Reconsider when |
|---|---|---|
| Supabase schema (Phase 1) | SmartSuite is the live DB for launch | Phase 2 begins |
| Multi-tenant / licensing | Personal app, not a product yet | Post-launch |
| Native mobile app (iOS/Android) | Web app is sufficient for Phase 1 | Post-Supabase migration |
| Full business CRM inside Stitser Way | That's S-BOS territory | Never — keep apps separate |

---

## Open Questions

| Question | Blocks | Priority |
|---|---|---|
| Goal Type field values in SmartSuite — how are domains tagged? | Domain filtering in Me tab | High |
| Family profile auth model for Phase 1 | Kids' data isolation | High |
| Strava sync frequency and data freshness | Training log accuracy | Med |
| Claude API integration pattern — server action vs edge function | Spiral + coaching features | Med |
| Phase 1 write-back to SmartSuite — which fields, which tables | Stats logging, GYR reports | Med |

---

## File Inventory

| File | Purpose | Status |
|---|------|-------|
| **— Continuity —** | | |
| `TSW_Operating_Agreement.md` | Working agreement | ✅ Active |
| `TSW_Design_Context.md` | This file | ✅ Active |
| `TSW_memory.md` | Running facts, decisions, context | ✅ Active |
| `TSW_restart.md` | Where we stopped, next steps | ✅ Active |
| **— Design —** | | |
| `TSW_Product_Design_Doc.md` | Problem, users, entities, features, workflows | 🔄 In Progress — §1 drafted |
| `TSW_Data_Integration_Doc.md` | Phase 1 SmartSuite field map (app IDs, slugs, read/write) | ⏳ Not Started |
| `TSW_DB_Schema.md` | Phase 2 Supabase schema | ⏳ Not Started (Phase 2) |
| `TSW_Technical_Spec.md` | Architecture, services, state machines, Claude integration | ⏳ Not Started |
| `TSW_UI_UX.md` | Design system, screens, mobile-first patterns | ⏳ Not Started |
| `TSW_UI_Strings.md` | User-visible copy | ⏳ Not Started |
| `TSW_Decisions_Log.md` | Design-phase ADRs | ⏳ Not Started |
| **— Coding Prep —** | | |
| `TSW_Module_Breakdown.md` | Modules, dependencies, build order | ⏳ Not Started |
| `TSW_API_Contract.md` | Request/response schemas | ⏳ Not Started |
| `TSW_Testing_Strategy.md` | Test types, coverage | ⏳ Not Started |
| `TSW_Deployment_Config.md` | Environments, build, rollback | ⏳ Not Started |
| **— Build —** | | |
| `TSW_Build_Decisions_Log.md` | Typed build-phase decisions | ⏳ Not Started |
| `TSW_Progress_Checklist.md` | Per-module progress | ⏳ Not Started |

---

## How to Start a Session

1. Read `TSW_Operating_Agreement.md`.
2. Read `TSW_memory.md`, `TSW_restart.md`, `TSW_Design_Context.md` (in that order).
3. Confirm what we're working on before acting.
4. Say "let's go" or name the task — pick up from `TSW_restart.md` → Next Steps.
