# Product Design Doc: Stitser Way

> **Status:** 🔄 In Progress — Gate 2 ✅ COMPLETE. Gate 3 (§4 Features + §5 Workflows) next.
> **Methodology:** Ryan Falke's Design Templates, adapted for Stitser Way
> **Decision-maker:** Clint Stitser
> **Last updated:** 2026-06-25

---

## Gate System

| Gate | Sections | Status |
|---|---|---|
| Gate 1 | §1 Problem + §2 Users | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 2 | §3 Core Entities | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 3 | §4 Features + §5 Workflows | ⏳ Next |
| Gate 4 | §6 Scope + §7 Metrics + §8 Timeline + §9 Open Questions | ⏳ Not started |
| ✅ PDD Done | All gates passed | Data Integration Doc + Tech Spec + UI/UX Doc can begin |

---

## Phase Model

> **Confirmed 2026-06-25.** Phase 1 is for Clint only. Family is entirely Phase 2.

| Phase | Users | What gets built |
|---|---|---|
| **Phase 1** | Clint only | Full personal OS — all four domains, rituals, Goal engine, Horizon Rings, Day Mode, Spiral, Big Ass Calendar, Quarterly Habit, Key Docs, Projects, Tools, Health data layer |
| **Phase 2** | Family members | Each family member gets their own profile, data, and experience. Built on the validated Phase 1 machine. |

**Table Talk nuance:** Phase 1 — Clint records the conversation. Phase 2 — per-member entry.
**About Me nuance:** Clint's 4 GitHub files are Phase 1. Family profiles are read-only in Phase 1, interactive in Phase 2.

---

## §1 — Problem Statement

### The problem

Most people who believe in intentional living have the same experience: the knowledge exists, the desire exists, and the tools exist — but they live in pieces. A habit tracker here. A journal there. Goals in a spreadsheet. Reminders on a sticky note. Coaching in a chat window. Training data in an app that doesn't talk to anything else. And at the center of it all, a person who has to manually hold the whole system together in their head every day.

For Clint, this produces four compounding problems:

**1. Fragmentation.** The infrastructure exists — SmartSuite holds the goals and GYR data, Claude runs the rituals and stacks, Google Calendar holds the year, Strava holds the training, GitHub holds the family profiles. But none of these surfaces know what the others are doing. There is no single place where the full picture of a life is visible, connected, and actionable.

**2. Absence.** There is no application designed around the actual frameworks and principles of intentional living — the GYR Spiral, the four life domains, phase-based accountability, the Container Model, spaced learning, the day-mode model, Brynn's Table Talk ritual, the Vivid Vision, the Misogi, the Quarterly Habit arc. These frameworks exist in Clint's operating system. They do not exist as a coherent, daily-use product.

**3. The activation gap.** Even with the knowledge and the frameworks, sustained installation requires a daily machine. Without it, great insights fade. Goals go untracked. Rituals drift. The GYR Spiral runs once, then gets forgotten. The Vivid Vision is written in January and not looked at until December. The knowledge is there — but there is no system that installs it into the body, the habits, and the daily rhythm, one concept at a time, until it becomes identity.

**4. No project-level tool layer.** Life is not just habits and domains — it contains bounded projects with specific, temporary needs. A trip to Europe needs a budget. A child's ear infection needs a medication schedule. A test needs a study app. Today, these tools are either cobbled together manually, outsourced to generic apps, or not built at all.

### The solution in one sentence

> *"The app is a machine that procedurally produces a better life. You don't need to arrive fully formed. The machine installs you into clarity over time."*

### What a good solution looks like

A single application that integrates the entire practice of intentional living — principles, tools, rituals, tracking, coaching, family coordination, health data, and project-level tool-building — into one coherent daily experience. One that:

- Surfaces the right information at the right time based on day type, domain status, and what's alive
- Installs frameworks and knowledge through staged, spaced learning — not information dumps
- Connects every domain of life (Body, Being, Balance, Business) to goals, phases, habits, and progress in one place
- Integrates Claude natively as the coaching, processing, suggestion, and tool-building layer
- Allows Claude to build custom tools for bounded projects — scoped to the project pillar, archived on completion
- Serves Clint first, and by extension his family

### What this is NOT

- Not a task manager — tasks are one input among many
- Not a journal app — journaling is one output among many
- Not a replacement for S-BOS — that is the business operating system; this is the personal one
- Not a collection of tools glued together — it is a machine with a single coherent logic
- Not finished when the data is entered — it is always working, always suggesting, always tending

---

## §1 Gate 1 Checklist

- [x] Problem clearly stated — ✅ Approved by Clint 2026-06-25
- [x] Solution criteria stated — ✅ Approved by Clint 2026-06-25
- [x] Scope boundaries stated — ✅ Approved by Clint 2026-06-25

---

## §2 — Target Users

### Phase 1 user: Clint Stitser (only)

- Personal operating system, daily driver. Four domains: Body, Being, Balance, Business.
- Operating manual profile: interest-based nervous system, object permanence challenges, consecutive appetite, Channel of Inspiration (1-8), non-linear thinking is a structural advantage.

### Phase 2 users: Family members

> ⏳ Entirely Phase 2. Architecture supports expansion.

Christie (Partner), Avery, Brynn, Max (Children). Kids enter their own data. Customized to each person's wiring.

### Phase 1 user needs by domain

| Domain | Phase 1 need |
|---|---|
| Body | Weight, body fat, meals, alcohol, sleep, readiness, training (Strava), health records. Phase-based protocol. Staged learning. |
| Being | Rituals, stacks, decisions, mindset, Spiral processing. |
| Balance | Table Talk (Clint records). Family profiles as read-only context. Relationship measurables tracked via Stat + Goal engine with Claude inference. |
| Business | Personal goals. Phase anchor for allocator seat. GYR per product line. |

---

## §2 Gate 1 Checklist

- [x] All user types identified — ✅ Approved by Clint 2026-06-25
- [x] Primary user needs per domain stated — ✅ Approved by Clint 2026-06-25
- [x] Family profile model confirmed — ✅ Approved by Clint 2026-06-25

---

## §3 — Core Entities

> **Scope:** Phase 1 only. All SmartSuite app IDs confirmed by live schema pull 2026-06-25.

### Entity Map

| # | Entity | Phase | Source | App ID / Location |
|---|---|---|---|---|
| 1 | Goal | 1 | Game App | `6824d4d1885a8769bd2dfc0d` |
| 2 | Priority | 1 | Game App | `68216f48f98789b5bb095a4b` |
| 3 | Milestone | 1 | Game App | `68216f48f98789b5bb095a37` |
| 4 | Stat | 1 | Game App | `6840927ebcfa2d2bfef039e2` |
| 5 | Stat Menu Item | 1 | Game App | `68409420391d32d925740e28` |
| 6 | GYR Status Report | 1 | Game App | `68216f48f98789b5bb095a51` |
| 7 | Task | 1 | Three sources | multiple — see definition |
| 8 | Journal Entry | 1 | Stitser Way | `68f8f8fe3757414d70d94ae0` |
| 9 | Day Mode Log | 1 | Stitser Way (automation) | `68f8f8fe3757414d70d94ae0` |
| 10 | Decision | 1 | Stitser Way | `68feda0035fd19c93de8d757` |
| 11 | Principle / Realization | 1 | Stitser Way | `6913f7e0e36376fca2b14b0e` |
| 12 | BAC Day Type | 1 | Stitser Way | `69458768a624db0406935efc` |
| 13 | BAC Calendar Event | 1 | Stitser Way | `6945877b88051cf9ac527e8a` |
| 14 | BAC Goal | 1 | Stitser Way | `69458793cc79c051739c047b` |
| 15 | Quarterly Habit | 1 | Game App Goals (tagged) | `6824d4d1885a8769bd2dfc0d` |
| 16 | Misogi | 1 | Game App Goals (tagged) | `6824d4d1885a8769bd2dfc0d` |
| 17 | Kevin's Rule Event | 1 | Game App Goals (tagged) | `6824d4d1885a8769bd2dfc0d` |
| 18 | Project | 1 | SB Biz Dev | `68216a706900e8eaf75a05a7` |
| 19 | Check List | 1 | SB Project MGT | `6a060dadc513b3329b7d4485` |
| 20 | Check List Task | 1 | SB Project MGT | `68a8e17251dc814e8c529f3f` |
| 21 | Project Budget — G702 | 1 | SB Project MGT | `68a8c3d2bba73ca6e62d00f0` |
| 22 | Project Budget Line Items — G703 | 1 | SB Project MGT | `68db71a363e88ace0bd45439` |
| 23 | Baseline Budget Items | 1 | SB Project MGT | `69bb89ebf6a195c2c73a3b3e` |
| 24 | Bills & Invoices | 1 | SB Project MGT | `68a8c3d2bba73ca6e62d1297` |
| 25 | Project Schedule | 1 | SB Project MGT | `68a8d2153c056ca71c9928fd` |
| 26 | Project Dates | 1 | SB Project MGT | `69bb7d64740e0e696d88c47f` |
| 27 | Notes & Comments | 1 | SB Biz Dev | `6894e64f621641b3ef90fa99` |
| 28 | Project Tool | 1 | Claude artifact + SmartSuite link | n/a |
| 29 | Strava Activity | 1 | Strava MCP | external — read-only |
| 30 | Oura Sleep Record | 1 | Oura API | external — read-only |
| 31 | Oura Readiness Record | 1 | Oura API | external — read-only |
| 32 | Apple Health Weight Record | 1 | Apple Health API | external — read-only |
| 33 | Body Scan Record | 1 | Google Drive link | DXA/DEXA — Drive link + date |
| 34 | Bloodwork Record | 1 | Google Drive link | Lab report — Drive link + date |
| 35 | Eye Prescription Record | 1 | Google Drive link | Prescription — Drive link + date |
| 36 | Vivid Vision | 1 | GitHub (source of truth) | `01-user-profile/vivid-vision-2036.md` |
| 37 | Annual Commitments | 1 | GitHub (source of truth) | `01-user-profile/2026-commitments.md` |
| 38 | Clint's Profile | 1 | GitHub | `01-user-profile/` |
| 39 | Family Profile | 1 (read-only) / 2 | GitHub | `07-family/` |
| 40 | Key Doc | 1 | JSON config → Supabase | n/a |
| 41 | Lesson | 1 | SB Training & Certifications | `68d480e2727607560a7f0d26` |
| 42 | Course | 1 | SB Training & Certifications | `68d480e2727607560a7f0d2c` |
| 43 | Learning Track | 1 | SB Training & Certifications | `68d480e2727607560a7f0d23` |
| 44 | Progress Record | 1 | SB Training & Certifications | `6a18ad82e630be8e82a202ea` |

---

### Key Clarifications

**Goals — universal tag-based entity:** Quarterly Habit, Misogi, Kevin's Rule are Goal records with a `type` tag. Values: Standard Goal / Quarterly Habit / Misogi / Kevin's Rule.

**Tasks — three source tables:** Check List Tasks + Notes & Comments + GYR Status Reports (follow-up owner = Clint, Red/Yellow, not completed).

**Day Mode → Journal automation:** "Day Mode Log" entry auto-created after confirmation. Enables pattern tracking over time.

**Daily Reminder Engine:** "A Thought For You" on Today tab. Draws from Vivid Vision lines + Annual Commitment lines + Principles & Realizations records. One per day, never repeats two days running.

**Bills & Invoices — Phase 1 confirmed:** Actively used for Europe trip budget tracking.

**Health data — two integration patterns:**
- *Live API reads (no SmartSuite storage):* Oura (sleep + readiness — primary), Apple Health (weight — primary), Strava (training).
- *Historical Drive links (date + notes + URL):* Body Scan / DXA, Bloodwork, Eye Prescription — stored in Body domain Health Vault.

**Source of truth rule (confirmed 2026-06-25):** Oura primary for sleep + readiness. Apple Health primary for weight.

**Health Vault:** Body domain section surfacing all Drive-linked health records chronologically. One-tap Drive access. Add via simple form: type → date → Drive URL → notes.

**Relationship measurables — no new entities needed:**
Balance domain Goals like "Rides with Max," "Dates with Christie" use the existing Stat + Goal engine. Logged via the Stat Inference Engine (Discovery Input O) — no forms.

**Skill/Habit Installation Arc — SB Training & Certifications `68d480e2727607560a7f0d22`:**
The data store for tracking progression from conscious incompetence to subconscious competence. Four tables, all confirmed live:

| Entity | Table | Role in the arc |
|---|---|---|
| **Lesson** (#41) | Lesson Catalog `68d480e2727607560a7f0d26` | Atomic learning unit — one concept, delivered as a Calmio-style card in the app. One per day, 6-hour safety rail. |
| **Course** (#42) | Courses `68d480e2727607560a7f0d2c` | A structured sequence of Lessons for one phase of the arc (e.g., the "Foundation" phase of the Body protocol). |
| **Learning Track** (#43) | Learning Tracks/Certifications `68d480e2727607560a7f0d23` | The full installation arc for a skill or habit — from Install through Subconscious Competence. A Learning Track contains multiple Courses. |
| **Progress Record** (#44) | Progress Table `6a18ad82e630be8e82a202ea` | Clint's live position in any Learning Track — which lesson he's on, which phase, completion percentage, date started. |

**How the arc maps to Phases of Proficiency:**

```
Learning Track = "Body Protocol: Fat Loss & MTB Performance"
  Course 1 = Foundation Phase    (Install → Beginner)
    Lesson 1 = Why sleep outperforms exercise for fat loss
    Lesson 2 = The cortisol/testosterone triangle
    Lesson 3 = Why alcohol is the primary visceral fat driver
    ...
  Course 2 = Engine Phase        (Intermediate → Expert)
  Course 3 = Race Block Phase    (Expert → Subconscious Competence)
```

**External learning excluded (confirmed 2026-06-25):** Only in-app lesson completion is tracked in the Progress Record. Books, external apps, coaches, courses — not logged here. The Progress Record reflects what the machine delivered and Clint completed inside Stitser Way.

**Cross-product note:** SB Training & Certifications is also the data store for S-BOS team member skill installation (Pay App, Compliance Audit, etc.). The same four tables serve both products. Stitser Way uses it for personal domain skills and habits; S-BOS uses it for business operational skills.

**Vivid Vision + Annual Commitments:** GitHub source of truth. App reads via GitHub API. Also feed Daily Reminder Engine.

**BAC infrastructure already live:** BAC-Day Types `69458768a624db0406935efc`, BAC-Calendar Events `6945877b88051cf9ac527e8a`, BAC-Goals `69458793cc79c051739c047b`.

### §3 Gate 2 Checklist

- [x] All Phase 1 entities named — ✅ Approved by Clint 2026-06-25
- [x] No orphan entities — ✅ Approved by Clint 2026-06-25
- [x] Phase 1 / Phase 2 boundary clear — ✅ Approved by Clint 2026-06-25

> **Note:** Four entities (41–44) added post-approval. These were missing from the original Gate 2 submission. Gate 2 remains closed — these additions are clarifications, not scope changes. Flag for review if Clint disagrees.

---

## §4 — Core Features

> ⏳ Not started. Gate 2 complete — ready to begin.

---

## §5 — User Workflows

> ⏳ Not started. Begins after §4.

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

### A — Navigation & Shell *(Phase 1)*
Four tabs: Today / Horizon / Me / Shortcuts. Me menu Phase 1: four domains + About Me + Key Docs + Big Ass Calendar + Quarterly Habit + Journal + Tools + Spec Sheet.

### B — Day Mode *(Phase 1)*
Focus / Buffer / Free. Kompass suggests. Uh-huh / uh-uh. Day Mode Log auto-created via automation.

### C — Horizon Rings *(Phase 1)*
Five rings. Three SmartSuite sources. Max 7–10 items. Stacked + Circles. Sacral Anchor. Quick Clear. Phase Context Strip.

### D — Kompass Operating Platform *(Phase 1)*
Three layers: Second Brain / Buffer Anchor / Genius Schedule. Nine-type capture routing (→ Decisions, → Principles & Realizations). `Who` privacy field.

### E — Universal Goal Engine *(Phase 1)*
Five steps. `type` tag differentiates all Goal sub-types.

### F — Shortcuts Tab *(Phase 1)*
19 personal + 17 business Claude skills + 8 external tools.

### G — In-App Spec Sheet *(Phase 1)*
10 sections, 60+ rows. Tap-to-advance. Tap-to-edit.

### H — About Me *(Phase 1: Clint only)*
4 GitHub files. Daily Reminder Engine. Phase 2: family profiles interactive.

### H1 — Vivid Vision & Annual Commitments *(Phase 1)*
GitHub source of truth. Annual update prompt. Feed Daily Reminder Engine.

### I — Big Ass Calendar *(Phase 1)*
BAC tables in Stitser Way solution. Misogi + Kevin's Rule + Quarterly Habit. Backward + forward layers. Surfaces on Free Day.

### J — Quarterly Habit *(Phase 1)*
Goal tagged `type = "Quarterly Habit"`. Five-stage arc. Staged learning via Learning Track in SB Training & Certifications.

### K — Key Docs *(Phase 1: Clint only)*
JSON config → Supabase. Drive links open natively. Nine categories. Emergency flag.

### L — Container Model & Learning Engine *(Phase 1)*
Three-layer empty state. Claude-guided build. Ebbinghaus / Calmio model. 6-hour safety rail. Data store: SB Training & Certifications — Lesson / Course / Learning Track / Progress Record (entities 41–44).

### M — Brand Identity & Positioning
Stitser Way (working name). Sage-Architect-Builder. Austrian fire / Kronerer developing. Full detail in `03-stitser-way/messaging.md`.

### N — Project-Based Tool Layer *(Phase 1)*
Four pillars — SB Project MGT tables confirmed including Bills & Invoices. Master → Child → Grandchild. Claude-built tools. Archive reusable.

### O — Stat Inference Engine *(Phase 1)*

**What it is:** Claude reads existing data sources (Strava, journal captures, calendar entries) after events occur and surfaces smart, one-tap confirmation prompts to log stats against active Goals — without requiring Clint to fill out forms.

**The core principle:** The app is a second brain. It notices what happened and asks one question. Clint confirms or dismisses. No forms. No manual entry. No friction.

**How it works — three signal sources:**

1. **Strava** — new activity detected → checks for matching active Balance Goal → surfaces prompt
2. **Journal / Capture** — person name + activity keyword found → matches to Goal → surfaces prompt
3. **Calendar** — completed event with named person + activity type → matches to Goal → surfaces prompt

**The prompt format (one-tap, never a form):**

> *"Hey — you just logged a 12-mile ride. Was Max with you? Tap yes to count it toward 'Rides with Max Q3' (3 of 10)."*

> *"I saw a note about driving with Avery to her game. Want me to log that against 'Drives with Avery' (7 of 20)?"*

> *"Looks like you had a date night with Christie. Log it against 'Monthly Dates with Christie' (2 of 3 this month)?"*

**Response options:** Yes — log it / No — skip / Not quite (one clarifying question max, never a form)

**Entities touched:**
- Reads: Strava Activity (#29), Journal Entry (#8), BAC Calendar Event (#13), Goal (#1), Stat Menu Item (#5)
- Writes: Stat (#4) — one new record per confirmed log

**Not automatic** — Claude always asks, Clint always confirms. Full feature spec in §4.
