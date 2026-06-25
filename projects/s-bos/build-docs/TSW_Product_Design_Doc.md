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

**Design principle (Phase 2):** Kids enter their own data. The app is intrinsically motivating — not parent-assigned homework. Customized to each family member's wiring and learning style.

### Phase 1 user needs by domain

| Domain | What Clint needs the app to do in Phase 1 |
|---|---|
| Body | Track weight, body fat, meals, alcohol, training rides (Strava), health records. Phase-based protocol (Foundation → Engine → Race Block). Staged learning connected to the why. |
| Being | Morning/evening rituals, stacks, affirmations, decisions, mindset tracking. Spiral processing. Phase gate for being/mindset development. |
| Balance | Table Talk (Clint records the dinner conversation). Family profiles readable by Clint as reference context. Relationship coaching via Claude. |
| Business | Personal business goals — separate from S-BOS team goals. Phase anchor for Clint's role as allocator/CEO. GYR status per product line from his personal vantage point. |

---

## §2 Gate 1 Checklist

- [x] All user types identified — ✅ Approved by Clint 2026-06-25
- [x] Primary user needs per domain stated — ✅ Approved by Clint 2026-06-25
- [x] Family profile model confirmed — ✅ Approved by Clint 2026-06-25

---

## §3 — Core Entities

> **Scope:** Phase 1 entities only — what the app needs to read, write, and reason about to serve Clint. Family entities are noted as Phase 2 where relevant but not defined here.

---

### Entity Map

| # | Entity | Phase | Source | Description |
|---|---|---|---|---|
| 1 | Goal | 1 | SmartSuite | Top-level initiative within a life domain |
| 2 | Priority | 1 | SmartSuite | Phase-level work chunk within a Goal |
| 3 | Milestone | 1 | SmartSuite | Discrete sub-step within a Priority |
| 4 | Stat | 1 | SmartSuite | A logged data point (weight, drinks, rides, etc.) |
| 5 | Stat Menu Item | 1 | SmartSuite | The catalog of measurable types available for logging |
| 6 | GYR Status Report | 1 | SmartSuite | A Spiral processing session for a Goal or Priority |
| 7 | Task | 1 | SmartSuite | A discrete action item (Check List Tasks or Notes follow-ups) |
| 8 | Journal Entry | 1 | SmartSuite | A completed ritual, stack, Table Talk, or Spiral record |
| 9 | Quarterly Habit | 1 | SmartSuite | The one active habit being installed this quarter |
| 10 | Misogi | 1 | SmartSuite + Calendar | The year-defining event |
| 11 | Kevin's Rule Event | 1 | Google Calendar | A bimonthly adventure slot |
| 12 | Key Doc | 1 | JSON config → Supabase | A named Google Drive link for a critical document |
| 13 | Project | 1 | SmartSuite S-BOS | A bounded initiative (Master / Child / Grandchild) |
| 14 | Project Tool | 1 | Claude artifact + SmartSuite | A Claude-built mini-app attached to a project pillar |
| 15 | Clint's Profile | 1 | GitHub Clint-s-Kompass | Operating manual, quick reference, vivid vision, commitments |
| 16 | Family Profile | 1 (read-only) / 2 (interactive) | GitHub Clint-s-Kompass | Christie, Avery, Brynn, Maxwell, Gwen — readable by Clint in Phase 1 |
| 17 | Vivid Vision | 1 | Google Drive + GitHub | 10-year ideal circumstances document |
| 18 | Annual Commitments | 1 | Google Drive + GitHub | 1-year measurable commitment document |
| 19 | Day Mode | 1 | App state | Current day operating posture: Focus / Buffer / Free |

---

### Entity Definitions

---

#### 1. Goal
The top-level initiative within a life domain. A Goal has a target, a deadline, and tracks progress over time through Stats and GYR Status Reports.

| Field | Type | Notes |
|---|---|---|
| Title | Text | The goal name |
| Domain | Select | Body / Being / Balance / Business |
| GYR Status | Select | Green / Yellow / Red |
| Target | Number | What success looks like numerically |
| % Metric Complete | Formula | SmartSuite field `sc9f2f3411` |
| % Time Complete | Formula | SmartSuite field `sbc4aa3064` |
| Due Date | Date | SmartSuite field `s65ce469a2` |
| Reporting Grade | Select | SmartSuite field `s9c754688f` |
| Linked Priorities | Relation | Child Priority records |
| Linked GYR Reports | Relation | GYR Status Report records |

**Phase 2 addition:** `owner` field — which family member this Goal belongs to.

---

#### 2. Priority
A phase-level work chunk within a Goal. The active initiative for a given time window.

| Field | Type | Notes |
|---|---|---|
| Title | Text | |
| GYR Status | Select | Green / Yellow / Red |
| Target | Number | |
| % Metric Complete | Formula | |
| % Time Complete | Formula | |
| Due Date | Date | |
| Linked Goal | Relation | Parent Goal |
| Linked Milestones | Relation | Child Milestone records |
| Linked GYR Reports | Relation | |

---

#### 3. Milestone
A discrete, completable sub-step within a Priority.

| Field | Type | Notes |
|---|---|---|
| Title | Text | |
| Due Date | Date | |
| Complete | Checkbox | |
| Linked Priority | Relation | Parent Priority |

---

#### 4. Stat
A logged data point against a Goal or Priority. The raw data that feeds progress tracking.

| Field | Type | Notes |
|---|---|---|
| Date | Date | When the stat was logged |
| Amount | Number | SmartSuite field `s6471266f2` |
| Stat Type | Relation | Links to Stat Menu Item |
| Linked Goal | Relation | |
| Linked Priority | Relation | |
| Period | Select | Monthly / Weekly |

---

#### 5. Stat Menu Item
The catalog of measurable types available for logging. Defines what can be tracked.

| Field | Type | Notes |
|---|---|---|
| Name | Text | e.g., Weight, Alcohol, TSS, Waist Circumference |
| Summary Type | Select | Sum / Average / Latest |
| Linked Goals | Relation | |
| Linked Priorities | Relation | |

---

#### 6. GYR Status Report
A Spiral processing session for a Goal or Priority. The transformation engine record — facts through fruit.

| Field | Type | Notes |
|---|---|---|
| Title | Text | |
| Domain | Select | Body / Being / Balance / Business |
| GYR Grade | Select | Green / Yellow / Red |
| Facts | Long text | What is actually true |
| Feelings | Long text | Emotional response to the facts |
| Root Cause | Long text | What's actually driving the situation |
| Focus | Long text | The one thing to change |
| Actions | Long text | Massive, relevant actions |
| Fruit | Long text | Expected outcome |
| Follow-Up Owner | Person | Assigned to Clint (Phase 1) |
| Follow-Up Date | Date | When to review |
| Linked Goal | Relation | |
| Linked Priority | Relation | |

---

#### 7. Task
A discrete action item. Sourced from SmartSuite Check List Tasks or Notes & Comments follow-ups. Surfaces in Horizon Rings.

| Field | Type | Notes |
|---|---|---|
| Title | Text | Clear action statement |
| Due Date | Date | |
| Assigned To | Person | Clint (Phase 1) |
| Status | Select | Open / In Progress / Complete / Cancelled |
| Who | Person | Privacy field — only populated when task genuinely involves another person |
| Linked Project / Goal / Priority | Relation | Context anchor |
| Source | Select | Task / Notes & Comments follow-up / GYR follow-up |

---

#### 8. Journal Entry
A completed ritual, stack, Table Talk, or Spiral session — the permanent record of processing.

| Field | Type | Notes |
|---|---|---|
| Title | Text | Auto-generated from type + date |
| Type | Select | Morning Ritual / Evening Ritual / WAR Stack / Cash Stack / Irritation Stack / Anger Stack / Guilt Stack / Gratitude Stack / Excitement Stack / Discovery Stack / Spiral / Table Talk / Free Write / Project Debrief / Strategic Vision / Weekly Review |
| Date | Date | |
| Domain | Select | Body / Being / Balance / Business / All |
| Content | Long text | Full session content |
| SmartSuite App | Fixed | `68f8f8fe3757414d70d94ae0` |

**Phase 2 addition:** `family_member` field — who recorded this entry.

---

#### 9. Quarterly Habit
The one active habit being installed this quarter. Consecutive Appetite model — one at a time.

| Field | Type | Notes |
|---|---|---|
| Title | Text | The habit name and identity statement |
| Domain | Select | Body / Being / Balance / Business |
| Stage | Select | Install / Beginner / Intermediate / Expert / Complete |
| Start Date | Date | Quarter start |
| End Date | Date | Quarter end |
| Streak | Number | Current consecutive days |
| Target Frequency | Select | Daily / 3x week / Weekly |
| Linked Goal | Relation | The Goal this habit serves |

*Stored as a Goal record with type = "Quarterly Habit" in SmartSuite.*

---

#### 10. Misogi
The year-defining event — slightly terrifying, deeply personal. One per year.

| Field | Type | Notes |
|---|---|---|
| Title | Text | |
| Date | Date | The event date |
| Domain | Select | Body / Being / Balance / Business |
| Description | Long text | What it is, why it matters, what it requires |
| Status | Select | Planned / In Preparation / Complete |
| Google Calendar Link | URL | Event link |

*Stored as a Goal record with type = "Misogi" in SmartSuite + Google Calendar event.*

---

#### 11. Kevin's Rule Event
A bimonthly adventure — one new experience every other month. Six slots per year.

| Field | Type | Notes |
|---|---|---|
| Title | Text | |
| Date | Date | |
| Month Slot | Number | 1–6 (which bimonthly slot this fills) |
| Description | Long text | What it is, why it was new |
| Status | Select | Planned / Complete |
| Google Calendar Link | URL | |

*Stored as Google Calendar events with a "Kevin's Rule" tag. Phase 1: manual entry. Phase 2: structured SmartSuite record.*

---

#### 12. Key Doc
A named Google Drive link for a critical personal document. Not file storage — a link registry.

| Field | Type | Notes |
|---|---|---|
| Name | Text | Human-readable document name |
| Category | Select | Identity / Health / Legal / Financial / Property / Education / Vehicle / Travel / Other |
| Drive URL | URL | Opens Drive natively — no API auth |
| Emergency Flag | Boolean | Surfaces first in a crisis |
| Owner | Select | Clint (Phase 1) / family member (Phase 2) |

*Phase 1: stored as structured JSON config. Phase 2: Supabase.*

---

#### 13. Project
A bounded initiative with a defined scope and lifecycle. Uses the existing S-BOS SmartSuite project infrastructure.

| Field | Type | Notes |
|---|---|---|
| Title | Text | |
| Level | Select | Master / Child / Grandchild |
| Domain | Select | Body / Being / Balance / Business |
| Status | Select | Active / Complete / Archived |
| Active Pillar | Select | Budget / Alignment / Schedule / Checklists |
| Linked Parent | Relation | Parent Project (null for Master) |
| Linked Tools | Relation | Claude-built Project Tools |

---

#### 14. Project Tool
A Claude-built custom mini-app, tracker, or checklist attached to a project pillar.

| Field | Type | Notes |
|---|---|---|
| Title | Text | |
| Type | Select | Budget tracker / Study app / Scheduler / Checklist / Team planner / Calculator / Reference |
| Pillar | Select | Budget / Alignment / Schedule / Checklists |
| Lifecycle Stage | Select | Create / Active / Complete / Archived |
| Linked Project | Relation | Parent Project |
| Artifact URL | URL | Link to Claude-built HTML artifact |
| Created Date | Date | |
| Archived Date | Date | |

---

#### 15. Clint's Profile
Clint's personal operating manual and context files — the document set that defines how he works, what he's building, and who he is.

| File | Contents | Source |
|---|---|---|
| `operating-manual.md` | Human Design, ADHD profile, cognitive mechanics, decision-making protocol | GitHub Clint-s-Kompass |
| `quick-reference.md` | Condensed key reminders | GitHub Clint-s-Kompass |
| `vivid-vision-2036.md` | 10-year ideal circumstances | GitHub Clint-s-Kompass |
| `2026-commitments.md` | Current year measurable commitments | GitHub Clint-s-Kompass |

*Read-only by the app via GitHub API. Not a SmartSuite entity.*

---

#### 16. Family Profile (Phase 1: read-only reference)
Profile files for Christie, Avery, Brynn, Maxwell, and Gwen. In Phase 1, these are readable by Clint as context for coaching and relationship navigation — not interactive, not writeable by family members.

*Phase 2: each family member gets their own auth and can read/write their own profile.*

---

#### 17. Vivid Vision
The 10-year ideal circumstances document. Surfaces inside Clint's profile section. Opens natively via Google Drive link.

| Field | Notes |
|---|---|
| Google Doc ID | `1KpYWZdRgeM93V79mp0sSStE57y-f9iWKwRu5pyYcIsI` |
| GitHub backup | `01-user-profile/vivid-vision-2036.md` |
| Sections | Health / Environment / Family / Business / Wealth & Legacy |
| Review cadence | Annual — New Year prompt |

---

#### 18. Annual Commitments
The 1-year measurable commitment document. Surfaces inside Clint's profile. Opens natively via Google Drive link.

| Field | Notes |
|---|---|
| Google Doc ID | Same doc as Vivid Vision — `1KpYWZdRgeM93V79mp0sSStE57y-f9iWKwRu5pyYcIsI` |
| GitHub backup | `01-user-profile/2026-commitments.md` |
| Sections | Rituals / Body / Being / Balance / Business |
| Review cadence | Annual renewal + quarterly check-in |
| Connection | Measurable commitments link to SmartSuite Goals in the corresponding domain |

---

#### 19. Day Mode
The current day's operating posture. App state — not a persisted SmartSuite record in Phase 1.

| Field | Notes |
|---|---|
| Mode | Focus / Buffer / Free |
| Date | Today |
| Set by | Kompass suggestion (confirmed) or manual override |
| Confirmed at | Timestamp |

**Open question (flag for §9):** Should Day Mode be logged to Journals so Clint can see his day-type patterns over time (e.g., how many Focus Days last month)? Low-cost addition with potential scoreboard value. Decide before Technical Spec.

---

## §3 Gate 2 Checklist

- [ ] All Phase 1 entities named ✳️ *Pending sign-off*
- [ ] No orphan entities — every entity referenced in Discovery Inputs is defined here ✳️ *Pending sign-off*
- [ ] Phase 1 / Phase 2 boundary clear for every entity ✳️ *Pending sign-off*

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

---

### A — Navigation & Shell *(Phase 1)*

- Four persistent bottom tabs: **Today / Horizon / Me / Shortcuts**
- Me tab is a domain menu — four domain cards (Body/Being/Balance/Business), tap domain → Goals list, tap Goal → universal initiative screen
- **Me menu sections (Phase 1):** Body / Being / Balance / Business + About Me (Clint) + Key Docs + Big Ass Calendar + Quarterly Habit + Journal + Tools + Spec Sheet
- **Me menu sections (Phase 2 additions):** People Around Me (family profiles, interactive) + Family Key Docs + family Table Talk
- Spiral and Spec Sheet accessible from top-right nav icons on any screen
- Tab bar hides entirely in Focus Day mode

---

### B — Day Mode *(Phase 1)*

Three modes: Focus Day / Buffer Day / Free Day. Kompass suggests from three signals (calendar, Horizon Rings, recent energy). Clint confirms with uh-huh / uh-uh. Full spec in Discovery Input B above.

---

### C — Horizon Rings *(Phase 1)*

Five rings. Three SmartSuite data sources. Max 7–10 items. Dual-mode: Stacked list + Circles overview. Sacral Anchor. Quick Clear. Phase Context Strip. Full spec in Discovery Input C above.

---

### D — Kompass Operating Platform *(Phase 1)*

Three-layer architecture: Second Brain / Buffer Anchor / Genius Schedule. Full spec in Discovery Input D above.

---

### E — Universal Goal Engine *(Phase 1)*

Five steps: Current Score → Goal + Deadline → Rhythm & Reminders → Progress Tracking → Celebrate Wins. Full spec in Discovery Input E above.

---

### F — Shortcuts Tab *(Phase 1)*

19 personal Claude skills + 17 business Claude skills + 8 external tools. Full inventory in `TSW_Design_Context.md`.

---

### G — In-App Spec Sheet *(Phase 1)*

Living design doc inside the app. 10 sections, 60+ rows. Tap-to-advance status. Tap-to-edit cells.

---

### H — About Me *(Phase 1: Clint only)*

Clint's 4 GitHub profile files — full markdown render, scrollable. Daily rotating reminder from Vivid Vision / Annual Commitments tied to morning ritual.

**H1 — Vivid Vision & Annual Commitments *(Phase 1)*:** Opens natively via Google Drive link. Annual update prompt each New Year.

> **Phase 2:** Family profiles become interactive — each member reads and writes their own profile.

---

### I — Big Ass Calendar *(Phase 1)*

Year-at-a-glance. Misogi + Kevin's Rule + Quarterly Habit. Backward layer (look how far) + forward layer (what's coming). Surfaces on Free Day Today tab.

---

### J — Quarterly Habit *(Phase 1)*

One habit at a time. Five-stage arc. Freud's sense of achievement. Staged learning. Celebration mechanics.

---

### K — Key Docs *(Phase 1: Clint's docs only)*

Google Drive link registry. Links open natively. Nine categories. Emergency flag.

> **Phase 2:** Family member docs added. Per-member selector becomes interactive.

---

### L — Container Model & Learning Engine *(Phase 1)*

L1: Three-layer empty state (glow + invitation + progress ring). Claude-guided build flow.
L2: Ebbinghaus / Calmio model. One concept per card. 6-hour safety rail.

---

### M — Brand Identity & Positioning

Working name: Stitser Way. Sage-Architect-Builder archetype. Austrian fire tradition (Kronerer) as brand story developing. Full detail in `03-stitser-way/messaging.md`.

---

### N — Project-Based Tool Layer *(Phase 1)*

Four project pillars (Budget / Alignment / Schedule / Checklists). Universal across S-BOS and Stitser Way. Master → Child → Grandchild hierarchy. Claude-built tools attach to pillars. Create → Active → Complete → Archived lifecycle. Archive is searchable and reusable.

> **Phase 2:** Tool sharing across family members becomes interactive.
