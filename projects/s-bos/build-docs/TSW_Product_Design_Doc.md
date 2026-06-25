# Product Design Doc: Stitser Way

> **Status:** 🔄 In Progress — Gate 2 ✅ COMPLETE. §4 ✅ COMPLETE. §5 User Workflows in progress.
> **Methodology:** Ryan Falke's Design Templates, adapted for Stitser Way
> **Decision-maker:** Clint Stitser
> **Last updated:** 2026-06-25

---

## Gate System

| Gate | Sections | Status |
|---|---|---|
| Gate 1 | §1 Problem + §2 Users | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 2 | §3 Core Entities | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 3 | §4 Features + §5 Workflows | 🔄 §4 ✅ approved. §5 in progress. |
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

> **21 features — ✅ Approved by Clint 2026-06-25**

---

### F01 — Day Mode Engine
**Entities read:** BAC Calendar Events (#13), Task (#7), Day Mode Log (#9). **Written:** Day Mode Log (#9).
Kompass suggests mode on open. Uh-huh/uh-uh. Focus: tab bar hides, anchor + Pomodoro, break-glass. Buffer: full app, Horizon command center, guardrail questions. Free: calendar + wins + Table Talk, *"Your job today is to wander."*
**Success:** Mode < 3s. Log created. Tab bar correct. Free Day no tasks/inbox.

---

### F02 — Horizon Rings
**Entities read:** Check List Tasks (#20), Notes & Comments (#27), GYR Status Reports (#6), Goals (#1). **Written:** Check List Tasks (#20), Stats (#4).
Five rings. Priority stack. Phase Context Strip. Stacked / Circles toggle. Sacral Anchor pinned all day. Quick Clear.
**Success:** Never > 10 items. Toggle without reload. Anchor persists.

---

### F03 — Stat Inference Engine
**Entities read:** Strava (#29), Journal Entry (#8), BAC Calendar Events (#13), Goal (#1), Stat Menu Item (#5). **Written:** Stat (#4).
Claude scans after events. Matches by person + activity. One prompt, Yes/No/Not quite. Queue one at a time.
**Success:** Match rate > 85%. Prompt < 5min. No forms. Goal % updates.

---

### F04 — Universal Goal Engine
**Entities read/written:** Goal (#1), Priority (#2), Milestone (#3), Stat (#4), Stat Menu Item (#5), GYR Status Report (#6).
Five steps. Claude conversation, one question at a time. Goal card: progress ring + score + GYR grade.
**Success:** Setup < 5 exchanges. Score always current. GYR reflects latest.

---

### F05 — GYR Spiral
**Entities read:** Goal (#1), Priority (#2), GYR Status Report (#6). **Written:** GYR Status Report (#6), Journal Entry (#8).
Six steps. Entry from icon, Goal card, or Kompass. Prior answers visible. Prior Spiral context loaded. Grade on completion.
**Success:** < 15min. Prior context loads. GYR updates immediately.

---

### F06 — Learning Engine
**Entities read:** Learning Track (#43), Course (#42), Lesson (#41), Progress Record (#44). **Written:** Progress Record (#44).
In-App (6-hour safety rail) + External Practice (daily checkbox, no rail). Today tab card delivery.
**Success:** Safety rail works. External Practice independent. Progress ring updates.

---

### F07 — Daily Reminder Engine
**Entities read:** Vivid Vision (#36), Annual Commitments (#37), Principles & Realizations (#11). **Written:** Principles & Realizations (#11) on save.
One rotating thought per day. Three pools. No repeat within 7 days. Tied to morning ritual.
**Success:** No 7-day repeat. All pools render. < 1s display.

---

### F08 — Body Domain — Health Tracking & Vault
**Entities read:** Oura (#30–31), Apple Health (#32), Strava (#29), Drive records (#33–35), Goal (#1), Learning Track (#43). **Written:** Stat (#4), Drive records (#33–35).
Metrics Dashboard (weight, sleep, readiness, training, alcohol, meals). Training Log (Strava + race countdown + phase gates). Health Vault (Drive links).
**Success:** 7-day weight average correct. Oura refreshes on open. Drive links open natively.

---

### F09 — Big Ass Calendar
**Entities read:** BAC Calendar Events (#13), BAC Goals (#14), Goal (#1). **Written:** BAC Calendar Events (#13).
12 months, one screen. Backward (muted) + forward (full color) layers. Free Day view. Wins panel last 90 days.
**Success:** All 12 months without scrolling. Add event < 4 taps. Free Day correct.

---

### F10 — Quarterly Habit Arc
**Entities read:** Goal (#1), Stat (#4), Progress Record (#44), Learning Track (#43). **Written:** Goal (#1), Journal Entry (#8), BAC Calendar Event (#13).
One habit at a time. Five stages. Streak milestones. Quarter-end: identity statement + BAC milestone + optional Table Talk.
**Success:** Only one active. Stage auto-advances. Celebration fires. Identity statement stored.

---

### F11 — Container Model — Empty State & Build Flow
Three visual layers (glow + invitation copy + progress ring). Claude conversation build flow. Refresh cycles per container.
**Success:** Correct empty state. No multi-field forms. Progress ring updates. Refresh triggers correct.

---

### F12 — About Me + Vivid Vision
**Entities read:** Clint's Profile (#38), Family Profiles (#39), Vivid Vision (#36), Annual Commitments (#37).
Four tabs: Operating Manual / Quick Reference / Vivid Vision / 2026 Commitments. Full markdown. Search. Commitment tap-through to Goal. Family profiles read-only.
**Success:** All 4 files from GitHub. Google Doc opens natively. Tap-through works.

---

### F13 — Project + Tool Layer
**Entities read/written:** Project (#18), Check Lists (#19), Check List Tasks (#20), Budget (#21–24), Schedule (#25–26), Project Tool (#28).
Four-pillar view. Tool creation via pillar or Shortcuts. Embedded render. Archive as searchable library.
**Success:** Hierarchy from SmartSuite. Tool < 5 exchanges. Archive findable.

---

### F14 — Kompass Operating Platform
**Entities written:** Task (#7), Journal Entry (#8), Decision (#10), Principles & Realizations (#11), BAC Calendar Events (#13), Notes & Comments (#27).
Second Brain (nine-type capture routing). Buffer Anchor (email triage + `Who` privacy). Genius Schedule (morning review + Monday audit).
**Success:** > 90% classification. `Who` never auto-populated. Buffer surfaces drafts. Monday audit flags violations.

---

### F15 — Shortcuts Tab
36 Claude skills (19 personal + 17 business) + 8 external tools. Four sections + Recent + Search.
**Success:** All 36 launch. All 8 deep-link. Recent updates. Search < 200ms.

---

### F16 — Journal & Decisions Library
**Entities read:** Journal Entry (#8), Decision (#10). **Written:** Journal Entry (#8), Decision (#10).
Filterable by type/domain/date. Full-text search. FAB launcher. Day Mode scoreboard sub-view. Decisions sub-tab: Open/Resolved/Deferred + Claude-guided creation.
**Success:** Search < 500ms. All filters work. Scoreboard correct. FABs route correctly.

---

### F17 — Being Domain
**Entities read:** Goal (#1) Being, Priority (#2), Stat (#4), GYR (#6), Journal (#8), Learning Track (#43). **Written:** Stat (#4), Journal Entry (#8).
Rituals, mindset practices (External Practice), stacks, identity development. Domain card: GYR grade, Goals, Quarterly Habit, Learning Track, "Run Being Spiral."
**Success:** Goals render. Spiral pre-fills. Ritual streaks update from captures and External Practice.

---

### F18 — Balance Domain
**Entities read:** Goal (#1) Balance, Stat (#4), GYR (#6), Journal (#8), Family Profiles (#39), Strava (#29), BAC Calendar Events (#13). **Written:** Stat (#4) via Stat Inference Engine, Journal Entry (#8).
Relationship measurables, Table Talk, family profiles (read-only), connection Spirals. Domain card: GYR grade, measurable Goals, Table Talk shortcut, family avatars, "Run Balance Spiral."
**Success:** Measurables render. Inference logs in one tap. Table Talk < 4 taps. Spiral pre-fills.

---

### F19 — Business Domain
**Entities read:** Goal (#1) Business, Priority (#2), Stat (#4), GYR (#6), BAC Calendar Events (#13). **Written:** Stat (#4), GYR Status Report (#6), Journal Entry (#8).
Allocator/CEO view — not S-BOS operational, but personal strategic scoreboard. Domain card: GYR grade, Business Goals, product line sub-cards (entity + phase + GYR → Phase Anchor deep link), Business Misogi, "Run Business Spiral," Strategic Vision shortcut.
**Success:** Goals render. Product line sub-cards load GYR from SmartSuite. Phase Anchor deep links work. Spiral pre-fills.

---

### F20 — Week at a Glance
**Entities read:** BAC Day Types (#12), BAC Calendar Events (#13), Task (#7), Day Mode Log (#9). **Written:** BAC Day Types (#12).
7-day strip on Today tab. Each day: day type badge, up to 2 events, Big 3 anchor placeholder. Today highlighted. Past days muted. Tap future day → assign day type. Middle layer between BAC (year) and Horizon Rings (today). Buffer Day sub-tab shows strip as planning context.
**Success:** Strip renders with today highlighted. Day type badges from SmartSuite. Assign in 2 taps. Big 3 renders. Buffer sub-tab shows strip.

---

### F21 — Key Docs
**Entities read:** Key Doc (#40). **Written:** Key Doc (#40) — new entries.
**Entities read (cross-reference):** Drive records (#33–35) — Health category overlap.

**What it does:** One-tap access to critical personal documents stored in Google Drive. Not file storage — a named link registry organized by person and category. Solves object permanence for life's most important documents.

**UX behavior:**
- Me menu → "Key Docs"
- Person selector at top (Clint Phase 1, family Phase 2) + "Family" tab for shared docs
- Nine categories with icons: Identity 🪪 / Health 🏥 / Legal ⚖️ / Financial 💰 / Property 🏠 / Education 🎓 / Vehicle 🚗 / Travel ✈️ / Other 📄
- Each entry: name + category icon + Drive URL → one-tap opens Drive natively (no API auth)
- Emergency access flag — flagged docs surface first
- Add doc: name → category → Drive URL → optional notes → save
- Health category cross-references Body Health Vault (DXA reports, bloodwork, eye prescriptions appear in both places)

**Success criteria:**
- All Drive links open natively without API auth
- Emergency-flagged docs surface at top of list
- Add doc flow completes in < 4 taps
- Health Vault records correctly appear in Health category

---

## §4 Gate 3 Checklist

- [x] All 21 features named and described — ✅ Approved by Clint 2026-06-25
- [x] All features have entities read/write — ✅ Approved by Clint 2026-06-25
- [x] All features have success criteria — ✅ Approved by Clint 2026-06-25
- [x] No feature references an entity not in §3 — ✅ Approved by Clint 2026-06-25

---

## §5 — User Workflows

> 🔄 In progress.

### Workflow list (7 key flows):

| # | Workflow | Features involved |
|---|---|---|
| W01 | Morning Launch | F01, F07, F06, F02, F20 |
| W02 | Buffer Day Sweep | F01, F02, F20, F14 |
| W03 | Logging a Stat (inference path) | F03, F04 |
| W04 | Running the GYR Spiral | F05, F04 |
| W05 | Setting a New Goal | F04, F11 |
| W06 | Building a Project Tool | F13, F15 |
| W07 | Quarter Start — New Habit | F10, F06, F04 |

---

### W01 — Morning Launch

**Trigger:** Clint opens the app to start the day.

**Steps:**

1. **App open → Day Mode suggestion (F01)**
   Kompass reads calendar, Horizon Ring counts, and days since last Buffer/Free. Surfaces one suggestion: *"Looks like a Focus Day — team meeting at 10, 2 overdue, last buffer was Tuesday."* Clint: uh-huh or uh-uh. Mode confirmed. Day Mode Log created.

2. **Daily Reminder Engine fires (F07)**
   One thought from the pool surfaces: a Vivid Vision line, a commitment, or a Principle. Clint reads it. Optional save. Tied to the ritual open — the day starts with intention.

3. **Today's lesson surfaces (F06)**
   If an active Learning Track has a lesson due, the lesson card appears. In-App: Clint taps through. External Practice: checkbox prompt (*"Did you do your Calmio session?"*). Takes < 3 minutes.

4. **Week at a Glance visible (F20)**
   7-day strip shows the week's shape — today highlighted, upcoming day types visible. Clint can see the week as a whole before diving into today.

5. **Horizon Rings scan (F02)**
   Clint reviews what has pull today. Sacral Anchor prompt: *"What has the most pull?"* Selects one item. Pinned as the day's anchor — becomes Domino #1 in Big 3.

6. **Day launches**
   Focus Day: tab bar hides, anchor + Pomodoro visible. Buffer Day: Horizon Rings command center active. Free Day: Big Ass Calendar + wins panel.

**Duration:** 5–10 minutes. Every step is one-tap or one-swipe. No forms.

---

### W02 — Buffer Day Sweep

**Trigger:** Clint confirms a Buffer Day (F01). Runs Tuesday and Wednesday.

**Steps:**

1. **Week at a Glance review (F20)**
   Buffer Day sub-tab shows the week strip. Clint reviews remaining days — assigns or adjusts day types for the rest of the week. Sets Big 3 placeholder anchors for Focus Days ahead.

2. **Horizon Rings triage (F02)**
   Clint works through the Horizon Rings list. For each item: Done / Park / Kill / Snooze / act now. Kompass presents one item at a time with context and a draft action or reply.

3. **Email triage (F14)**
   Kompass surfaces the email queue from Gmail. Each email: context shown, draft reply attached. Clint: approve reply / edit / skip. Tasks created for any email requiring follow-up.

4. **Commitment check (F14)**
   For any item involving another person, two guardrail questions fire: *"What do you actually want here?"* and *"Is this a hell yes or people-pleasing?"* Clint responds. Action confirmed or declined.

5. **Big 3 set (F20)**
   Clint sets the three dominos for the day — the three moves that would make today a win. First domino is the Sacral Anchor from Horizon Rings.

6. **Stat Inference Engine check (F03)**
   Any pending inference prompts from the last 24 hours surface: *"Was Max on that ride?"* Clint confirms or skips. Stats logged.

**Duration:** 30–60 minutes. The week gets designed, not just survived.

---

### W03 — Logging a Stat (Inference Path)

**Trigger:** Clint completes a Strava ride, adds a journal entry mentioning a person, or a calendar event with a named person is marked complete.

**Steps:**

1. **Signal detected (F03)**
   Claude scans the new Strava activity (or journal entry / calendar event). Finds a person name + activity type. Checks active Balance Goals for a matching Stat Menu Item.

2. **Prompt surfaces**
   *"You just logged a 14-mile ride. Was Max with you? Count it toward 'Rides with Max Q3' (3 of 10)?"*

3. **Clint responds**
   - Yes → Stat record created. Goal % metric updated. Prompt dismissed.
   - No → Prompt dismissed. No record created.
   - Not quite → One clarifying question max (*"Was it a different family member?"*) → then log or dismiss.

4. **Goal card updates**
   The "Rides with Max Q3" Goal card in the Balance domain reflects the new count immediately.

**Duration:** < 30 seconds. One prompt, one tap.

---

### W04 — Running the GYR Spiral

**Trigger:** Clint notices a domain is Yellow or Red, Kompass proactively suggests, or Clint taps the Spiral icon.

**Steps:**

1. **Entry (F05)**
   Spiral icon tapped (or Goal card "Run Spiral" button). Domain selector appears. Clint selects Being, Balance, Body, or Business. If entering from a Goal card, domain and Goal pre-populated.

2. **Prior context loaded**
   If Clint has run a Spiral for this Goal before, the most recent one surfaces as context: *"Last time: root cause was sleep deprivation. Focus was alcohol reduction."*

3. **Six steps, one at a time**
   Claude presents each step conversationally. Clint responds in natural language. Prior answers visible as a running summary on the side:
   - **Facts:** What is actually true right now, by the numbers?
   - **Feelings:** What is the emotional response to those facts?
   - **Root Cause:** What is actually driving this?
   - **Focus:** The one thing to change.
   - **Actions:** Massive, relevant, specific.
   - **Fruit:** What success looks like.

4. **Grade selected**
   Clint assigns: Green / Yellow / Red. Claude may suggest based on the session content.

5. **Filed**
   GYR Status Report created in SmartSuite. Journal Entry created simultaneously (type = "Spiral," domain tagged). GYR grade on the linked Goal card updates immediately.

**Duration:** 10–15 minutes.

---

### W05 — Setting a New Goal

**Trigger:** Clint taps "Add Goal" in a domain, or an empty container in the Me tab is tapped.

**Steps:**

1. **Empty container invite (F11)**
   If the domain is empty: "why this matters" card shown. Uh-huh → Claude opens build conversation.
   If adding to an existing domain: "Add Goal" tapped → same conversation flow begins.

2. **Claude conversation — five steps (F04)**
   One question at a time:
   - *"What are you trying to achieve? Give me a clear outcome."*
   - *"What does success look like, in numbers? What's the target?"*
   - *"By when?"*
   - *"How often do you want to track progress — weekly or monthly?"*
   - *"What would you need to do this quarter to stay on pace?"* → Priority created.

3. **Goal card created**
   Appears in the domain with: title, target, due date, progress ring at 0%, GYR grade (no grade yet — shown as grey).

4. **Stat Menu Item linked**
   Claude asks: *"What are you tracking? I'll set up the stat counter."* → Stat Menu Item created or selected from existing catalog.

5. **Learning Track offered (F06)**
   *"Want me to set up a learning sequence for this goal? I can create lessons connected to the why."* Uh-huh → Learning Track scaffolded.

**Duration:** < 10 minutes. No forms. Goal is live and trackable.

---

### W06 — Building a Project Tool

**Trigger:** Clint is inside a project pillar and taps "Build a tool" — or triggers from Shortcuts: *"Build me a budget tracker for the Europe trip."*

**Steps:**

1. **Context identified**
   If from inside a pillar: project and pillar already known.
   If from Shortcuts: Kompass asks one question — *"Which project is this for?"* — Clint names it. Kompass identifies the project record and correct pillar.

2. **Tool type determined**
   Claude asks: *"What do you need it to do?"* Clint describes in natural language. Claude proposes a tool type (budget tracker, checklist, study app, etc.) and confirms: *"I'll build a budget tracker with categories and a running total — sound right?"* Uh-huh.

3. **Tool built**
   Claude generates an HTML artifact. Tool renders embedded inside the project pillar view.

4. **Tool goes Active**
   Lifecycle stage set to Active. Tool accessible from the project card. Relevant tasks surface in Horizon Rings.

5. **Archive on completion**
   When the project stage closes, archive prompt fires: *"This tool has served its purpose — archive it?"* Uh-huh → stage = Archived. Searchable and reusable.

**Duration:** < 5 Claude exchanges. Tool live in < 5 minutes.

---

### W07 — Quarter Start — New Habit

**Trigger:** Quarter end (Jan 1, Apr 1, Jul 1, Oct 1). Kompass surfaces the quarter-start prompt.

**Steps:**

1. **Previous habit reviewed**
   If a previous Quarterly Habit is in progress: *"Last quarter's habit was [X] — stage: Expert. Want to promote it to 'installed identity' and start fresh?"* Uh-huh → old habit archived with identity statement. New quarter begins.

2. **New habit selection (F10)**
   Kompass surfaces one suggestion based on current domain GYR and Goal progress: *"Your Body domain is Yellow and your morning ritual streak is 4 days. What if this quarter's habit was the morning ritual?"* Uh-huh (confirms) or uh-uh (two alternatives offered).

3. **Goal created (F04)**
   Quarterly Habit Goal record created. Domain tagged. Target frequency set. Stage = Install.

4. **Learning Track activated (F06)**
   A Learning Track scaffolded for this habit. First lesson queued for tomorrow. External Practice checkbox set up if applicable (e.g., Calmio daily).

5. **Day 1 prompt**
   Tomorrow morning: the lesson card appears on the Today tab. *"Day 1 of [habit] — here's why this matters."* First lesson delivers the why.

6. **Streak begins**
   Daily progress tracked. Milestone acknowledgments at Day 7, 14, 21, 30, 60, 90.

7. **Quarter-end celebration (F10)**
   At 90 days: Journal prompt, identity statement generated (*"You're now someone who starts every day with intention."*), BAC milestone marked.

**Duration:** Quarter start setup < 5 minutes. Then daily: < 3 minutes per day.

---

## §5 Gate 3 Checklist

- [ ] All key workflows documented ✳️ *Pending sign-off*
- [ ] All workflows are walkable end to end ✳️ *Pending sign-off*
- [ ] All workflows reference only features in §4 ✳️ *Pending sign-off*

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
