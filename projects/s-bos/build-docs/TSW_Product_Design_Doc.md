# Product Design Doc: Stitser Way

> **Status:** 🔄 In Progress — Gate 2 ✅ COMPLETE. §4 Core Features in progress (18 features).
> **Methodology:** Ryan Falke's Design Templates, adapted for Stitser Way
> **Decision-maker:** Clint Stitser
> **Last updated:** 2026-06-25

---

## Gate System

| Gate | Sections | Status |
|---|---|---|
| Gate 1 | §1 Problem + §2 Users | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 2 | §3 Core Entities | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 3 | §4 Features + §5 Workflows | 🔄 In progress |
| Gate 4 | §6 Scope + §7 Metrics + §8 Timeline + §9 Open Questions | ⏳ Not started |
| ✅ PDD Done | All gates passed | Data Integration Doc + Tech Spec + UI/UX Doc can begin |

---

## Phase Model

| Phase | Users | What gets built |
|---|---|---|
| **Phase 1** | Clint only | Full personal OS — all four domains, rituals, Goal engine, Horizon Rings, Day Mode, Spiral, Big Ass Calendar, Quarterly Habit, Key Docs, Projects, Tools, Health data layer |
| **Phase 2** | Family members | Each family member gets their own profile, data, and experience. Built on the validated Phase 1 machine. |

---

## §1 — Problem Statement

### The problem

For Clint, four compounding problems:

**1. Fragmentation.** No single place where the full picture of a life is visible, connected, and actionable.

**2. Absence.** No application designed around the actual frameworks — GYR Spiral, four life domains, phase-based accountability, Container Model, spaced learning, day-mode model, Table Talk, Vivid Vision, Misogi, Quarterly Habit arc.

**3. The activation gap.** Without a daily machine, knowledge fades, rituals drift, the Vivid Vision is written once and forgotten.

**4. No project-level tool layer.** Bounded projects have no purpose-built tool system.

### The solution in one sentence

> *"The app is a machine that procedurally produces a better life. You don't need to arrive fully formed. The machine installs you into clarity over time."*

### What this is NOT
Not a task manager. Not a journal app. Not S-BOS. Not tools glued together. Not finished when data is entered.

---

## §1 Gate 1 Checklist
- [x] Problem clearly stated — ✅ Approved by Clint 2026-06-25
- [x] Solution criteria stated — ✅ Approved by Clint 2026-06-25
- [x] Scope boundaries stated — ✅ Approved by Clint 2026-06-25

---

## §2 — Target Users

**Phase 1:** Clint only. **Phase 2:** Christie, Avery, Brynn, Max.

| Domain | Phase 1 need |
|---|---|
| Body | Weight, body fat, meals, alcohol, sleep, readiness, training, health records. Phase-based protocol. |
| Being | Rituals, stacks, decisions, mindset, Spiral processing. |
| Balance | Table Talk (Clint records). Family profiles as read-only context. Relationship measurables via inference. |
| Business | Personal goals. Phase anchor for allocator seat. GYR per product line. |

---

## §2 Gate 1 Checklist
- [x] All user types identified — ✅ Approved by Clint 2026-06-25
- [x] Primary user needs per domain stated — ✅ Approved by Clint 2026-06-25
- [x] Family profile model confirmed — ✅ Approved by Clint 2026-06-25

---

## §3 — Core Entities

> 44 entities. Full definitions and app IDs in TSW_memory.md and prior commit history.

**Game App:** Goals (#1), Priorities (#2), Milestones (#3), Stats (#4), Stat Menu Items (#5), GYR Status Reports (#6)
**Stitser Way:** Journal Entries (#8), Day Mode Log (#9), Decisions (#10), Principles/Realizations (#11), BAC Day Types (#12), BAC Calendar Events (#13), BAC Goals (#14)
**Game App Goals tagged:** Quarterly Habit (#15), Misogi (#16), Kevin's Rule (#17)
**Tasks composite:** Check List Tasks + Notes & Comments + GYR follow-ups (#7)
**SB Project MGT:** Projects (#18–26), Notes & Comments (#27)
**Claude artifact:** Project Tool (#28)
**External APIs:** Strava (#29), Oura Sleep (#30), Oura Readiness (#31), Apple Health Weight (#32)
**Drive links:** Body Scan (#33), Bloodwork (#34), Eye Prescription (#35)
**GitHub:** Vivid Vision (#36), Annual Commitments (#37), Clint's Profile (#38), Family Profiles (#39)
**App config:** Key Doc (#40)
**SB Training & Certifications:** Lesson (#41), Course (#42), Learning Track (#43), Progress Record (#44)

---

## §3 Gate 2 Checklist
- [x] All Phase 1 entities named — ✅ Approved by Clint 2026-06-25
- [x] No orphan entities — ✅ Approved by Clint 2026-06-25
- [x] Phase 1 / Phase 2 boundary clear — ✅ Approved by Clint 2026-06-25

---

## §4 — Core Features

> **18 features.** Each spec: what it does, entities read/write, UX behavior, success criteria.

---

### F01 — Day Mode Engine

**What it does:** Determines the day's operating posture (Focus / Buffer / Free) and changes the app's behavior accordingly.

**Entities read:** BAC Calendar Events (#13), Task (#7), Day Mode Log (#9).
**Entities written:** Day Mode Log (#9).

**UX behavior:**
- Kompass suggests one mode with one-sentence reason on app open
- Clint: uh-huh (confirms) or uh-uh (three-option picker). Max two questions.
- **Focus Day:** Tab bar hides. Anchor + Pomodoro only. Break-glass available. Grounding affirmation shown.
- **Buffer Day:** Full app. Horizon Rings command center. Sub-tabs: Horizon / Email / Big 3. Guardrail questions on commitments.
- **Free Day:** Calendar + wins panel + Table Talk prompt. *"Your job today is to wander."* Kompass off.

**Success criteria:** Mode suggested < 3s. Log created on confirmation. Tab bar correct. Free Day shows no tasks/inbox.

---

### F02 — Horizon Rings

**What it does:** Surfaces the most important items from projects, goals, and follow-ups. Max 7–10 items.

**Entities read:** Check List Tasks (#20), Notes & Comments (#27), GYR Status Reports (#6), Goals (#1).
**Entities written:** Check List Tasks (#20) status. Stats (#4) Sacral Anchor.

**Five rings:** 🔴 Overdue / 🟡 This Week / 🔵 Active / 🟢 Coming Soon / ⚪ Parked

**UX behavior:** Priority stack order. Phase Context Strip. Stacked list default / Circles overview toggle. Sacral Anchor (top 3 → Clint selects → pinned). Quick Clear for stale items.

**Success criteria:** Never > 10 items. Toggle works without reload. Anchor persists all day.

---

### F03 — Stat Inference Engine

**What it does:** Claude scans Strava, journals, and calendar after events and surfaces one-tap prompts to log stats — no forms.

**Entities read:** Strava (#29), Journal Entry (#8), BAC Calendar Events (#13), Goal (#1), Stat Menu Item (#5).
**Entities written:** Stat (#4).

**UX behavior:** Matches event to Goal by person name + activity type. One prompt per match. Yes / No / Not quite (one clarifying question max). Prompts queue one at a time.

**Success criteria:** Match rate > 85%. Prompt < 5min. No multi-field forms. Goal % updates on confirm.

---

### F04 — Universal Goal Engine

**What it does:** Five-step framework for every initiative across every domain. All Goal types use this engine.

**Entities read/written:** Goal (#1), Priority (#2), Milestone (#3), Stat (#4), Stat Menu Item (#5), GYR Status Report (#6).

**Five steps:** Current Score → Goal + Deadline → Rhythm & Reminders → Progress Tracking → Celebrate Wins

**UX behavior:** New Goal → Claude conversation, one question at a time. Goal card: progress ring + current score + GYR grade.

**Success criteria:** Setup < 5 Claude exchanges. Score + % always current. GYR grade reflects latest report.

---

### F05 — GYR Spiral

**What it does:** Six-step transformation session — Facts → Feelings → Root Cause → Focus → Actions → Fruit. Filed to GYR Status Reports + Journals.

**Entities read:** Goal (#1), Priority (#2), GYR Status Report (#6) prior reports.
**Entities written:** GYR Status Report (#6), Journal Entry (#8).

**UX behavior:** Entry from top-right icon, Goal card, or Kompass suggestion. One step at a time with prior answers visible. Prior Spiral context loaded. Grade selected on completion.

**Success criteria:** Completable < 15min. Prior context loads. GYR grade updates immediately. Journal filed correctly.

---

### F06 — Learning Engine

**What it does:** Staged, spaced learning — one concept per day. In-App (6-hour safety rail) and External Practice (daily checkbox). Progress in SB Training & Certifications.

**Entities read:** Learning Track (#43), Course (#42), Lesson (#41), Progress Record (#44).
**Entities written:** Progress Record (#44).

| Type | Delivery | Safety rail |
|---|---|---|
| In-App | Calmio-style card, Next button | 6-hour hold |
| External Practice | Daily checkbox prompt | None |

**Success criteria:** Safety rail works. External Practice independent. Progress ring updates immediately.

---

### F07 — Daily Reminder Engine

**What it does:** One rotating thought per day from three pools: Vivid Vision, Annual Commitments, Principles & Realizations.

**Entities read:** Vivid Vision (#36), Annual Commitments (#37), Principles & Realizations (#11).
**Entities written:** Principles & Realizations (#11) on save.

**Success criteria:** No repeat within 7 days. All pools render. Displays < 1s.

---

### F08 — Body Domain — Health Tracking & Vault

**What it does:** All Body data — live metrics (Oura, Apple Health, Strava) and historical Drive records. Three sub-sections: Metrics Dashboard, Training Log, Health Vault.

**Entities read:** Oura Sleep (#30), Readiness (#31), Apple Health Weight (#32), Strava (#29), Drive records (#33–35), Goal (#1), Learning Track (#43).
**Entities written:** Stat (#4) manual logs. Drive link records (#33–35) new additions.

**Metrics Dashboard:** Weight trend, sleep sparkline, readiness score, waist circumference, training summary, alcohol streak, meal adherence.
**Training Log:** Last 10 Strava activities. Race countdown. Phase gate criteria.
**Health Vault:** Chronological Drive-linked records. Add: type → date → URL → notes.

**Success criteria:** Weight 7-day average correct. Oura refreshes on open. Drive links open natively. Race countdown correct.

---

### F09 — Big Ass Calendar

**What it does:** Year-at-a-glance. Backward layer (completed, muted) + forward layer (upcoming, full color). Primary Free Day view.

**Entities read:** BAC Calendar Events (#13), BAC Goals (#14), Goal (#1).
**Entities written:** BAC Calendar Events (#13).

**UX behavior:** 12 months, one screen, color-coded. Tap month → month view. Free Day: replaces Today tab. Wins panel: last 90 days completed events.

**Success criteria:** All 12 months without scrolling. Completed events muted. Add event < 4 taps. Free Day correct.

---

### F10 — Quarterly Habit Arc

**What it does:** Full lifecycle of one quarterly habit — selection, five-stage installation, Freudian achievement at completion.

**Entities read:** Goal (#1), Stat (#4), Progress Record (#44), Learning Track (#43).
**Entities written:** Goal (#1) stage, Journal Entry (#8) milestones, BAC Calendar Event (#13) quarter completion.

**Stages:** Install → Beginner → Intermediate → Expert → Complete

**Quarter-end:** Journal prompt, identity statement generated, BAC milestone, optional Table Talk entry.

**Success criteria:** Only one active habit. Stage auto-advances. Celebration fires correctly. Identity statement stored.

---

### F11 — Container Model — Empty State & Build Flow

**What it does:** Every section exists from day one as an invitation — three visual layers (glow, invitation copy, progress ring) — with Claude-guided conversational build flow.

**UX behavior:** Empty container → "why this matters" card → uh-huh → Claude conversation → filed → container fills → progress ring updates → celebration.

**Success criteria:** All sections show correct empty state. Build completes without multi-field form. Progress ring updates. Refresh triggers fire correctly.

---

### F12 — About Me + Vivid Vision

**What it does:** Clint's full personal profile (4 GitHub files) and Vivid Vision / Annual Commitments. Family profiles read-only.

**Entities read:** Clint's Profile (#38), Family Profiles (#39), Vivid Vision (#36), Annual Commitments (#37).

**UX behavior:** Four tabs: Operating Manual / Quick Reference / Vivid Vision / 2026 Commitments. Full markdown render. Search. Measurable commitments tap-through to linked Goal. Family profiles: person selector, read-only.

**Success criteria:** All 4 files render from GitHub. Google Doc opens natively. Commitment tap-through navigates correctly.

---

### F13 — Project + Tool Layer

**What it does:** Personal/family projects via S-BOS infrastructure, organized by four pillars. Claude builds custom tools per stage, archived on completion.

**Entities read/written:** Project (#18), Check Lists (#19), Check List Tasks (#20), Budget tables (#21–24), Schedule tables (#25–26), Project Tool (#28).

**UX behavior:** Project list → four-pillar view. Tool creation: inside pillar or Shortcuts trigger. Tool renders embedded. Archive = searchable library.

**Success criteria:** Hierarchy renders from SmartSuite. Tool creation < 5 exchanges. Archived tools findable.

---

### F14 — Kompass Operating Platform

**What it does:** Three-layer cognitive offload — Second Brain (capture + route), Buffer Anchor (clear + reply), Genius Schedule (protect + review).

**Entities written:** Task (#7), Journal Entry (#8), Decision (#10), Principles & Realizations (#11), BAC Calendar Events (#13), Notes & Comments (#27).

**Capture routing:** Nine types classified by signal. Ambiguous → two options + gut check.
**Buffer:** Email triage → task + draft reply → Clint approves → sends. `Who` privacy rule.
**Genius Schedule:** Morning day review. Weekly Monday audit.

**Success criteria:** > 90% classification accuracy. `Who` never auto-populated. Buffer surfaces draft replies. Monday audit identifies violations.

---

### F15 — Shortcuts Tab

**What it does:** Single-tap access to 36 Claude skills (19 personal + 17 business) and 8 external tools.

**UX behavior:** Four sections: Personal / Business / External Tools / Recent. Search bar.

**Success criteria:** All 36 skills launch. All 8 tools deep-link. Recent updates. Search < 200ms.

---

### F16 — Journal & Decisions Library

**What it does:** A unified, searchable, filterable library of every Journal Entry (rituals, stacks, Spirals, Table Talk, free writes, Day Mode Logs) and every Decision record — the permanent record of Clint's inner life and choices.

**Entities read:** Journal Entry (#8), Decision (#10).
**Entities written:** Journal Entry (#8) — new entries. Decision (#10) — new decisions.

**UX behavior:**

**Journal Library:**
- Me menu → "Journal" — chronological feed, most recent first
- Filter by type: All / Rituals / Stacks / Spirals / Table Talk / Free Write / Day Mode Log / Project Debrief
- Filter by domain: All / Body / Being / Balance / Business
- Filter by date range: This week / This month / This year / Custom
- Full-text search across all journal content
- Each entry: type icon, date, domain tag, first line → tap to expand
- FAB: new journal session → type picker → Claude-guided or free-form
- **Day Mode scoreboard sub-view:** Focus / Buffer / Free Day counts by week, month, quarter

**Decisions Library (sub-tab):**
- All Decision records from SmartSuite `68feda0035fd19c93de8d757`
- Each: title, date, status (Open / Resolved / Deferred), domain
- Filter by status + domain
- Tap: full record with context, options, outcome
- FAB: new decision → Claude guides: What's the question? What are the options? What does your gut say?
- Resolved decisions archived but searchable

**What this is NOT:** Not a social feed. Not the Today tab. The Today tab shows today's ritual and lesson — the Journal is the full historical library.

**Success criteria:**
- Full-text search < 500ms
- All filter combinations work correctly
- Day Mode scoreboard shows correct counts per period
- New entry FAB routes to correct table with correct type + domain tags
- New decision FAB routes to Decisions table correctly

---

### F17 — Being Domain

**What it does:** Surfaces Clint's inner life domain — Goals and progress for mindset, presence, spiritual/emotional development, and rituals. Being is about who Clint is becoming internally.

**Entities read:** Goal (#1) domain=Being, Priority (#2), Stat (#4), GYR Status Report (#6), Journal Entry (#8), Learning Track (#43).
**Entities written:** Stat (#4) — Being stat logs. Journal Entry (#8) — ritual completions and stacks.

**What belongs in Being:**
- Rituals (morning/evening ritual streaks)
- Mindset practices (meditation, breathwork — tracked via External Practice checkboxes)
- Emotional processing (stacks, Being-domain Spirals)
- Identity and character development (being a great man, not just a nice one)
- Principles and realizations accumulation
- Presence and connection quality

**UX behavior:**

**Being domain card (Me tab):**
- GYR grade indicator — overall Being status
- Active Being Goals with progress rings (e.g., "Morning Ritual Streak: 14 days," "Stack completions: 3/month")
- Quarterly Habit shown if Being-domain
- Active Learning Track shown if Being-domain (e.g., Calmio practice)
- "Run Being Spiral" button — Spiral pre-loaded with Being domain

**Stat logging for Being:** Primarily via External Practice checkboxes (Calmio) or journal capture inference (Kompass classifies ritual completion mentions as stat logs). Claude prompts before any manual form.

**Success criteria:**
- Being Goals render correctly in Me tab domain card
- Spiral pre-fills with Being domain on button tap
- Ritual streak stats update from journal captures and External Practice logs
- Being Learning Tracks surface correct daily lesson

---

### F18 — Balance Domain

**What it does:** Surfaces Clint's relational domain — Goals and progress for family coordination, relationships, and connection. Balance measures how well Clint tends the people entrusted to him.

**Entities read:** Goal (#1) domain=Balance, Stat (#4), GYR Status Report (#6), Journal Entry (#8), Family Profiles (#39), Strava Activity (#29), BAC Calendar Events (#13).
**Entities written:** Stat (#4) — relationship measurables via Stat Inference Engine (F03) primarily. Journal Entry (#8) — Table Talk entries.

**What belongs in Balance:**
- Relationship measurables — quantitative goals for time with each family member
- Table Talk — dinner ritual, recorded by Clint
- Family profile context — who each person is, how they're wired
- Connection quality — Spiral assessments of relational health
- Shared experiences (Kevin's Rule events often involve family)

**UX behavior:**

**Balance domain card (Me tab):**
- GYR grade indicator — overall Balance status
- Relationship measurable Goals with progress: *"Rides with Max Q3: 3/10"*, *"Dates with Christie: 2/3 this month"*
- Table Talk history shortcut — last 7 entries or add new
- "Run Balance Spiral" button — Spiral pre-loaded with Balance domain
- Family member avatars — tap to view read-only profile

**Relationship measurables model:**
- Each measurable = Balance Goal with custom Stat Menu Item
- Logged via Stat Inference Engine (F03) — Claude notices Strava rides or journal mentions and prompts
- Manual fallback: long-press Goal card → "Log one now" → single-tap confirmation
- Progress: current count / target, rolling period (quarter or month per goal)

**Table Talk sub-section:**
- Chronological feed of all Table Talk journal entries
- Add new: date → Hi (best thing today) → Lo (hardest thing today) → Buffalo (surprise) → save
- Phase 1: Clint logs for the whole family. Phase 2: each person logs their own.

**Success criteria:**
- Relationship measurable Goals render with correct current count / target
- Stat Inference Engine logs correctly with one tap
- Table Talk add flow < 4 taps
- Family profile avatars load from GitHub and open read-only markdown
- Balance Spiral pre-fills with Balance domain on button tap

---

## §4 Gate 3 Checklist

- [ ] All 18 features named and described ✳️ *Pending sign-off*
- [ ] All features have entities read/write ✳️ *Pending sign-off*
- [ ] All features have success criteria ✳️ *Pending sign-off*
- [ ] No feature references an entity not in §3 ✳️ *Pending sign-off*

---

## §5 — User Workflows

> ⏳ Not started. Writing after §4 sign-off.

---

## §6 — Scope & Phasing
> ⏳ Not started. Begins after Gate 3.

## §7 — Success Metrics
> ⏳ Not started. Begins after Gate 3.

## §8 — Timeline
> ⏳ Not started. Begins after Gate 3.

## §9 — Open Questions
> ⏳ Not started. Running list in `TSW_memory.md`.

---

## Discovery Inputs (from session 2026-06-24 / 2026-06-25)

*A–O captured in full in prior commits. Full detail in TSW_memory.md.*
