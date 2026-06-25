# Product Design Doc: Stitser Way

> **Status:** 🔄 In Progress — Gate 2 ✅ COMPLETE. §4 Core Features in progress (20 features).
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

> **20 features.** Each spec: what it does, entities read/write, UX behavior, success criteria.

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

**What it does:** Surfaces the most important items from projects, goals, and follow-ups. Max 7–10 items. The daily triage view — answers *"What needs my attention right now?"*

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

**What it does:** Unified searchable library of every Journal Entry (rituals, stacks, Spirals, Table Talk, free writes, Day Mode Logs) and every Decision record.

**Entities read:** Journal Entry (#8), Decision (#10).
**Entities written:** Journal Entry (#8) new entries. Decision (#10) new decisions.

**Journal Library:** Filterable by type, domain, date range. Full-text search. FAB launcher. Day Mode scoreboard sub-view (Focus/Buffer/Free counts by week/month/quarter).

**Decisions sub-tab:** Open / Resolved / Deferred status. Claude-guided creation: What's the question? What are the options? What does your gut say?

**Success criteria:** Full-text search < 500ms. All filters work. Day Mode scoreboard correct. FABs route to correct tables.

---

### F17 — Being Domain

**What it does:** Inner life domain — Goals and progress for mindset, presence, spiritual/emotional development, and rituals. Who Clint is becoming internally.

**Entities read:** Goal (#1) domain=Being, Priority (#2), Stat (#4), GYR Status Report (#6), Journal Entry (#8), Learning Track (#43).
**Entities written:** Stat (#4) Being stat logs. Journal Entry (#8) ritual completions and stacks.

**What belongs:** Morning/evening ritual streaks. Mindset practices (External Practice checkboxes). Emotional processing (stacks, Being Spirals). Identity development. Principles accumulation.

**UX behavior:** Being domain card: GYR grade, active Goals with progress rings, Quarterly Habit if Being-domain, Learning Track if Being-domain, "Run Being Spiral" button.

**Stat logging:** Via External Practice checkboxes or journal inference. Claude prompts before any manual form.

**Success criteria:** Goals render in Me tab. Spiral pre-fills Being domain. Ritual streaks update from External Practice and journal captures.

---

### F18 — Balance Domain

**What it does:** Relational domain — Goals and progress for family coordination, relationships, and connection. How well Clint tends the people entrusted to him.

**Entities read:** Goal (#1) domain=Balance, Stat (#4), GYR Status Report (#6), Journal Entry (#8), Family Profiles (#39), Strava (#29), BAC Calendar Events (#13).
**Entities written:** Stat (#4) relationship measurables via Stat Inference Engine (F03). Journal Entry (#8) Table Talk.

**What belongs:** Relationship measurables (rides with Max, dates with Christie). Table Talk. Family profile context. Connection quality Spirals. Shared experiences.

**UX behavior:** Balance card: GYR grade, relationship measurable Goals with progress, Table Talk shortcut, "Run Balance Spiral" button, family member avatars (read-only).

**Table Talk:** Add: date → Hi → Lo → Buffalo → save. Phase 1: Clint logs. Phase 2: per-member.

**Success criteria:** Measurables render with current count/target. Stat Inference Engine logs in one tap. Table Talk < 4 taps. Spiral pre-fills Balance domain.

---

### F19 — Business Domain

**What it does:** Surfaces Clint's external mission domain — Goals and progress for his role as allocator/CEO across all Stitser BUILT product lines, personal financial objectives, and business phase advancement. Business is the work Clint is building in the world, tracked from his personal vantage point — not the operational S-BOS view but the strategic CEO view.

**Entities read:** Goal (#1) domain=Business, Priority (#2), Stat (#4), GYR Status Report (#6), Goal (#1) tagged Misogi (domain=Business), Goal (#1) tagged Kevin's Rule if Business-adjacent, BAC Calendar Events (#13) — business milestones.

**Entities written:** Stat (#4) — Business stat logs (revenue milestones, cash flow targets, product line metrics). GYR Status Report (#6) — Business domain Spirals. Journal Entry (#8) — Business domain entries, project debriefs, strategic vision sessions.

**What belongs in Business:**
- Personal business Goals (allocator seat metrics, cash flow targets, company milestones)
- GYR status per product line — Clint's read of each company's health from his CEO seat
- Phase anchor progress — where each Stitser BUILT product line sits in its phase arc
- Business Misogi — the one business-defining goal for the year (e.g., completing a specific development, reaching a revenue milestone)
- Strategic vision sessions (logged as Journal entries via the Strategic Vision skill)
- Project Debrief entries for completed business initiatives

**UX behavior:**

**Business domain card (Me tab):**
- GYR grade indicator — overall Business status from Clint's most recent Business Spiral
- Active Business Goals with progress rings (e.g., "Allocator Seat: 4 days/week," "6-month liquidity buffer")
- Product line sub-cards — one per active Stitser BUILT entity showing:
  - Entity name + current phase
  - GYR status from most recent GYR Report for that entity
  - Tap → Phase Anchor deep link (opens Railway app)
- Business Misogi shown if set — progress toward the year-defining business goal
- "Run Business Spiral" button — Spiral pre-loaded with Business domain
- "Strategic Vision" shortcut → opens Strategic Vision Claude skill in Shortcuts tab

**Product line GYR view:**
- Each product line (Formation Homes, BUILT construction, Arbitrage CFO, brokerage, etc.) shown as a row
- GYR status pulled from the most recent GYR Status Report for that entity
- Tap product line → opens Phase Anchor for full phase detail
- This is Clint's scoreboard for the empire — not operational detail, just the allocator's snapshot

**Stat logging for Business:** Business stats are typically milestones and phase completions rather than daily logs. Claude prompts when a journal entry or capture mentions a business achievement. Manual log available for specific measurables (cash flow position, days in allocator seat per week).

**Success criteria:**
- Business Goals render correctly in Me tab domain card
- Product line sub-cards load GYR status from SmartSuite correctly
- Phase Anchor deep links open correctly for each product line
- Business Spiral pre-fills with Business domain on button tap
- Strategic Vision shortcut navigates to correct Shortcuts skill

---

### F20 — Week at a Glance

**What it does:** Shows the shape of the current week — day types (Focus/Buffer/Free), key scheduled events, and Big 3 placeholders — as a lightweight 7-day strip. Answers *"What does this week look like as a whole?"* The middle layer between the Big Ass Calendar (year view) and Horizon Rings (today's items). Distinct from both.

**Entities read:** BAC Day Types (#12) — day type assignments for the week. BAC Calendar Events (#13) — key events. Task (#7) — Big 3 anchors if set. Day Mode Log (#9) — days already logged this week.

**Entities written:** BAC Day Types (#12) — when Clint assigns or changes a day type for an upcoming day.

**UX behavior:**

**The strip:**
- 7-day horizontal strip — Mon through Sun, current week
- Each day shows:
  - Day type badge (Focus 🎯 / Buffer ⚡ / Free 🌿 / Unassigned ○)
  - Up to 2 key BAC Calendar Events as chips (e.g., "Team meeting", "Date night")
  - Big 3 anchor if set for that day — greyed if not yet set
  - Today highlighted with a stronger border
- Past days (earlier this week) shown in muted/completed state
- Tap any future day → assign or change day type (uh-huh/uh-uh, same Sacral mechanic)

**Where it lives:**
- On the Today tab — below the Day Mode badge, above the Big 3 dominos
- Also accessible from the Buffer Day sub-tab "Big 3" — where the week shape is most useful for planning

**Relationship to Buffer Day Sweep:**
The Buffer Day Sweep (a workflow, not a feature) uses the Week at a Glance as its visual anchor — Clint reviews the week strip, confirms or adjusts day types for remaining days, and sets Big 3 anchors for Focus Days ahead. The strip is the view; the Buffer Day Sweep is the process that populates it.

**Relationship to Big Ass Calendar:**
Week at a Glance is the zoomed-in view of the BAC. Tapping a week in the BAC year or month view jumps to the Week at a Glance for that week.

**What this is NOT:**
- Not a full calendar — no time-blocking, no hour-by-hour view
- Not a task list — Horizon Rings handles tasks
- Not a scheduling tool — Google Calendar handles scheduling

**Success criteria:**
- 7-day strip renders correctly with today highlighted
- Day type badges reflect current BAC Day Types records
- Tap to assign day type completes in 2 taps
- Big 3 anchors render correctly when set
- Buffer Day sub-tab correctly shows the week strip as planning context

---

## §4 Gate 3 Checklist

- [ ] All 20 features named and described ✳️ *Pending sign-off*
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
