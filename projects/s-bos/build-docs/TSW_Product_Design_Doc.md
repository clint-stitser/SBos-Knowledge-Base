# Product Design Doc: Stitser Way

> **Status:** 🔄 In Progress — Gate 2 ✅ COMPLETE. §4 Core Features in progress.
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

Most people who believe in intentional living have the same experience: the knowledge exists, the desire exists, and the tools exist — but they live in pieces. For Clint, this produces four compounding problems:

**1. Fragmentation.** No single place where the full picture of a life is visible, connected, and actionable.

**2. Absence.** No application designed around the actual frameworks — GYR Spiral, four life domains, phase-based accountability, Container Model, spaced learning, day-mode model, Table Talk, Vivid Vision, Misogi, Quarterly Habit arc.

**3. The activation gap.** Without a daily machine, knowledge fades, rituals drift, the Vivid Vision is written once and forgotten.

**4. No project-level tool layer.** Bounded projects (trips, medical protocols, study prep) have no purpose-built tool system.

### The solution in one sentence

> *"The app is a machine that procedurally produces a better life. You don't need to arrive fully formed. The machine installs you into clarity over time."*

### What a good solution looks like

A single application integrating the entire practice of intentional living — principles, rituals, tracking, coaching, health data, and project tools — into one coherent daily experience that serves Clint first and his family next.

### What this is NOT

Not a task manager. Not a journal app. Not S-BOS. Not tools glued together. Not finished when data is entered.

---

## §1 Gate 1 Checklist

- [x] Problem clearly stated — ✅ Approved by Clint 2026-06-25
- [x] Solution criteria stated — ✅ Approved by Clint 2026-06-25
- [x] Scope boundaries stated — ✅ Approved by Clint 2026-06-25

---

## §2 — Target Users

**Phase 1:** Clint Stitser only. Interest-based nervous system, object permanence challenges, consecutive appetite, Channel of Inspiration (1-8).

**Phase 2:** Christie, Avery, Brynn, Max. Kids enter their own data. Customized to each person's wiring.

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

> 44 entities. Full definitions, app IDs, and clarifications in the previous version of this document. Summarized here for reference.

**Game App:** Goals (#1), Priorities (#2), Milestones (#3), Stats (#4), Stat Menu Items (#5), GYR Status Reports (#6)
**Stitser Way solution:** Journal Entries (#8), Day Mode Log (#9), Decisions (#10), Principles/Realizations (#11), BAC Day Types (#12), BAC Calendar Events (#13), BAC Goals (#14)
**Game App Goals (tagged):** Quarterly Habit (#15), Misogi (#16), Kevin's Rule Event (#17)
**Tasks (composite — 3 sources):** Check List Tasks + Notes & Comments + GYR follow-ups (#7)
**SB Project MGT:** Projects (#18), Check Lists (#19), Check List Tasks (#20), Budget G702 (#21), Budget Line Items G703 (#22), Baseline Budget Items (#23), Bills & Invoices (#24), Project Schedule (#25), Project Dates (#26)
**SB Biz Dev:** Notes & Comments (#27)
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

> **15 features.** Each spec contains: what it does, entities read/write, UX behavior, success criteria. Phase 1 only.

---

### F01 — Day Mode Engine

**What it does:** Determines the operating posture for the day — Focus, Buffer, or Free — and changes the app's behavior accordingly. Claude suggests the mode based on three signals; Clint confirms with a gut check.

**Entities read:** BAC Calendar Events (#13) — today's schedule. Task (#7) — count of Overdue + This Week items. Day Mode Log (#9) — days since last Buffer and Free Day.

**Entities written:** Day Mode Log (#9) — one Journal Entry created per confirmed day, type = "Day Mode Log," fields: mode, date, confirmed-at timestamp.

**UX behavior:**
- On app open, Kompass surfaces one suggestion with a one-sentence reason: *"Today looks like a Buffer Day — 4 overdue, last buffer was Tuesday."*
- Clint responds: uh-huh (confirms) or uh-uh (opens three-option picker: Focus / Buffer / Free)
- Max two questions. Never open-ended.
- Mode badge visible in nav. Tap to switch at any time.
- **Focus Day:** Tab bar hides. Only anchor item + Pomodoro timer. Break-glass button (low-visibility) reveals task/capture/email access. Grounding affirmation from Clint's operating manual shown.
- **Buffer Day:** Full app. Horizon Rings is the command center. Three sub-tabs: Horizon / Email / Big 3. Two guardrail questions fire on any commitment decision.
- **Free Day:** Calendar + wins panel + Table Talk prompt only. Mandate: *"Your job today is to wander."* Kompass off.

**Success criteria:**
- Mode suggested within 3 seconds of app open
- Day Mode Log entry created automatically on confirmation
- Tab bar hides/shows correctly on mode change
- Free Day shows no tasks, no inbox, no Horizon Rings

---

### F02 — Horizon Rings

**What it does:** Surfaces the most important and urgent items from across Clint's active projects, goals, and follow-ups in a single, filtered, actionable view. The daily planning screen. Max 7–10 items at any time.

**Entities read:** Check List Tasks (#20) — assigned to Clint, not complete/cancelled. Notes & Comments (#27) — assigned to Clint, follow-up date ≤ today + 7 days. GYR Status Reports (#6) — Clint is follow-up owner, Red/Yellow, not completed. Goals (#1) — for Phase Context Strip.

**Entities written:** Check List Tasks (#20) — status updates (Done, Parked, Killed, Snoozed). Stats (#4) — when Sacral Anchor is confirmed as logged.

**UX behavior — five rings:**

| Ring | Signal |
|---|---|
| 🔴 Overdue | Past due date |
| 🟡 This Week | Due within 7 days OR aging 48hr+ untouched |
| 🔵 Active | No due date, currently in motion |
| 🟢 Coming Soon | Due 8–21 days out |
| ⚪ Parked | Due 22+ days or explicitly snoozed |

- Priority stack order: Overdue → GYR Red → Notes past due → Active Big 3 → Aging This Week
- Coming Soon and Parked collapsed by default — tap to expand
- Phase Context Strip above rings: one chip per active product line, current phase, tappable

**Dual-mode toggle (top-right icon):**
- **Stacked list (default):** Full triage. Done / Park / Kill / Snooze inline on each card.
- **Circles overview:** Concentric rings. Items as dots. Tap ring to filter. Tap dot → detail card below with actions. Tap "→ Focus" → returns to stacked with item highlighted in gold.

**Sacral Anchor:**
- Prompt: *"What has the most pull for you today?"*
- Surfaces top 3 actionable items. Clint selects one. Becomes the day's anchor, pinned at top.
- No algorithm — gut only.

**Quick Clear mode:** Triggered by button. Surfaces items with no due date AND no activity in 7+ days. Batches of 3–5. Commands: Done / Park / Kill / Keep.

**Success criteria:**
- Never more than 10 items in the active view
- Dual-mode toggle works without page reload
- Sacral Anchor persists as pinned item for the full day
- Quick Clear correctly surfaces stale items only

---

### F03 — Stat Inference Engine

**What it does:** Claude scans Strava, journal entries, and calendar events after they occur and surfaces one-tap prompts to log stats against active Goals — eliminating manual form entry for relationship and activity measurables.

**Entities read:** Strava Activity (#29), Journal Entry (#8), BAC Calendar Events (#13), Goal (#1) — active Balance Goals with linked Stat Menu Items, Stat Menu Item (#5).

**Entities written:** Stat (#4) — one new record per confirmed log.

**UX behavior:**
- Claude scans for signal after each event (Strava sync, new journal entry, completed calendar event)
- Matches event to active Goal by person name + activity type
- Surfaces one prompt per match: *"You just logged a ride — was Max with you? Count it toward 'Rides with Max Q3' (3 of 10)?"*
- Response options: Yes / No / Not quite (one clarifying question, never a form)
- Prompts queue — if multiple matches, presented one at a time
- Dismissed prompts do not re-surface for that event

**Success criteria:**
- Correct match rate > 85% on obvious signals (person name + activity in journal)
- Prompt surfaces within 5 minutes of triggering event
- No prompt ever opens a multi-field form
- Confirmed logs correctly increment the linked Goal's % metric complete

---

### F04 — Universal Goal Engine

**What it does:** Provides a consistent five-step framework for every initiative across every domain. All Goal types (Standard, Quarterly Habit, Misogi, Kevin's Rule) use this same engine. Domain-specific fields layer on top.

**Entities read/written:** Goal (#1), Priority (#2), Milestone (#3), Stat (#4), Stat Menu Item (#5), GYR Status Report (#6).

**The five steps:**

| Step | What happens | Entity |
|---|---|---|
| 1. Current Score | Where you are now vs. target — pulls from linked Stats | Goal + Stat |
| 2. Goal + Deadline | What you're aiming for and by when | Goal (target, due date) |
| 3. Rhythm & Reminders | Reporting frequency, chunked execution, staged learning | Goal (reporting cadence) + Learning Track |
| 4. Progress Tracking | Visual + numerical. % Metric Complete, % Time Complete, Balance to Finish, Reporting Grade | Goal (formula fields) |
| 5. Celebrate Wins | GYR grade update, scoreboard, phase advancement, identity statement | GYR Status Report + Journal Entry |

**UX behavior:**
- New Goal is created by tapping "Add" inside a domain → Claude opens a guided conversation (not a form)
- Claude asks one question at a time, never a multi-field page
- Once all five steps are populated, the Goal card renders with progress ring and current score
- Existing Goal tap → initiative screen showing all five steps, current values, and quick-log for Stats
- GYR grade shown as a colored indicator (Green/Yellow/Red) on the Goal card in the domain view

**Success criteria:**
- New Goal fully set up in < 5 Claude exchanges
- Goal card shows current score vs. target and % metric complete at all times
- GYR grade correctly reflects the most recent GYR Status Report

---

### F05 — GYR Spiral

**What it does:** Guides Clint through the six-step transformation processing session (Facts → Feelings → Root Cause → Focus → Massive/Relevant Actions → Fruit) for any Goal or Priority in any domain. Claude facilitates each step. Output is filed to the GYR Status Reports table.

**Entities read:** Goal (#1), Priority (#2), GYR Status Report (#6) — prior reports for context.

**Entities written:** GYR Status Report (#6) — one new record per completed Spiral. Journal Entry (#8) — filed simultaneously as a Journal record.

**UX behavior:**
- Entry points: (a) top-right Spiral icon from any screen, (b) "Run Spiral" button on any Goal/Priority card, (c) Kompass proactively suggests when a domain is Yellow or Red
- Step 1: Domain selector → Goal selector (or free-form if not linked to a Goal)
- Steps 2–7: Claude presents one step at a time. Prior answers visible as Clint progresses — the full picture builds.
- Claude provides contextual coaching at each step — not generic prompts
- On completion: summary shown, GYR grade selected, filed to SmartSuite, Journal Entry created
- Prior Spirals for the same Goal surfaced as context ("Last time you ran this, your root cause was…")

**Success criteria:**
- Spiral can be completed start to finish in < 15 minutes
- Prior Spiral context loaded correctly for returning Goals
- GYR grade on the linked Goal card updates immediately on completion
- Journal Entry filed correctly with type = "Spiral" and domain tag

---

### F06 — Learning Engine

**What it does:** Delivers staged, spaced learning for skills and habits being installed — one concept per day, sleep between sessions, 6-hour safety rail for in-app lessons. Tracks external practice (Calmio, other apps) via a daily checkbox. Progress tracked against a Learning Track in SB Training & Certifications.

**Entities read:** Learning Track (#43), Course (#42), Lesson (#41), Progress Record (#44).

**Entities written:** Progress Record (#44) — completion entry per lesson or external practice confirmation.

**Two lesson types:**

| Type | Delivery | Logged by | Safety rail |
|---|---|---|---|
| In-App | Calmio-style card: title, one concept, Next button. Progress indicator (17/20). Time estimate shown. | Auto on tap of Next | 6-hour hold after completion |
| External Practice | Daily prompt: *"Did you do your Calmio session today?"* One-tap Yes / Skip | Manual confirmation | None |

**UX behavior:**
- Learning Track activates when a new Quarterly Habit or Body protocol phase starts — automatic, not manual
- Today's lesson surfaces as a card on the Today tab (below Big 3, above Table Talk)
- "Next" tap logs completion, shows brief celebration, queues next lesson for next day's slot
- If 6-hour rail is active, Next is replaced by: *"Come back tomorrow — sleep is doing the work."*
- External Practice prompt surfaces each morning for active external-practice lessons
- Progress ring on the Learning Track card shows % of current Course complete
- Phase advancement (Course completion) triggers celebration and unlocks the next Course

**Success criteria:**
- 6-hour safety rail correctly prevents next lesson delivery within same day
- External Practice checkbox works independently of the safety rail
- Progress ring updates immediately on completion
- Learning Track completion triggers Quarterly Habit celebration sequence (F10)

---

### F07 — Daily Reminder Engine

**What it does:** Surfaces one rotating thought per day on the Today tab — drawn from Vivid Vision lines, Annual Commitment lines, and Principles & Realizations records. Designed to keep the north star visible without overwhelming. Modeled on Calmio's "A Thought For You."

**Entities read:** Vivid Vision (#36), Annual Commitments (#37), Principles & Realizations (#11).

**Entities written:** None. Display only.

**UX behavior:**
- Appears on Today tab as a dedicated card: "A Thought For You" label, one sentence or short paragraph
- Content pools: (a) parsed lines from `vivid-vision-2036.md`, (b) parsed lines from `2026-commitments.md`, (c) records from Principles & Realizations table
- Rotates daily — never the same content two days in a row
- Clint can tap to save (adds to Principles & Realizations if not already there) or dismiss
- If dismissed, a new one is drawn from the pool
- Tied to morning ritual — shown as part of the ritual open sequence

**Success criteria:**
- No repeat within a 7-day window
- Content renders correctly from all three pools
- Save action correctly creates/links a Principles & Realizations record
- Displays within 1 second of Today tab load

---

### F08 — Body Domain — Health Tracking & Vault

**What it does:** Surfaces all Body domain data in one place — live metrics from Oura, Apple Health, and Strava alongside historical health records from Google Drive. Phase-based protocol with staged learning. Three sub-sections: Metrics Dashboard, Training Log, Health Vault.

**Entities read:** Oura Sleep (#30), Oura Readiness (#31), Apple Health Weight (#32), Strava Activity (#29), Body Scan (#33), Bloodwork (#34), Eye Prescription (#35), Goal (#1) — Body domain goals, Learning Track (#43) — Body protocol track.

**Entities written:** Stat (#4) — when Clint logs a manual stat (meals, alcohol). Body Scan / Bloodwork / Eye Prescription records — when new Drive links are added.

**Three sub-sections:**

**Metrics Dashboard:**
- Weight: daily log from Apple Health → 7-day rolling average + trend indicator (↑↓→)
- Sleep: Oura sleep score, total sleep, HRV, RHR — last 7 nights shown as a sparkline
- Readiness: Oura readiness score + top 2 contributors surfaced
- Waist circumference: manually logged Stat, shown as latest value + trend
- Training: this week's rides/runs from Strava (TSS, elevation, ride count)
- Alcohol: current streak (days without) + this week's count
- Meals: current protocol adherence (manually logged or inferred from journal captures)

**Training Log:**
- Strava activities feed — last 10 activities with date, type, distance, elevation, TSS
- Race countdown: Downieville (Jul 23), Grizzly 100 (Sep 5)
- Phase indicator: Foundation / Engine / Race Block — current phase shown, gate criteria visible

**Health Vault:**
- Chronological list of Drive-linked health records
- Types: Body Scan / DXA, Bloodwork, Eye Prescription (extensible)
- Each record: type, date, notes field, one-tap Drive link
- Add record: type → date → Drive URL → notes (simple form — only place in the app where a mini-form is acceptable because this is record creation, not stat logging)
- Cross-reference with Key Docs Health category — a DXA report can be flagged to appear in both

**Success criteria:**
- Weight trend shows correct 7-day rolling average
- Oura data refreshes on each app open (or background sync)
- Health Vault Drive links open natively without Drive API auth
- Race countdown shows correct days remaining
- Phase gate criteria visible and checkable

---

### F09 — Big Ass Calendar

**What it does:** Provides a year-at-a-glance visual that makes the entire designed year visible simultaneously — solving object permanence for time-bound intentions. Two layers: backward (look how far we've come) and forward (what's coming). Surfaces as the primary view on Free Days.

**Entities read:** BAC Calendar Events (#13), BAC Goals (#14), Goal (#1) — Misogi and Kevin's Rule tagged goals.

**Entities written:** BAC Calendar Events (#13) — when Clint adds a new intentional event.

**UX behavior:**

**Year view (default):**
- All 12 months on one screen — tiny but scannable
- Color-coded by category: Misogi (red), Kevin's Rule adventures (orange), races (purple), family milestones (green), phase completions (blue), quarterly habit milestones (gold)
- Backward layer: completed events shown in muted/desaturated color with a checkmark — the "look how far" visual
- Forward layer: upcoming events in full color — the anticipation layer
- Tap any month → expands to month view

**Month view:**
- Standard month calendar grid
- Events shown as colored dots/chips
- Tap any event → detail card with name, date, category, notes
- "Add event" → category picker → date → title → notes → saves to BAC Calendar Events

**Free Day behavior:**
- Today tab shows Big Ass Calendar (year view) instead of the standard Today layout
- Forward layer emphasized — what are you looking forward to?
- Wins panel below: completed events from the last 90 days
- No tasks, no inbox, no Horizon Rings

**Success criteria:**
- Year view renders all 12 months on one screen without scrolling
- Completed events render in muted color
- Add event flow completes in < 4 taps
- Free Day correctly shows calendar instead of standard Today layout

---

### F10 — Quarterly Habit Arc

**What it does:** Manages the full lifecycle of one quarterly habit — from selection through five stages of installation to completion and identity-level celebration. One habit at a time. Consecutive Appetite model.

**Entities read:** Goal (#1) tagged `type = "Quarterly Habit"`, Stat (#4), Progress Record (#44), Learning Track (#43).

**Entities written:** Goal (#1) — stage field updated, Journal Entry (#8) — celebration record at milestones, BAC Calendar Event (#13) — quarter completion milestone.

**Five stages:**

| Stage | What it means | App behavior |
|---|---|---|
| Install | Habit chosen, first week | Daily prompt + Learning Track begins |
| Beginner | Weeks 2–4, streaks building | Streak counter, encouragement messages |
| Intermediate | Month 2, consistency forming | Weekly reflection prompt, domain Goal connection shown |
| Expert | Month 3, feels automatic | Reduced prompting, identity language surfaced |
| Complete | End of quarter | Full celebration ritual (see below) |

**Selection flow (quarter start):**
- Kompass surfaces prompt: *"New quarter — what habit would make the biggest difference?"*
- One suggestion based on current domain GYR + Goal progress
- Clint gut-checks: uh-huh to adopt, uh-uh to see two alternatives
- Previous habit optionally promoted to "installed identity" and removed from active tracking

**Streak mechanics:**
- Daily streak counter on the habit card
- Milestone acknowledgments at: Day 7 ("hardest week is behind you"), 14, 21, 30, 60, 90
- Each milestone generates a brief celebration message — not gamified, just acknowledged

**Quarter-end celebration:**
- Journal prompt: "You did it — what does it mean that you're now someone who [habit]?"
- Identity statement generated: *"You're now someone who rides 4x a week."*
- BAC Calendar Event created marking the quarter completion
- Optional Table Talk entry: share the win at dinner

**Success criteria:**
- Only one active Quarterly Habit at any time
- Stage advances automatically based on streak and time elapsed
- Quarter-end celebration fires on the correct date
- Identity statement generated and stored in Journal Entry

---

### F11 — Container Model — Empty State & Build Flow

**What it does:** Every section of the app exists from day one as an invitation. Empty containers have three visual layers (glow, invitation, progress ring) and a Claude-guided build flow that fills the container conversationally — no forms.

**Entities read:** All entities relevant to the container being built.

**Entities written:** Whatever the container requires — Goal record, GitHub markdown file, Key Doc JSON entry, etc.

**Three visual layers (always present on empty container):**
1. **Soft glow / shimmer** — animated gold border on the empty card — present but not urgent
2. **Invitation copy** — a short "why this matters" statement, not a feature description
3. **Progress ring** — around the section icon in the Me menu, fills as containers are completed

**Empty state copy examples:**

| Container | Invitation copy |
|---|---|
| Vivid Vision | *"You can't move toward something you haven't named. Ten minutes here shapes the next ten years."* |
| Quarterly Habit | *"One habit, fully installed, changes more than ten habits half-started. What's the one?"* |
| Misogi | *"What would you do this year if you knew you'd look back and say — I can't believe I did that?"* |
| Body Goals | *"Your body is the hardware that runs everything else. Where does it stand right now?"* |
| Key Docs | *"The documents that matter most are the hardest to find in a crisis. Ten minutes now saves hours later."* |

**Build flow:**
1. Clint taps empty container
2. "Why this matters" card appears
3. Uh-huh → Claude opens guided build conversation
4. One question at a time — never a form
5. Output filed to correct destination
6. Container resolves from empty to active — glow disappears, content appears, progress ring updates
7. Celebration: *"First one built — the machine is starting."*

**Refresh cycles (containers aren't built once):**

| Container | Refresh trigger |
|---|---|
| Vivid Vision | Annual — New Year prompt |
| Annual Commitments | Annual + quarterly check-in |
| Quarterly Habit | Quarter end |
| Misogi | Annual — start of year |
| Key Docs | User-triggered |
| Body Goals | Phase advancement |

**Success criteria:**
- Every section shows correct empty state on first use
- Build flow completes without any multi-field form
- Progress ring updates immediately on container completion
- Refresh triggers fire at correct cadences

---

### F12 — About Me + Vivid Vision

**What it does:** Surfaces Clint's full personal profile and the Vivid Vision / Annual Commitments documents in one section of the Me menu. Clint's operating manual, quick reference, 10-year vision, and 1-year commitments — all readable and searchable. Family profiles readable as reference context.

**Entities read:** Clint's Profile (#38) — 4 GitHub files. Family Profiles (#39) — 5 GitHub files (read-only). Vivid Vision (#36), Annual Commitments (#37).

**Entities written:** None in Phase 1. Phase 2: edits saved to Supabase.

**UX behavior:**
- Me menu → "About Me" → Clint's profile shown first
- Four tabs: Operating Manual / Quick Reference / Vivid Vision / 2026 Commitments
- Full markdown rendered — scrollable, not truncated
- Search bar — searches across all profile content simultaneously
- "Open in Google Doc" button on Vivid Vision tab — opens Drive link natively
- Last-updated date pulled from GitHub file metadata
- Family profiles: person selector (Christie / Avery / Brynn / Max / Gwen) — tap to read, read-only

**Vivid Vision sub-section:**
- 10-year vision organized by section: Health / Environment / Family / Business / Wealth & Legacy
- 2026 Commitments organized by domain: Rituals / Body / Being / Balance / Business
- Measurable commitments tap-through to their linked Goal card

**Daily Reminder Engine connection:** The Vivid Vision and Annual Commitments files are parsed into lines that feed the Daily Reminder Engine pool. One line per day, rotating.

**Success criteria:**
- All 4 Clint profile files render correctly from GitHub
- Google Doc link opens Drive natively without API auth
- Measurable commitment tap-through correctly navigates to linked Goal
- Family profile selector correctly loads each person's file

---

### F13 — Project + Tool Layer

**What it does:** Surfaces personal/family projects using the existing S-BOS project infrastructure, organized by the four universal pillars (Budget, Alignment, Schedule, Checklists). Claude builds custom tools scoped to specific project stages, which are used through their lifecycle and archived on completion.

**Entities read/written:** Project (#18), Check List (#19), Check List Task (#20), Project Budget G702 (#21), Budget Line Items G703 (#22), Baseline Budget Items (#23), Bills & Invoices (#24), Project Schedule (#25), Project Dates (#26), Project Tool (#28).

**UX behavior:**

**Project list view:**
- Me menu → "Tools" → Projects tab
- Active projects listed with domain tag, current pillar status, and tool count
- Tap project → four-pillar view: Budget / Alignment / Schedule / Checklists
- Each pillar shows its current status and any attached tools

**Tool creation:**
- Entry point A: inside a pillar → "Build a tool for this stage" → Claude conversation → tool attaches to that pillar
- Entry point B: Shortcuts tab → Claude skill trigger → "Build me a [tool type] for [project]" → Kompass identifies the right project and pillar → attaches
- Claude generates the tool as an HTML artifact
- Tool rendered inside the app as an embedded view

**Tool lifecycle:**
- Create → Active (used daily/weekly during stage) → Complete (stage ends) → Archived (stored with project)
- Archive prompt fires when a project stage closes
- Archived tools browsable from the project record

**Archive as library:**
- Completed tools searchable by type, domain, project name
- A study app built for one exam can be surfaced when a similar exam starts
- A medication schedule can be found when a new protocol is needed

**Success criteria:**
- Project hierarchy (Master/Child/Grandchild) renders correctly from SmartSuite
- Four pillars surface correct data from SB Project MGT tables
- Tool creation completes in < 5 Claude exchanges
- Archived tools are findable by type and domain

---

### F14 — Kompass Operating Platform

**What it does:** Three-layer system that externalizes everything requiring sustained attention, sequential processing, or remembering — removing cognitive load from Clint and routing it to the correct destination automatically. Second Brain (capture), Buffer Anchor (clear), Genius Schedule (protect).

**Entities read:** All entities — Kompass reads the full state of the system.

**Entities written:** Task (#7) — new tasks from email triage. Journal Entry (#8) — from capture. Decision (#10) — from capture. Principles & Realizations (#11) — from capture. BAC Calendar Events (#13) — from capture. Notes & Comments (#27) — email thread records.

**Layer 1 — Second Brain (Capture):**
- `/capture` activated from anywhere: typed text, voice, photo
- Nine-type classification routing:

| Input signals | Destination |
|---|---|
| Action verb + person + deadline | Check List Task |
| Emotional content, first-person processing | Journal Entry |
| Speculative, "what if" | Principles & Realizations |
| Named person + message intent | Held for Buffer session |
| Competing options, open question | Decision |
| Frustration, anger, friction language | Flagged for next Morning Ritual |
| Factual update about people/business | GitHub Clint-s-Kompass |
| Date + time + meeting reference | BAC Calendar Event |
| Someone needs something from Clint | Check List Task + Notes thread |

- Ambiguous: Claude offers two options with a gut check — never open-ended

**Layer 2 — Buffer Anchor (Clear):**
- Email triage: Gmail → Kompass reviews → creates Task + Notes thread → surfaces in Buffer session with draft reply attached → Clint approves → Kompass sends
- `Who` field privacy rule: task only gets a person tag when the task genuinely involves them
- Buffer session runs Tuesday (team meeting) and Wednesday (development meeting)
- Two guardrail questions on any commitment decision: *"What do you actually want here?"* and *"Is this a people-pleasing tendency or a hell yes?"*

**Layer 3 — Genius Schedule (Protect):**
- Morning: reviews day against day type, flags violations, confirms Big 3 are protected
- Weekly Monday audit: Focus Day violations, Buffer Days without clearing sessions, Free Days with business creep

**Success criteria:**
- Capture correctly classifies > 90% of inputs without clarification needed
- `Who` field is never populated without explicit Clint intent
- Buffer session surfaces items with draft replies attached, not just raw email content
- Monday audit correctly identifies week-type violations

---

### F15 — Shortcuts Tab

**What it does:** Provides single-tap access to every Claude skill and external tool Clint uses regularly — organized by Personal and Business categories. Tap a skill → prompt copied, Claude project opens. Tap an external tool → deep link opens the app directly.

**Entities read:** None — pure navigation/launch feature.

**Entities written:** None.

**UX behavior:**
- Four sections: Personal Skills / Business Skills / External Tools / Recent
- Personal Skills (19): Morning Ritual, Evening Ritual, Weekly Review, WAR/Cash/Irritation/Anger/Guilt/Gratitude/Excitement/Discovery Stacks, Free Write, Strategic Vision, Project Debrief, Horizon Scan, Quick Capture, Relationship Coach, Buffer Session, Email Triage
- Business Skills (17): Baseline a Project, Pay App, Pay App Audit, Underwrite a Deal, Strategic Review, GP Cash Flow, Entity Budget, Refresh Actuals, Year-End Audit, Interco Review, Platform Performance, Compliance Audit, Schema Registry, Data Dictionary, Training Docs, Skill Management, Duplicate Contact Merge
- External Tools (8): Google Calendar, Gmail, Google Drive, Strava, S-BOS (SmartSuite), Phase Anchor, Sage Intacct, GP Cash Flow (Railway)
- Recent section: last 5 used skills, surfaced at top
- Search bar: filter by name

**Success criteria:**
- All 36 skills launch correctly
- All 8 external tools deep-link correctly (no generic app open — specific app section where possible)
- Recent section updates after each use
- Search filters correctly within < 200ms

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

*Sections A–O captured in full in previous commits. Summarized here.*

**A** Navigation & Shell. **B** Day Mode. **C** Horizon Rings. **D** Kompass Operating Platform. **E** Universal Goal Engine. **F** Shortcuts Tab. **G** In-App Spec Sheet. **H** About Me. **H1** Vivid Vision & Annual Commitments. **I** Big Ass Calendar. **J** Quarterly Habit. **K** Key Docs. **L** Container Model & Learning Engine. **M** Brand Identity. **N** Project-Based Tool Layer. **O** Stat Inference Engine.

*Full Discovery Input detail preserved in TSW_memory.md and prior commit history.*
