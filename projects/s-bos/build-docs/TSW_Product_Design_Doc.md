# Product Design Doc: Stitser Way

> **Status:** 🔄 In Progress — Section 1 drafted, Gate 1 pending sign-off
> **Methodology:** Ryan Falke's Design Templates, adapted for Stitser Way
> **Decision-maker:** Clint Stitser
> **Last updated:** 2026-06-24

---

## Gate System

| Gate | Sections | Condition to advance |
|---|---|---|
| Gate 1 | §1 Problem + §2 Users | Clint approves both |
| Gate 2 | §3 Core Entities | All entities named, no orphans |
| Gate 3 | §4 Features + §5 Workflows | All features have success criteria; all workflows are walkable |
| Gate 4 | §6 Scope + §7 Metrics + §8 Timeline + §9 Open Questions | All open questions resolved or deferred with owner |
| ✅ PDD Done | All gates passed | Data Integration Doc + Tech Spec + UI/UX Doc can begin |

---

## §1 — Problem Statement

### The problem

Clint has a working personal goal-tracking infrastructure in SmartSuite (the Game App — Goals, Priorities, Stats, GYR, Milestones). A Softr portal surfaces it today. But the Softr UI has three fundamental limitations:

1. **No LLM integration.** Claude cannot be woven into the daily experience — no Spiral processing, no day-mode suggestions, no coaching, no context-aware reminders. Claude lives in a separate chat window and has no awareness of what's happening in the data.

2. **No family profiles.** Softr cannot support per-member profiles with age-appropriate autonomy. The personal app needs to serve Clint differently than it serves Brynn or Max — and each family member needs to own and enter their own data.

3. **Softr constraints.** UI is limited to Softr's component library. Mobile experience is compromised. Navigation, day-mode behavior, dual-view screens (e.g., Horizon Rings stacked/circles toggle), and the universal Goal card pattern cannot be built without fighting the platform.

### What a good solution looks like

- A mobile-first web app that surfaces the existing SmartSuite Game App data through a purpose-built UI
- Claude integrated natively — the Spiral runs inside the app, day-mode is suggested and confirmed in-app, coaching appears in context
- Family profiles with per-member data ownership
- Day-mode aware: the app changes posture based on Focus / Buffer / Free day type
- Every life domain (Body, Being, Balance, Business) has goals, phases, progress tracking, rituals, and Claude-assisted processing — all in one place
- Built on the same Railway/Supabase stack as S-BOS so it can migrate off SmartSuite when that infrastructure is ready

### What this is NOT

- Not a replacement for S-BOS (the business operating system — that's a separate app)
- Not a new data model — the SmartSuite Game App infrastructure is preserved and extended
- Not a native mobile app (Phase 1 is a mobile-optimized web app)
- Not a multi-tenant product (Phase 1 is personal, for the Stitser family only)

---

## §1 Gate 1 Checklist

- [ ] Problem clearly stated — Clint confirms this is the real problem ✳️ *Pending sign-off*
- [ ] Solution criteria stated — Clint confirms these are the right criteria ✳️ *Pending sign-off*
- [ ] Scope boundaries stated — Clint confirms what this is NOT ✳️ *Pending sign-off*

---

## §2 — Target Users

### Primary user: Clint Stitser

- Personal operating system, daily driver
- Uses the app across all four life domains: Body, Being, Balance, Business (personal)
- Day-mode aware: Focus Day (deep work), Buffer Day (Kompass-assisted clearing), Free Day (incubation)
- Interacts with Claude for Spiral processing, day-mode confirmation, reminders, and coaching
- Runs morning ritual, evening ritual, weekly review, and stacks (WAR, Cash, Irritation, etc.) through the app
- Operating manual profile: interest-based nervous system, object permanence challenges, consecutive appetite (one thing at a time), Channel of Inspiration (1-8), non-linear thinking is a structural advantage

### Secondary users: Family members

| Member | Role | Notes |
|---|---|---|
| Christie | Partner | Full profile, own data |
| Avery | Child | Own profile, age-appropriate autonomy, owns their data |
| Brynn | Child | Initiated the Table Talk ritual. Own profile, owns their data |
| Max | Child | Own profile, owns their data |

**Design principle:** Kids enter their own data. The app is intrinsically motivating — not parent-assigned homework. Customized to each family member's wiring and learning style.

### User needs by domain

| Domain | What Clint needs the app to do |
|---|---|
| Body | Track weight, body fat, meals, alcohol, training rides (Strava), health records. Phase-based protocol (Foundation → Engine → Race Block). Reminders and staged learning connected to the why. |
| Being | Morning/evening rituals, stacks, affirmations, decisions, mindset tracking. Spiral processing. Phase gate for being/mindset development. |
| Balance | Family coordination, Table Talk (Hi/Lo/Buffalo), relationship tracking, gratitude, celebrations. Christie + kids profiles. |
| Business | Personal business goals — separate from S-BOS team goals. Phase anchor for Clint's role as allocator/CEO. GYR status per product line from his personal vantage point. |

---

## §2 Gate 1 Checklist (continued)

- [ ] All user types identified ✳️ *Pending sign-off*
- [ ] Primary user needs per domain stated ✳️ *Pending sign-off*
- [ ] Family profile model confirmed ✳️ *Pending sign-off*

---

## §3 — Core Entities

> ⏳ Not started. Begins after Gate 1 sign-off.

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

## Discovery Inputs (from session 2026-06-24)

> These are confirmed design decisions from the discovery session. They will be formalized into §3–§5 once Gate 1 is signed off. Listed here so nothing is lost.

### Navigation
- Four tabs: Today / Horizon / Me / Shortcuts
- Me tab is a domain menu — four domain cards, tap to drill into Goals, tap Goal to get universal initiative screen
- Spiral and Spec accessible from top-right nav icons (not main tabs)

### Day Mode
- Three modes: Focus Day / Buffer Day / Free Day
- Kompass reads: calendar events, Horizon ring counts, days since last buffer/free
- Makes one suggestion with reasoning
- Clint responds: uh-huh (confirms) or uh-uh (triggers three-option picker)
- Max two questions, never open-ended
- Focus: tab bar hides, only anchor + timer visible, break-glass available
- Buffer: full app visible, Horizon Rings is command center, email digest + Big 3 sub-tabs
- Free: calendar + wins + Table Talk prompt only, mandate to wander

### Horizon Rings
- Five rings: Overdue / This Week / Active / Coming Soon / Parked
- Sources: SmartSuite Tasks + Notes/Comments + GYR Follow-Ups
- Default view: Stacked list (full triage functionality)
- Overview view: Concentric circles (spatial orientation, tap dot to inspect, tap ring to filter)
- Toggle between views: single icon button top-right, not a tab
- Bridge: tap "→ Focus" in circles view → returns to stacked with that item highlighted
- Sacral Anchor prompt: "What has the most pull for you today?" — surfaces top 3 actionable items
- Quick Clear mode: bulk triage for parked/inactive items
- Phase Context Strip: shows current phase status per product line above rings
- Actions per item: Done / Park / Kill / Snooze / Pin as Anchor

### Universal Goal Engine (five steps)
1. Current Score — where you are now vs target
2. New Goal + Deadline — what you're aiming for, by when
3. Rhythm & Reminders — reporting frequency, chunked execution, staged learning
4. Progress Tracking — visual + numerical, SmartSuite Stats as the data source
5. Celebrate Wins — ritual of confirmation, GYR grade, scoreboard update

### Body Section (first domain built out)
- Three phases: Foundation (sleep + alcohol + protein baseline) → Engine (strength + polarized riding + green-MED diet) → Race Block (maintenance + race-fueled)
- Phase gates with explicit "how we know" criteria
- Trackers: weight (daily log → weekly trend), body fat % (DXA upload), waist circumference, meals (Mediterranean green protocol), alcohol (drinks per day, streak), training (Strava pull)
- Reminders: staged learning connected to the research protocol (why visceral fat, why protein, why sleep)
- Health vault: DXA reports, blood tests, eye prescriptions, skin records — file upload + date
- Strava integration: training log, ride metrics (watts, elevation, TSS), race countdown

### Table Talk
- Two formats: Hi / Lo / Buffalo (positive / negative / surprise) and Rose / Thorn / Bud
- Brynn-initiated dinner table ritual
- Family member selector — tap to see each person's entry
- Lives on Today tab and in Journal history (type: Table Talk)
- No journaling required — dinner table IS the ritual

### Shortcuts Tab
- 28 Claude skills organized by type (Personal / Business)
- External tools grid (Google Calendar, Drive, Gmail, Strava, S-BOS, Intacct, Phase Anchor)
- Tap skill → prompt copied, Claude project opens

### Spec Sheet (living design doc)
- Built into the app as its own screen (not a separate document)
- Grouped by section with collapsible headers
- Each row: Feature / Principle & Framework / Tool / Status
- Status cycles: Designed → In Build → Live → Parked
- Cells are tap-to-edit inline
- Add row button per section
