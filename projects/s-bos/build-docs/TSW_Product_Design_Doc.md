# Product Design Doc: Stitser Way

> **Status:** 🔄 In Progress — Gate 1 ✅ COMPLETE. §3 Core Entities in progress.
> **Methodology:** Ryan Falke's Design Templates, adapted for Stitser Way
> **Decision-maker:** Clint Stitser
> **Last updated:** 2026-06-25

---

## Gate System

| Gate | Sections | Status |
|---|---|---|
| Gate 1 | §1 Problem + §2 Users | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 2 | §3 Core Entities | 🔄 In progress |
| Gate 3 | §4 Features + §5 Workflows | ⏳ Not started |
| Gate 4 | §6 Scope + §7 Metrics + §8 Timeline + §9 Open Questions | ⏳ Not started |
| ✅ PDD Done | All gates passed | Data Integration Doc + Tech Spec + UI/UX Doc can begin |

---

## Phase Model

> **Confirmed 2026-06-25.** Phase 1 is for Clint only. Family is entirely Phase 2. The architecture accounts for family expansion but Phase 1 does not build it. Clint validates the machine first.

| Phase | Users | What gets built |
|---|---|---|
| **Phase 1** | Clint only | The full personal OS for one person — all four domains, all rituals, Goal engine, Horizon Rings, Day Mode, Spiral, Big Ass Calendar, Quarterly Habit, Key Docs, Projects, Tools |
| **Phase 2** | Family members | Each family member gets their own profile, their own data, their own experience — age-appropriate, customized to their wiring. Built on the validated Phase 1 machine. |

**Table Talk nuance:** The Table Talk ritual itself is Phase 1 — Clint records the dinner conversation. The per-member experience (each person entering their own Hi/Lo/Buffalo from their own profile) is Phase 2.

**About Me nuance:** Clint's profile (4 GitHub files) is Phase 1. Family profiles are read-only reference data in Phase 1 (Clint can read them to inform his coaching and relationship approach) — they are not interactive or writeable by family members until Phase 2.

---

## §1 — Problem Statement

### The problem

Most people who believe in intentional living have the same experience: the knowledge exists, the desire exists, and the tools exist — but they live in pieces. A habit tracker here. A journal there. Goals in a spreadsheet. Reminders on a sticky note. Coaching in a chat window. Training data in an app that doesn't talk to anything else. And at the center of it all, a person who has to manually hold the whole system together in their head every day.

For Clint, this produces four compounding problems:

**1. Fragmentation.** The infrastructure exists — SmartSuite holds the goals and GYR data, Claude runs the rituals and stacks, Google Calendar holds the year, Strava holds the training, GitHub holds the family profiles. But none of these surfaces know what the others are doing. There is no single place where the full picture of a life is visible, connected, and actionable.

**2. Absence.** There is no application designed around the actual frameworks and principles of intentional living — the GYR Spiral, the four life domains, phase-based accountability, the Container Model, spaced learning, the day-mode model, Brynn's Table Talk ritual, the Vivid Vision, the Misogi, the Quarterly Habit arc. These frameworks exist in Clint's operating system. They do not exist as a coherent, daily-use product.

**3. The activation gap.** Even with the knowledge and the frameworks, sustained installation requires a daily machine. Without it, great insights fade. Goals go untracked. Rituals drift. The GYR Spiral runs once, then gets forgotten. The Vivid Vision is written in January and not looked at until December. The knowledge is there — but there is no system that installs it into the body, the habits, and the daily rhythm, one concept at a time, until it becomes identity.

**4. No project-level tool layer.** Life is not just habits and domains — it contains bounded projects with specific, temporary needs. A trip to Europe needs a budget. A child's ear infection needs a medication schedule. A test needs a study app. Today, these tools are either cobbled together manually, outsourced to generic apps, or not built at all. There is no system that spins up a purpose-built Claude tool for a specific project stage, tracks it through completion, and archives it for future reference or sharing.

### The solution in one sentence

> *"The app is a machine that procedurally produces a better life. You don't need to arrive fully formed. The machine installs you into clarity over time."*

### What a good solution looks like

A single application that integrates the entire practice of intentional living — bringing together the principles, tools, rituals, tracking, coaching, family coordination, and project-level tool-building that Clint has built over years — into one coherent daily experience. A machine, not a collection of tools. One that:

- Surfaces the right information at the right time based on day type, domain status, and what's alive
- Installs frameworks and knowledge through staged, spaced learning — not information dumps
- Connects every domain of life (Body, Being, Balance, Business) to goals, phases, habits, and progress in one place
- Integrates Claude natively as the coaching, processing, suggestion, and tool-building layer — not a separate chat window
- Allows Claude to build custom tools for bounded projects — scoped to the project pillar, used through its lifecycle, archived or shared on completion
- Serves Clint first, and by extension his family, with each member owning their own data and experience

### What this is NOT

- Not a task manager — tasks are one input among many
- Not a journal app — journaling is one output among many
- Not a replacement for S-BOS — that is the business operating system; this is the personal one
- Not a collection of tools glued together — it is a machine with a single coherent logic
- Not finished when the data is entered — it is always working, always suggesting, always tending
- Not limited to recurring life domains — bounded projects with temporary, Claude-built tools are a first-class citizen

---

## §1 Gate 1 Checklist

- [x] Problem clearly stated — ✅ Approved by Clint 2026-06-25
- [x] Solution criteria stated — ✅ Approved by Clint 2026-06-25
- [x] Scope boundaries stated — ✅ Approved by Clint 2026-06-25

---

## §2 — Target Users

### Phase 1 user: Clint Stitser (only)

- Personal operating system, daily driver
- Uses the app across all four life domains: Body, Being, Balance, Business (personal)
- Day-mode aware: Focus Day (deep work), Buffer Day (Kompass-assisted clearing), Free Day (incubation)
- Interacts with Claude for Spiral processing, day-mode confirmation, reminders, and coaching
- Runs morning ritual, evening ritual, weekly review, and stacks (WAR, Cash, Irritation, etc.) through the app
- Operating manual profile: interest-based nervous system, object permanence challenges, consecutive appetite (one thing at a time), Channel of Inspiration (1-8), non-linear thinking is a structural advantage

### Phase 2 users: Family members

> ⏳ Family is entirely Phase 2. Designed for, not built in Phase 1. Architecture supports expansion.

| Member | Role | Phase 2 experience |
|---|---|---|
| Christie | Partner | Full profile, own data, own experience |
| Avery | Child | Own profile, age-appropriate, owns their data |
| Brynn | Child | Initiated Table Talk. Own profile, owns their data |
| Max | Child | Own profile, owns their data |

**Design principle (Phase 2):** Kids enter their own data. Intrinsically motivating — customized to each person's wiring.

### Phase 1 user needs by domain

| Domain | What Clint needs the app to do in Phase 1 |
|---|---|
| Body | Track weight, body fat, meals, alcohol, training rides (Strava), health records. Phase-based protocol. Staged learning. |
| Being | Morning/evening rituals, stacks, decisions, mindset tracking. Spiral processing. |
| Balance | Table Talk (Clint records). Family profiles readable by Clint as reference context. Relationship coaching via Claude. |
| Business | Personal business goals. Phase anchor for allocator seat. GYR status per product line. |

---

## §2 Gate 1 Checklist

- [x] All user types identified — ✅ Approved by Clint 2026-06-25
- [x] Primary user needs per domain stated — ✅ Approved by Clint 2026-06-25
- [x] Family profile model confirmed — ✅ Approved by Clint 2026-06-25

---

## §3 — Core Entities

> **Scope:** Phase 1 entities only. Family entities noted as Phase 2 where relevant but not defined here. All SmartSuite app IDs confirmed by live schema pull 2026-06-25.

---

### Entity Map

| # | Entity | Phase | SmartSuite Source | App ID |
|---|---|---|---|---|
| 1 | Goal | 1 | Game App | `6824d4d1885a8769bd2dfc0d` |
| 2 | Priority | 1 | Game App | `68216f48f98789b5bb095a4b` |
| 3 | Milestone | 1 | Game App | `68216f48f98789b5bb095a37` |
| 4 | Stat | 1 | Game App | `6840927ebcfa2d2bfef039e2` |
| 5 | Stat Menu Item | 1 | Game App | `68409420391d32d925740e28` |
| 6 | GYR Status Report | 1 | Game App | `68216f48f98789b5bb095a51` |
| 7 | Task | 1 | Three sources — see definition | multiple |
| 8 | Journal Entry | 1 | Stitser Way | `68f8f8fe3757414d70d94ae0` |
| 9 | Day Mode Log | 1 | Stitser Way (via automation) | `68f8f8fe3757414d70d94ae0` |
| 10 | BAC Day Type | 1 | Stitser Way | `69458768a624db0406935efc` |
| 11 | BAC Calendar Event | 1 | Stitser Way | `6945877b88051cf9ac527e8a` |
| 12 | BAC Goal | 1 | Stitser Way | `69458793cc79c051739c047b` |
| 13 | Quarterly Habit | 1 | Game App Goals (tagged) | `6824d4d1885a8769bd2dfc0d` |
| 14 | Misogi | 1 | Game App Goals (tagged) | `6824d4d1885a8769bd2dfc0d` |
| 15 | Kevin's Rule Event | 1 | Game App Goals (tagged) | `6824d4d1885a8769bd2dfc0d` |
| 16 | Project | 1 | SB Biz Dev | `68216a706900e8eaf75a05a7` |
| 17 | Check List | 1 | SB Project MGT | `6a060dadc513b3329b7d4485` |
| 18 | Check List Task | 1 | SB Project MGT | `68a8e17251dc814e8c529f3f` |
| 19 | Project Budget (G702) | 1 | SB Project MGT | `68a8c3d2bba73ca6e62d00f0` |
| 20 | Project Budget Line Items (G703) | 1 | SB Project MGT | `68db71a363e88ace0bd45439` |
| 21 | Project Schedule | 1 | SB Project MGT | `68a8d2153c056ca71c9928fd` |
| 22 | Project Dates | 1 | SB Project MGT | `69bb7d64740e0e696d88c47f` |
| 23 | Baseline Budget Items | 1 | SB Project MGT | `69bb89ebf6a195c2c73a3b3e` |
| 24 | Notes & Comments | 1 | SB Biz Dev | `6894e64f621641b3ef90fa99` |
| 25 | Project Tool | 1 | Claude artifact + SmartSuite link | n/a |
| 26 | Vivid Vision | 1 | GitHub (source of truth) | `01-user-profile/vivid-vision-2036.md` |
| 27 | Annual Commitments | 1 | GitHub (source of truth) | `01-user-profile/2026-commitments.md` |
| 28 | Clint's Profile | 1 | GitHub | `01-user-profile/` |
| 29 | Family Profile | 1 (read-only) / 2 | GitHub | `07-family/` |
| 30 | Key Doc | 1 | JSON config → Supabase | n/a |

---

### Key Clarifications from 2026-06-25

**Goals as the universal tag-based entity:**
Quarterly Habit, Misogi, and Kevin's Rule Event are all stored as Goal records in the Game App Goals table — differentiated by a `type` tag field. Not separate entities. Same five-step engine, same SmartSuite table. Type values: Standard Goal / Quarterly Habit / Misogi / Kevin's Rule.

**Tasks pull from three source tables:**
Horizon Rings and the Task entity surface items from:
1. SB Project MGT — Check List Tasks `68a8e17251dc814e8c529f3f` — assigned to Clint, not complete/cancelled
2. SB Biz Dev — Notes & Comments `6894e64f621641b3ef90fa99` — assigned to Clint, follow-up date ≤ today + 7 days
3. Game App — GYR Status Reports `68216f48f98789b5bb095a51` — Clint is follow-up owner, follow-up not completed, Red or Yellow status

**Day Mode → Journal Entry via automation:**
Day Mode is app state (not persisted). An automation fires at a set time after Clint confirms the mode — it creates a Journal Entry of type "Day Mode Log" in the Journals/Rituals table `68f8f8fe3757414d70d94ae0`. Same pattern as the existing dashboard print tag. Provides a permanent record enabling day-type pattern tracking over time (how many Focus Days last month, etc.).

**Project infrastructure confirmed in SB Project MGT `68a8c3d1bba73ca6e62d00f0`:**
The four pillars map to existing tables:
- Budget pillar → Project Budget Management-G702 + Project Budget Line Items-G703 + Baseline Budget Items + Bills & Invoices
- Alignment pillar → Project record itself (purpose, outcome, team) + Project Stakeholder Bridge
- Schedule pillar → Project Schedule + Project Dates
- Checklists pillar → Check Lists + Check List Tasks + Check List Templates

**Vivid Vision and Annual Commitments → GitHub as source of truth:**
Confirmed 2026-06-25. GitHub markdown files (`vivid-vision-2036.md` and `2026-commitments.md`) are the source of truth. The Google Doc becomes a view/reading copy. The app reads from GitHub, consistent with the rest of the profile. Removes the Google Drive integration dependency for these two entities.

**The Stitser Way solution `68f8f8fd3757414d70d94ade` already has BAC infrastructure:**
Three dedicated tables exist: BAC-Day Types, BAC-Calendar Events, BAC-Goals. These are the Phase 1 data source for the Big Ass Calendar feature — not raw Google Calendar data.

---

### Entity Definitions (abbreviated — full field detail in Data Integration Doc)

#### Goals (and tagged sub-types)
Top-level initiative within a life domain. `type` tag differentiates Standard Goal / Quarterly Habit / Misogi / Kevin's Rule. All share the same five-step engine and SmartSuite table.

**Quarterly Habit specific fields:** stage (Install/Beginner/Intermediate/Expert/Complete), streak, target frequency, quarter start/end.

**Misogi specific fields:** event date, description, status (Planned/In Preparation/Complete).

**Kevin's Rule specific fields:** month slot (1–6), description, status (Planned/Complete).

#### Task (composite — three sources)
A discrete action item surfaced in Horizon Rings. Pulled from Check List Tasks, Notes & Comments follow-ups, and GYR Status Report follow-ups. Common display fields: title, due date, source (Task/Note/GYR), linked context (project/goal/priority), status. `Who` privacy field — only populated when the task genuinely involves another person.

#### Journal Entry + Day Mode Log
All ritual completions, stack sessions, Table Talk records, Spirals, free writes, and Day Mode Logs live in the Journals/Rituals table `68f8f8fe3757414d70d94ae0`. Type field differentiates. Day Mode Log is created by automation after Clint confirms the day's mode — not manually entered.

#### BAC Entities
Three dedicated tables in The Stitser Way solution:
- **BAC-Day Types** `69458768a624db0406935efc` — Focus / Buffer / Free day type records
- **BAC-Calendar Events** `6945877b88051cf9ac527e8a` — curated events on the year-at-a-glance (Misogi, Kevin's Rule adventures, races, milestones)
- **BAC-Goals** `69458793cc79c051739c047b` — goals surfaced in the BAC context

#### Project + Four Pillar Tables
Project record lives in SB Biz Dev `68216a706900e8eaf75a05a7`. The four pillars are served by existing SB Project MGT tables — Budget (G702/G703/Baseline), Alignment (Project + Stakeholder Bridge), Schedule (Schedule + Dates), Checklists (Check Lists + Tasks + Templates). The Stitser Way app reads this same infrastructure for personal/family projects.

#### Project Tool
A Claude-built artifact (HTML file) linked to a project record. Stored as a URL + metadata in SmartSuite. Lifecycle stage tracked: Create → Active → Complete → Archived.

#### Vivid Vision + Annual Commitments
Markdown files in GitHub `Clint-s-Kompass` repo. App fetches via GitHub API and renders. Source of truth is GitHub. Annual update prompted by app each New Year.

#### Clint's Profile + Family Profiles
Markdown files in GitHub `Clint-s-Kompass` repo. Clint's 4 files are Phase 1 interactive. Family profiles (Christie, Avery, Brynn, Maxwell, Gwen) are Phase 1 read-only reference — Phase 2 interactive.

#### Key Doc
Named Google Drive link registry. Phase 1: JSON config. Phase 2: Supabase. Links open Drive natively — no API auth.

---

## §3 Gate 2 Checklist

- [ ] All Phase 1 entities named ✳️ *Pending sign-off*
- [ ] No orphan entities ✳️ *Pending sign-off*
- [ ] Phase 1 / Phase 2 boundary clear ✳️ *Pending sign-off*

---

## §4 — Core Features

> ⏳ Not started. Begins after Gate 2.

---

## §5 — User Workflows

> ⏳ Not started. Begins after Gate 2.

---

## §6 — Scope & Phasing

> ⏳ Not started. Begins after Gate 3.

---

## §7 — Success Metrics

> ⏳ Not started. Begins after Gate 3.

---

## §8 — Timeline

> ⏳ Not started. Begins after Gate 3.

---

## §9 — Open Questions

> ⏳ Not started. Running list maintained in `TSW_memory.md` until this section is opened.

---

## Discovery Inputs (from session 2026-06-24 / 2026-06-25)

> Organized into fourteen areas. Phase flags added 2026-06-25.

### A — Navigation & Shell *(Phase 1)*
Four tabs: Today / Horizon / Me / Shortcuts. Me menu Phase 1: four domains + About Me (Clint) + Key Docs + Big Ass Calendar + Quarterly Habit + Journal + Tools + Spec Sheet. Phase 2 additions: People Around Me (interactive family profiles) + Family Key Docs.

### B — Day Mode *(Phase 1)*
Focus / Buffer / Free. Kompass suggests from three signals. Uh-huh / uh-uh confirmation. Day Mode Log auto-created in Journals table via automation on confirmation.

### C — Horizon Rings *(Phase 1)*
Five rings. Three SmartSuite sources (Check List Tasks + Notes & Comments + GYR follow-ups). Max 7–10 items. Stacked + Circles dual-mode. Sacral Anchor. Quick Clear. Phase Context Strip.

### D — Kompass Operating Platform *(Phase 1)*
Three layers: Second Brain / Buffer Anchor / Genius Schedule. Nine-type capture routing. `Who` privacy field. Weekly Monday audit.

### E — Universal Goal Engine *(Phase 1)*
Five steps. Same template for all Goal types (Standard / Quarterly Habit / Misogi / Kevin's Rule). Domain-specific fields via `type` tag.

### F — Shortcuts Tab *(Phase 1)*
19 personal + 17 business Claude skills + 8 external tools.

### G — In-App Spec Sheet *(Phase 1)*
10 sections, 60+ rows. Tap-to-advance status. Tap-to-edit cells.

### H — About Me *(Phase 1: Clint only)*
4 GitHub files. Daily rotating reminder from Vivid Vision / Annual Commitments. Phase 2: family profiles become interactive.

### H1 — Vivid Vision & Annual Commitments *(Phase 1)*
GitHub as source of truth (confirmed 2026-06-25). App renders markdown via GitHub API. Annual update prompt each New Year.

### I — Big Ass Calendar *(Phase 1)*
Year-at-a-glance using BAC tables in The Stitser Way SmartSuite solution. Misogi + Kevin's Rule + Quarterly Habit milestones. Backward + forward layers. Surfaces on Free Day.

### J — Quarterly Habit *(Phase 1)*
Goal record with `type = "Quarterly Habit"`. One at a time. Five-stage arc. Staged learning. Celebration mechanics.

### K — Key Docs *(Phase 1: Clint only)*
JSON config → Supabase Phase 2. Drive links open natively. Nine categories. Emergency flag.

### L — Container Model & Learning Engine *(Phase 1)*
Three-layer empty state. Claude-guided build. Ebbinghaus / Calmio model. 6-hour safety rail.

### M — Brand Identity & Positioning
Stitser Way (working name). Sage-Architect-Builder. Austrian fire tradition / Kronerer developing. Full detail in `03-stitser-way/messaging.md`.

### N — Project-Based Tool Layer *(Phase 1)*
Four pillars (Budget / Alignment / Schedule / Checklists) — existing SB Project MGT tables confirmed. Master → Child → Grandchild hierarchy. Claude-built tools → Create → Active → Complete → Archived. Archive searchable and reusable.
