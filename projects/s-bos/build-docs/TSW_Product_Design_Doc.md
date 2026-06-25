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

> 44 entities. Full definitions and app IDs in prior commit history and TSW_memory.md.

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

**What it does:** Determines the operating posture for the day — Focus, Buffer, or Free — and changes the app's behavior accordingly.

**Entities read:** BAC Calendar Events (#13), Task (#7), Day Mode Log (#9).
**Entities written:** Day Mode Log (#9).

**UX behavior:**
- Kompass suggests one mode with one-sentence reason on app open
- Clint: uh-huh (confirms) or uh-uh (three-option picker). Max two questions.
- **Focus Day:** Tab bar hides. Anchor + Pomodoro only. Break-glass available. Grounding affirmation shown.
- **Buffer Day:** Full app. Horizon Rings command center. Sub-tabs: Horizon / Email / Big 3. Guardrail questions on commitments.
- **Free Day:** Calendar + wins panel + Table Talk prompt only. *"Your job today is to wander."* Kompass off.

**Success criteria:** Mode suggested < 3s. Log created on confirmation. Tab bar correct. Free Day shows no tasks/inbox.

---

### F02 — Horizon Rings

**What it does:** Surfaces the most important items from projects, goals, and follow-ups in a single filtered view. Max 7–10 items.

**Entities read:** Check List Tasks (#20), Notes & Comments (#27), GYR Status Reports (#6), Goals (#1).
**Entities written:** Check List Tasks (#20) — status updates. Stats (#4) — Sacral Anchor log.

**Five rings:** 🔴 Overdue / 🟡 This Week / 🔵 Active / 🟢 Coming Soon / ⚪ Parked

**UX behavior:**
- Priority stack: Overdue → GYR Red → Notes past due → Active Big 3 → Aging This Week
- Phase Context Strip above rings
- Stacked list (default triage) / Circles overview (tap → Focus bridge)
- Sacral Anchor: *"What has the most pull?"* — top 3 options, Clint selects, pinned all day
- Quick Clear: stale items (no date + no activity 7+ days), batches of 3–5

**Success criteria:** Never > 10 items. Toggle works without reload. Anchor persists all day. Quick Clear filters correctly.

---

### F03 — Stat Inference Engine

**What it does:** Claude scans Strava, journals, and calendar events and surfaces one-tap prompts to log stats against active Goals — no manual forms.

**Entities read:** Strava (#29), Journal Entry (#8), BAC Calendar Events (#13), Goal (#1), Stat Menu Item (#5).
**Entities written:** Stat (#4).

**UX behavior:**
- Scans after each event. Matches by person name + activity type.
- Prompt: *"Was Max with you on that ride? Count it toward 'Rides with Max Q3' (3/10)?"*
- Yes / No / Not quite (one clarifying question max). Prompts queue one at a time.

**Success criteria:** Match rate > 85%. Prompt surfaces < 5min. No multi-field forms. Goal % updates on confirm.

---

### F04 — Universal Goal Engine

**What it does:** Five-step framework for every initiative across every domain. All Goal types use this engine.

**Entities read/written:** Goal (#1), Priority (#2), Milestone (#3), Stat (#4), Stat Menu Item (#5), GYR Status Report (#6).

**Five steps:** Current Score → Goal + Deadline → Rhythm & Reminders → Progress Tracking → Celebrate Wins

**UX behavior:** New Goal → Claude-guided conversation, one question at a time. Goal card: progress ring + current score. GYR grade as colored indicator.

**Success criteria:** Setup in < 5 Claude exchanges. Score + % metric always current. GYR grade reflects latest report.

---

### F05 — GYR Spiral

**What it does:** Claude-guided six-step transformation session: Facts → Feelings → Root Cause → Focus → Actions → Fruit. Filed to GYR Status Reports and Journals.

**Entities read:** Goal (#1), Priority (#2), GYR Status Report (#6) — prior reports.
**Entities written:** GYR Status Report (#6), Journal Entry (#8).

**UX behavior:**
- Entry: top-right icon, Goal card button, or Kompass proactive suggestion (Yellow/Red domain)
- One step at a time. Prior answers visible as you progress. Prior Spiral context loaded.
- On completion: grade selected, filed to SmartSuite + Journal simultaneously.

**Success criteria:** Completable < 15min. Prior context loads correctly. GYR grade updates immediately. Journal filed with correct type + domain tag.

---

### F06 — Learning Engine

**What it does:** Staged, spaced learning — one concept per day. Two lesson types: In-App (6-hour safety rail) and External Practice (daily checkbox). Progress tracked in SB Training & Certifications.

**Entities read:** Learning Track (#43), Course (#42), Lesson (#41), Progress Record (#44).
**Entities written:** Progress Record (#44).

| Type | Delivery | Safety rail |
|---|---|---|
| In-App | Calmio-style card, progress indicator, Next button | 6-hour hold |
| External Practice | *"Did you do your Calmio session today?"* Yes/Skip | None |

**UX behavior:** Today's lesson surfaces on Today tab. Next tap logs completion + brief celebration. 6-hour rail shows: *"Come back tomorrow — sleep is doing the work."*

**Success criteria:** Safety rail prevents same-day next lesson. External Practice independent of rail. Progress ring updates immediately.

---

### F07 — Daily Reminder Engine

**What it does:** One rotating thought per day on Today tab from three pools: Vivid Vision lines, Annual Commitment lines, Principles & Realizations records.

**Entities read:** Vivid Vision (#36), Annual Commitments (#37), Principles & Realizations (#11).
**Entities written:** None (display only). Save action → Principles & Realizations (#11).

**Success criteria:** No repeat within 7 days. All three pools render correctly. Displays < 1s.

---

### F08 — Body Domain — Health Tracking & Vault

**What it does:** All Body data in one place — live API metrics (Oura, Apple Health, Strava) and historical Drive-linked records. Three sub-sections: Metrics Dashboard, Training Log, Health Vault.

**Entities read:** Oura Sleep (#30), Oura Readiness (#31), Apple Health Weight (#32), Strava (#29), Body Scan (#33), Bloodwork (#34), Eye Prescription (#35), Goal (#1), Learning Track (#43).
**Entities written:** Stat (#4) — manual logs (meals, alcohol). Drive link records (#33–35) — new additions.

**Metrics Dashboard:** Weight trend, sleep sparkline, readiness score, waist circumference, training summary, alcohol streak, meal adherence.

**Training Log:** Last 10 Strava activities, race countdown (Downieville Jul 23, Grizzly 100 Sep 5), phase gate criteria visible.

**Health Vault:** Chronological Drive-linked records. Add: type → date → URL → notes. Cross-references Key Docs Health category.

**Success criteria:** Weight 7-day rolling average correct. Oura refreshes on app open. Drive links open natively. Race countdown correct.

---

### F09 — Big Ass Calendar

**What it does:** Year-at-a-glance visual. Backward layer (completed events, muted) + forward layer (upcoming events, full color). Primary view on Free Days.

**Entities read:** BAC Calendar Events (#13), BAC Goals (#14), Goal (#1).
**Entities written:** BAC Calendar Events (#13).

**UX behavior:** 12 months on one screen. Color-coded by category. Tap month → month view. Free Day: replaces Today tab layout. Wins panel shows last 90 days of completed events.

**Success criteria:** All 12 months without scrolling. Completed events muted. Add event < 4 taps. Free Day shows calendar correctly.

---

### F10 — Quarterly Habit Arc

**What it does:** Full lifecycle of one quarterly habit — selection, five-stage installation, Freudian achievement at completion. One at a time.

**Entities read:** Goal (#1) `type = "Quarterly Habit"`, Stat (#4), Progress Record (#44), Learning Track (#43).
**Entities written:** Goal (#1) stage update, Journal Entry (#8) milestones, BAC Calendar Event (#13) quarter completion.

**Five stages:** Install → Beginner → Intermediate → Expert → Complete

**Quarter-end:** Journal prompt, identity statement generated (*"You're now someone who..."*), BAC milestone, optional Table Talk entry.

**Success criteria:** Only one active habit at a time. Stage auto-advances. Celebration fires on correct date. Identity statement stored.

---

### F11 — Container Model — Empty State & Build Flow

**What it does:** Every section exists from day one as an invitation — three visual layers (glow, invitation copy, progress ring) — with a Claude-guided conversational build flow.

**Success criteria:** All sections show correct empty state on first use. Build flow completes without multi-field form. Progress ring updates immediately. Refresh triggers fire correctly.

---

### F12 — About Me + Vivid Vision

**What it does:** Clint's full personal profile (4 GitHub files) and Vivid Vision / Annual Commitments — readable, searchable. Family profiles read-only.

**Entities read:** Clint's Profile (#38), Family Profiles (#39), Vivid Vision (#36), Annual Commitments (#37).

**UX behavior:** Four tabs: Operating Manual / Quick Reference / Vivid Vision / 2026 Commitments. Full markdown render. Search across all. Measurable commitments tap-through to linked Goal. Family profiles: person selector, read-only.

**Success criteria:** All 4 files render from GitHub. Google Doc link opens natively. Commitment tap-through navigates correctly.

---

### F13 — Project + Tool Layer

**What it does:** Personal/family projects via S-BOS infrastructure, organized by four pillars. Claude builds custom tools per stage, archived on completion.

**Entities read/written:** Project (#18), Check Lists (#19), Check List Tasks (#20), Budget tables (#21–24), Schedule tables (#25–26), Project Tool (#28).

**UX behavior:** Project list → four-pillar view. Tool creation: inside pillar or Shortcuts Claude skill. Tool renders as embedded HTML. Archive is searchable library.

**Success criteria:** Hierarchy renders from SmartSuite. Tool creation < 5 exchanges. Archived tools findable by type + domain.

---

### F14 — Kompass Operating Platform

**What it does:** Three-layer cognitive offload system — Second Brain (capture + route), Buffer Anchor (clear + reply), Genius Schedule (protect + review).

**Entities written:** Task (#7), Journal Entry (#8), Decision (#10), Principles & Realizations (#11), BAC Calendar Events (#13), Notes & Comments (#27).

**Capture routing:** Nine types, classified by signal. Ambiguous → two options + gut check.
**Buffer:** Email triage → task + draft reply → Clint approves → sends. `Who` privacy rule.
**Genius Schedule:** Morning day review. Weekly Monday audit.

**Success criteria:** > 90% classification accuracy. `Who` never auto-populated. Buffer surfaces draft replies. Monday audit identifies violations.

---

### F15 — Shortcuts Tab

**What it does:** Single-tap access to 36 Claude skills (19 personal + 17 business) and 8 external tools.

**UX behavior:** Four sections: Personal / Business / External Tools / Recent. Search bar. Recent = last 5 used.

**Success criteria:** All 36 skills launch correctly. All 8 tools deep-link correctly. Recent updates after use. Search filters < 200ms.

---

### F16 — Journal & Decisions Library

**What it does:** A unified, searchable, filterable library of every Journal Entry (rituals, stacks, Spirals, Table Talk, free writes, Day Mode Logs) and every Decision record — the permanent, growing record of Clint's inner life and choices. Not a feed to scroll — a library to search and reference.

**Entities read:** Journal Entry (#8), Decision (#10).
**Entities written:** Journal Entry (#8) — new entries from FAB launcher. Decision (#10) — new decisions from FAB or capture routing.

**UX behavior:**

**Journal Library:**
- Accessible from Me menu → "Journal" — full chronological feed, most recent first
- Filter by type: All / Rituals / Stacks / Spirals / Table Talk / Free Write / Day Mode Log / Project Debrief
- Filter by domain: All / Body / Being / Balance / Business
- Filter by date range: This week / This month / This year / Custom
- Search: full-text search across all journal content
- Each entry shows: type icon, date, domain tag, first line of content → tap to expand full entry
- FAB (floating action button): launches a new journal session → type picker → Claude-guided or free-form entry
- Day Mode Logs surfaced as a scoreboard sub-view: how many Focus / Buffer / Free Days this month, this quarter

**Decisions Library:**
- Sub-tab within Journal section: "Decisions"
- All Decision records from SmartSuite `68feda0035fd19c93de8d757`
- Each decision shows: title, date created, status (Open / Resolved / Deferred), linked domain
- Filter by status and domain
- Tap to open: full decision record with context, options considered, outcome
- FAB: create new decision → Claude guides through: What's the question? What are the options? What does your gut say?
- Resolved decisions archived but searchable — the historical record of how Clint decided

**What this is NOT:**
- Not a social feed — no likes, no sharing, no streaks shown here
- Not the Today tab — Today tab shows today's ritual and lesson. The Journal is the full historical library.

**Success criteria:**
- Full-text search returns results < 500ms
- All filter combinations work correctly
- Day Mode Log scoreboard shows correct counts per period
- New journal entry from FAB correctly routes to Journals table with correct type and domain tags
- New decision from FAB correctly routes to Decisions table

---

### F17 — Being Domain

**What it does:** Surfaces Clint's inner life domain — Goals, progress, and context for mindset, presence, spiritual/emotional development, and rituals. The Being domain is about who Clint is becoming internally — separate from what he's building (Business), how his body performs (Body), or how his relationships are going (Balance).

**Entities read:** Goal (#1) — domain = Being. Priority (#2) — linked to Being Goals. Stat (#4) — Being domain stats. GYR Status Report (#6) — Being domain Spirals. Journal Entry (#8) — Being domain entries. Learning Track (#43) — Being-domain learning arcs.

**Entities written:** Stat (#4) — Being stat logs (e.g., days of morning ritual completion, meditation sessions). Journal Entry (#8) — ritual completions and stacks filed here.

**What belongs in Being:**
- Rituals (morning ritual streak, evening ritual streak)
- Mindset practices (meditation, breathwork, journaling practices as habits)
- Emotional processing (stacks, Spirals run in the Being domain)
- Identity and character development (being a great man, not just a nice one — from 2026 Commitments)
- Principles and realizations accumulation
- Presence and connection — being present with people, not just coordinating with them

**UX behavior:**

**Being domain card (Me tab):**
- GYR grade indicator — overall Being status from most recent GYR Spiral
- Active Being Goals listed with progress rings (e.g., "Morning Ritual Streak," "Meditation Practice," "Stack completion")
- Current Quarterly Habit shown if Being-domain (e.g., "Evening ritual before bed")
- Active Learning Track shown if Being-domain
- "Run Being Spiral" button — shortcut to GYR Spiral pre-loaded with Being domain

**Being Goal examples:**
- Morning ritual — daily completion tracked as a Stat. Target: 90% of days this quarter.
- Evening ritual — same model
- Meditation practice — External Practice Learning Track checkbox (Calmio)
- Stack completion frequency — how many stacks completed per month
- Presence score — self-rated Stat, logged weekly

**Stat logging for Being:** Most Being stats are logged via the Learning Engine (External Practice checkboxes) or via journal entry capture (Kompass classifies ritual completion mentions as stat logs). Claude infers from journal content where possible before prompting.

**Success criteria:**
- Being Goals render correctly in the Me tab domain card
- GYR Spiral pre-fills with Being domain on "Run Being Spiral" tap
- Ritual streak stats update correctly from journal captures and External Practice logs
- Being domain Learning Tracks surface the correct Course and daily lesson

---

### F18 — Balance Domain

**What it does:** Surfaces Clint's relational domain — Goals, progress, and context for family coordination, relationships, and connection. Balance is about how well Clint tends the people entrusted to him — Christie, the kids, close friends — measured by both quantitative (time spent) and qualitative (quality of connection) indicators.

**Entities read:** Goal (#1) — domain = Balance. Stat (#4) — Balance domain stats (relationship measurables). GYR Status Report (#6) — Balance Spirals. Journal Entry (#8) — Table Talk entries, relationship notes. Family Profiles (#39) — read-only context. Strava Activity (#29) — for inference of shared activities. BAC Calendar Events (#13) — shared events.

**Entities written:** Stat (#4) — relationship measurables (rides with Max, dates with Christie, etc.) — via Stat Inference Engine (F03) primarily, manual as fallback. Journal Entry (#8) — Table Talk entries.

**What belongs in Balance:**
- Relationship measurables — quantitative goals for time with each family member
- Table Talk — the dinner ritual, recorded by Clint
- Family profile context — who each person is, how they're wired, what they need
- Connection quality — self-rated Spiral assessments of relational health
- Shared experiences — Kevin's Rule events often involve family

**UX behavior:**

**Balance domain card (Me tab):**
- GYR grade indicator — overall Balance status
- Relationship measurable Goals listed with progress (e.g., "Rides with Max Q3: 3/10," "Dates with Christie: 2/3 this month")
- Table Talk history shortcut — tap to see last 7 entries or add a new one
- "Run Balance Spiral" button — shortcut to Spiral pre-loaded with Balance domain
- Family members shown as avatars — tap to view read-only profile

**Relationship measurables model:**
- Each measurable is a Balance Goal with a custom Stat Menu Item (e.g., "Rides with Max")
- Stats logged via Stat Inference Engine (F03) — Claude notices Strava rides or journal mentions and prompts
- Manual fallback: long-press the Goal card → "Log one now" → single tap confirmation
- Progress shown as: current count / target, rolling period (this quarter or this month depending on the goal)

**Table Talk sub-section:**
- Chronological feed of all Table Talk journal entries
- Add new: date → Hi (best thing today) → Lo (hardest thing today) → Buffalo (surprise) → save
- Phase 1: Clint logs for the whole family or just himself. Phase 2: each family member logs their own.
- View by person (Phase 2) or as a combined feed (Phase 1)

**Success criteria:**
- Relationship measurable Goals render with correct current count / target
- Stat Inference Engine correctly logs relationship stats with one tap
- Table Talk add flow completes in < 4 taps
- Family profile avatars load correctly from GitHub and open read-only markdown
- Balance Spiral pre-fills with Balance domain on button tap

---

## §4 Gate 3 Checklist

- [ ] All features named and described ✳️ *Pending sign-off*
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
