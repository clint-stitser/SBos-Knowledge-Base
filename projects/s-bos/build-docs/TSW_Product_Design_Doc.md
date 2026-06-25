# Product Design Doc: Stitser Way

> **Status:** ✅ PDD COMPLETE — All gates passed. Ready for Data Integration Doc + Technical Spec + UI/UX Doc.
> **Methodology:** Ryan Falke's Design Templates, adapted for Stitser Way
> **Decision-maker:** Clint Stitser
> **Last updated:** 2026-06-25

---

## Gate System

| Gate | Sections | Status |
|---|---|---|
| Gate 1 | §1 Problem + §2 Users | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 2 | §3 Core Entities | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 3 | §4 Features + §5 Workflows | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 4 | §6 Scope + §7 Metrics + §8 Timeline + §9 Open Questions | ✅ Complete — approved by Clint 2026-06-25 |
| ✅ PDD Done | All gates passed | Data Integration Doc + Technical Spec + UI/UX Doc can begin |

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

> **23 features — ✅ Approved by Clint 2026-06-25**

F01 Day Mode Engine — F02 Horizon Rings — F03 Stat Inference Engine — F04 Universal Goal Engine — F05 GYR Spiral — F06 Learning Engine — F07 Daily Reminder Engine — F08 Body Domain Health Tracking & Vault — F09 Big Ass Calendar — F10 Quarterly Habit Arc — F11 Container Model — F12 About Me + Vivid Vision — F13 Project + Tool Layer (incl. Misogi + Kevin's Rule as project types) — F14 Kompass Operating Platform — F15 Shortcuts Tab — F16 Journal & Decisions Library — F17 Being Domain — F18 Balance Domain — F19 Business Domain — F20 Week at a Glance — F21 Key Docs — F22 In-App Spec Sheet — F23 App Shell & Navigation

*Full feature specs in prior commit. All 23 features have entities read/write and success criteria.*

---

## §4 Gate 3 Checklist
- [x] All 23 features named and described — ✅ Approved by Clint 2026-06-25
- [x] All features have entities read/write — ✅ Approved by Clint 2026-06-25
- [x] All features have success criteria — ✅ Approved by Clint 2026-06-25
- [x] No feature references an entity not in §3 — ✅ Approved by Clint 2026-06-25
- [x] Full review complete — all Discovery Inputs A–O, 44 entities, 23 decisions cross-checked. No gaps. ✅

---

## §5 — User Workflows

> **7 workflows — ✅ Approved by Clint 2026-06-25**

| # | Workflow | Features | Duration |
|---|---|---|---|
| W01 | Morning Launch | F23, F01, F07, F06, F20, F02 | 5–10 min |
| W02 | Buffer Day Sweep | F01, F02, F20, F14 | 30–60 min |
| W03 | Logging a Stat — Inference Path | F03, F04 | < 30 sec |
| W04 | Running the GYR Spiral | F05, F04 | 10–15 min |
| W05 | Setting a New Goal | F04, F11 | < 10 min |
| W06 | Building a Project Tool | F13, F15 | < 5 exchanges |
| W07 | Quarter Start — New Habit | F10, F06, F04 | < 5 min setup |

*Full step-by-step detail in prior commit. Misogi and Kevin's Rule run via W06 + F04.*

---

## §5 Gate 3 Checklist
- [x] All key workflows documented — ✅ Approved by Clint 2026-06-25
- [x] All workflows walkable end to end — ✅ Approved by Clint 2026-06-25
- [x] All workflows reference only features in §4 — ✅ Approved by Clint 2026-06-25

---

## §6 — Scope & Phasing

### Phase 1 — Clint Only (current build)

**In scope:**
- All 23 features as specified in §4
- All 44 entities as specified in §3
- All 7 workflows as specified in §5
- Single user — no auth complexity, no per-user data isolation
- SmartSuite as the Phase 1 data layer (read + write via Kompass MCP)
- GitHub API for profile and vision files (read-only)
- External APIs: Strava MCP, Oura API, Apple Health API (read-only)
- Google Drive links opened natively (no Drive API write)
- Claude via Anthropic API as the native intelligence layer
- Railway deployment — separate from S-BOS
- Next.js / React / TypeScript / Tailwind v4 — mobile-first web app

**Explicitly out of scope for Phase 1:**
- Family member profiles (interactive — read-only reference only)
- Per-member data isolation or authentication
- Family Table Talk entry (Clint records Phase 1)
- Supabase migration (Phase 2)
- Native mobile app (iOS/Android) — Phase 1 is a mobile-optimized web app
- Public-facing brand or multi-tenant access
- Offline mode
- Push notifications (browser notifications acceptable as Phase 1 fallback)

### Phase 2 — Family Expansion

**Adds:**
- Multi-user authentication (Supabase Auth)
- Per-member profiles with own data — Christie, Avery, Brynn, Max
- Age-appropriate UX per family member
- Per-member Table Talk entry (Hi/Lo/Buffalo from each person's profile)
- Family Key Docs section
- Shared project access (Kevin's Rule adventures, family trips)
- Tool sharing across family members
- Kids' data autonomy — each child owns their entries
- Phase 2 data layer: Supabase replaces SmartSuite for personal/family data

### Build sequence within Phase 1

The 23 features are not all equal in build complexity. Recommended sequence:

**Sprint 1 — Shell + Today (foundation everything else depends on):**
F23 App Shell, F01 Day Mode Engine, F07 Daily Reminder Engine, F20 Week at a Glance, F11 Container Model

**Sprint 2 — Core data layer (Goals + Rings):**
F04 Universal Goal Engine, F02 Horizon Rings, F05 GYR Spiral

**Sprint 3 — Domains:**
F08 Body Domain, F17 Being Domain, F18 Balance Domain, F19 Business Domain

**Sprint 4 — Intelligence layer:**
F03 Stat Inference Engine, F06 Learning Engine, F14 Kompass Operating Platform

**Sprint 5 — Planning layer:**
F09 Big Ass Calendar, F10 Quarterly Habit Arc, F12 About Me + Vivid Vision

**Sprint 6 — Tools + Library:**
F13 Project + Tool Layer, F15 Shortcuts Tab, F16 Journal & Decisions Library, F21 Key Docs, F22 In-App Spec Sheet

---

## §7 — Success Metrics

### Phase 1 is successful when Clint can do all of the following without friction:

**Daily (morning ritual):**
- [ ] Open app, get a day mode suggestion, confirm in < 30 seconds
- [ ] See today's rotating thought and lesson card without any navigation
- [ ] See the week's shape (day types + key events) in one glance
- [ ] Identify the day's Sacral Anchor in < 2 minutes

**Weekly (Buffer Day):**
- [ ] Triage all Horizon Rings items, approve email replies, and set the week's day types in a single Buffer session
- [ ] See Focus / Buffer / Free Day counts for the week in the Journal scoreboard

**Monthly:**
- [ ] Every active domain Goal has an up-to-date GYR grade
- [ ] At least one Spiral has been run per Yellow or Red domain
- [ ] Relationship measurables are tracking against targets without manual form entry
- [ ] Today's lesson card has been tapped more days than not

**Quarterly:**
- [ ] One Quarterly Habit is selected, in progress, and tracking
- [ ] The year's Misogi is set up as a Project with at least the Alignment and Schedule pillars populated
- [ ] Kevin's Rule slots are visible in the Big Ass Calendar

**Annually:**
- [ ] Vivid Vision and Annual Commitments are reviewed and updated
- [ ] The prior year's Quarterly Habits all have identity statements in the Journal

### Qualitative success signal:
Clint says the app feels like a partner, not a tool. He doesn't have to remember to use it — it surfaces what matters when it matters.

### Anti-metrics (signs the machine is failing):
- Clint is manually logging stats by filling out forms more than 2x per week
- Horizon Rings regularly has > 10 items
- A domain has had no Spiral and no GYR update in 30+ days
- The Daily Reminder Engine is showing the same thought twice in a 7-day window
- Clint is opening a separate chat window to run a ritual or stack instead of using the Shortcuts tab

---

## §8 — Timeline

> This is a direction, not a commitment. Clint decides actual milestones when Claude Code handoff is complete and the build team has reviewed the PDD.

### Phase 1 milestones

| Milestone | What it means |
|---|---|
| **M1 — Shell live** | F23 + F01 + F07 + F20 + F11 deployed. Clint can open the app, confirm a day mode, see a thought, and see the week strip. First real daily use. |
| **M2 — Goals + Rings live** | F04 + F02 + F05 deployed. Clint can set a Goal, see it in Horizon Rings, and run a Spiral. The core data loop is live. |
| **M3 — All four domains live** | F08 + F17 + F18 + F19 deployed. Clint can see all domain scorecards, log Body stats, view relationship measurables, and read his business phase. |
| **M4 — Intelligence layer live** | F03 + F06 + F14 deployed. Stat Inference Engine running, Learning Engine delivering lessons, Kompass routing captures. The machine is doing work without Clint asking. |
| **M5 — Full Phase 1 live** | All 23 features deployed. Clint uses Stitser Way as his primary daily operating system. Phase 2 design begins. |

### Phase 2 trigger
Phase 2 begins when Phase 1 has been Clint's daily driver for one full quarter and has passed the qualitative success signal above.

---

## §9 — Open Questions

> These are decisions that must be resolved before or during the Technical Spec. Owner = Clint unless noted. Flag in `TSW_memory.md` when resolved.

| # | Question | Blocks | Owner |
|---|---|---|---|
| OQ01 | Goal Type field values in SmartSuite — how are Body/Being/Balance/Business tagged? | Me tab domain filtering in F04, F17, F18, F19 | Clint — pull schema |
| OQ02 | Family profile auth model for Phase 1 — how does Clint's session read family GitHub files? | F12 read-only family profiles | Tech Spec |
| OQ03 | Strava sync frequency — on-demand on app open, or background webhook? | F03 Stat Inference Engine timing, F08 Training Log freshness | Tech Spec |
| OQ04 | Claude API integration pattern — server action vs. edge function? | All features using Claude (F01, F03, F04, F05, F11, F14) | Tech Spec |
| OQ05 | Phase 1 write-back scope — which SmartSuite tables does the app write to, and which are read-only? | All SmartSuite write operations | Tech Spec |
| OQ06 | Gwen Gifford — does she appear in About Me or a separate Extended Family section? | F12 family profiles | Clint |
| OQ07 | Learning Engine visual progress metaphor — mountain trail, fire-lit ridgeline, or bike climb profile? | F06 progress ring visual design | Clint |
| OQ08 | Domain rename — what replaces Body/Being/Balance/Business for the public brand? | All domain labels across the app | Clint — evolving |
| OQ09 | Day Mode Log automation timing — fires on confirmation (morning) or end of day? | F01 Day Mode Log creation | Tech Spec |
| OQ10 | Oura API auth — personal token (Phase 1) or OAuth (Phase 2)? | F08 Oura data integration | Tech Spec |
| OQ11 | Apple Health API — requires native iOS app or can be accessed from a mobile web app? | F08 weight data source | Tech Spec — may force a Phase 1 scope change |
| OQ12 | Key Doc storage in Phase 1 — JSON config file in the repo, or a lightweight Supabase table from the start? | F21 Key Docs data layer | Tech Spec |
| OQ13 | Spec Sheet storage — local browser storage, GitHub file, or lightweight Supabase? | F22 notes + screenshots persistence | Tech Spec |
| OQ14 | Project Tool archive — where do Claude HTML artifacts live? GitHub Gist, Supabase blob, or SmartSuite file attachment? | F13 tool archive and retrieval | Tech Spec |

---

## §9 Gate 4 Checklist
- [x] Scope defined — Phase 1 in/out of scope clearly stated ✅ Approved by Clint 2026-06-25
- [x] Build sequence defined — sprint order established ✅ Approved by Clint 2026-06-25
- [x] Success metrics defined — qualitative + quantitative ✅ Approved by Clint 2026-06-25
- [x] Anti-metrics defined ✅ Approved by Clint 2026-06-25
- [x] Timeline milestones defined ✅ Approved by Clint 2026-06-25
- [x] Open questions captured with owners and blockers ✅ Approved by Clint 2026-06-25

---

## ✅ PDD COMPLETE

**All four gates passed. Approved by Clint 2026-06-25.**

**What comes next:**

| Document | Purpose | Input from PDD |
|---|---|---|
| **Data Integration Doc** | Field-level mapping for all 44 entities — exact SmartSuite field IDs, API endpoints, read/write permissions, sync strategy | §3 entities + OQ01–OQ05 |
| **Technical Spec** | Architecture decisions, API integration patterns, auth model, deployment config, data flow diagrams | §6 scope + §9 open questions |
| **UI/UX Doc** | Screen-by-screen wireframes, component library, interaction patterns, mobile-first layout spec | §4 features + F23 shell spec |

**Hand off to Claude Code with this PDD as the source of truth.**

---

## Discovery Inputs (from session 2026-06-24 / 2026-06-25)

*A–O captured in full in prior commits. Full detail in TSW_memory.md.*
