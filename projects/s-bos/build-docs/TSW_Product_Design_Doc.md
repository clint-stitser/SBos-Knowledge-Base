# Product Design Doc: Stitser Way

> **Status:** 🔄 In Progress — Gate 2 ✅ COMPLETE. §4 ✅ COMPLETE (23 features). §5 ✅ COMPLETE. Gate 3 pending sign-off.
> **Methodology:** Ryan Falke's Design Templates, adapted for Stitser Way
> **Decision-maker:** Clint Stitser
> **Last updated:** 2026-06-25

---

## Gate System

| Gate | Sections | Status |
|---|---|---|
| Gate 1 | §1 Problem + §2 Users | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 2 | §3 Core Entities | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 3 | §4 Features + §5 Workflows | 🔄 §4 ✅ 23 features. §5 ✅ 7 workflows. Pending sign-off. |
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

**2. Absence.** No application designed around the actual frameworks.

**3. The activation gap.** Without a daily machine, knowledge fades, rituals drift.

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

> **23 features — ✅ Approved by Clint 2026-06-25**

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

Four-pillar view (Budget / Alignment / Schedule / Checklists). Tool creation via pillar or Shortcuts. Embedded render. Archive as searchable library.

**Project types — all use the same four-pillar infrastructure:**

| Project type | Domain | Pillar emphasis |
|---|---|---|
| Construction / development | Business | All four pillars — full S-BOS depth |
| Family trip | Balance | Budget + Schedule + Checklists |
| Medical protocol | Body | Alignment + Schedule + Checklists |
| Study / exam prep | Balance or Being | Alignment + Schedule + Checklists |
| **Misogi** | Any domain | Alignment (purpose + who) + Schedule (training arc + event date) + Checklists (gear, preparation gates) + Budget if relevant |
| **Kevin's Rule event** | Balance | Lightweight — Alignment (what + why) + Schedule (date + logistics) + Checklists (packing, booking) + Budget if relevant |

**Misogi as a Project (confirmed 2026-06-25):**
A Misogi is achieved using the Project + Tool Layer. The Misogi Goal record (entity #16, tagged `type = "Misogi"`) provides the north-star target and tracks completion. The linked Project record provides the infrastructure — the Alignment pillar captures the purpose and preparation requirements; the Schedule pillar holds the training timeline leading up to the event; the Checklists pillar holds gear, fitness gate criteria, and pre-event preparation. Claude builds custom tools within the pillars as needed (e.g., a training schedule calculator, a gear checklist, a fitness readiness tracker).

**Kevin's Rule as a Project (confirmed 2026-06-25):**
A Kevin's Rule event can be run as a lightweight Project. The Kevin's Rule Goal record (entity #17, tagged `type = "Kevin's Rule"`) marks the slot (1–6 for the year) and logs completion. The linked Project provides: Alignment (what's the experience, who's coming, what makes it new), Schedule (date, logistics), Checklists (booking, packing, preparation). Budget pillar optional — relevant for trips, not needed for local adventures. A Claude-built tool (e.g., a trip budget, a packing list) attaches to the relevant pillar.

**Success:** Hierarchy renders from SmartSuite. Misogi and Kevin's Rule Projects surface correctly in their respective domains. Tool creation < 5 exchanges. Archive findable.

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
7-day strip on Today tab. Each day: day type badge, up to 2 events, Big 3 anchor placeholder. Today highlighted. Past days muted. Tap future day → assign day type.
**Success:** Strip renders correctly. Day type badges from SmartSuite. Assign in 2 taps. Big 3 renders. Buffer sub-tab shows strip.

---

### F21 — Key Docs
**Entities read/written:** Key Doc (#40). Cross-reference: Drive records (#33–35).
Nine categories. Drive links open natively (no API auth). Emergency flag. Add < 4 taps. Health Vault cross-reference.
**Success:** All links open natively. Emergency docs first. Add < 4 taps. Health Vault appears in Health category.

---

### F22 — In-App Spec Sheet
**Entities read/written:** App config / local storage (Phase 1). Supabase (Phase 2).
Always one tap from top-right nav icon. Ten seed sections. Each row: Feature / Framework / Status (cycles Designed→In Build→Live→Parked) / Notes / Screenshot. Inline edit. Add row + section. Summary strip counts by status.
**Success:** Accessible from every screen. Status cycles correctly. Notes save. Screenshots attach. Summary strip updates immediately.

---

### F23 — App Shell & Navigation
**Entities read:** Day Mode Log (#9), BAC Day Types (#12). **Written:** None.

Four persistent bottom tabs: Today / Horizon / Me / Shortcuts. Tab bar hides on Focus Day.

**Today tab layouts by day mode:**

*Standard (Focus/Buffer):* Day Mode badge → Daily Reminder → Lesson card → Week strip → Big 3 dominos → Table Talk shortcut.

*Free Day:* "Your job today is to wander" → Big Ass Calendar year view → Wins panel → Table Talk shortcut.

*Focus Day:* Grounding affirmation → Anchor item → Pomodoro timer → Break-glass button. Tab bar hidden.

**Me menu:** Body → Being → Balance → Business → About Me → Key Docs → Big Ass Calendar → Quarterly Habit → Journal → Tools → Spec Sheet. Each section has a progress ring (Container Model).

**Top-right icons (all screens):** 🌀 Spiral → F05. 📋 Spec Sheet → F22.

**Success:** Tab bar correct on all screens. Today tab renders correct layout per day mode. Me menu progress rings correct. Top-right icons navigate correctly. Tab switch ≤ 1 tap.

---

## §4 Gate 3 Checklist

- [x] All 23 features named and described — ✅ Approved by Clint 2026-06-25
- [x] All features have entities read/write — ✅ Approved by Clint 2026-06-25
- [x] All features have success criteria — ✅ Approved by Clint 2026-06-25
- [x] No feature references an entity not in §3 — ✅ Approved by Clint 2026-06-25
- [x] Review complete — all Discovery Inputs A–O, all 44 entities, all 23 core decisions cross-checked. No gaps. ✅ 2026-06-25

---

## §5 — User Workflows

> ✅ W01–W07 complete. Pending Gate 3 sign-off.

| # | Workflow | Features involved |
|---|---|---|
| W01 | Morning Launch | F23, F01, F07, F06, F20, F02 |
| W02 | Buffer Day Sweep | F01, F02, F20, F14 |
| W03 | Logging a Stat (inference path) | F03, F04 |
| W04 | Running the GYR Spiral | F05, F04 |
| W05 | Setting a New Goal | F04, F11 |
| W06 | Building a Project Tool | F13, F15 |
| W07 | Quarter Start — New Habit | F10, F06, F04 |

*Full workflow step-by-step detail in prior commit. All 7 workflows walkable end to end. Misogi and Kevin's Rule run via W06 (Project Tool) + F04 (Universal Goal Engine) — no separate workflows needed.*

---

## §5 Gate 3 Checklist

- [ ] All key workflows documented ✳️ *Pending sign-off*
- [ ] All workflows walkable end to end ✳️ *Pending sign-off*
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
