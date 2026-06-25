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
- External APIs: Strava MCP, Oura REST API (PAT), Apple HealthKit bridge (read-only)
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
- Per-member Table Talk entry
- Family Key Docs section
- Shared project access
- Tool sharing across family members
- Kids' data autonomy
- Phase 2 data layer: Supabase replaces SmartSuite for personal/family data

### Build sequence within Phase 1

**Sprint 1 — Shell + Today:** F23, F01, F07, F20, F11
**Sprint 2 — Goals + Rings:** F04, F02, F05
**Sprint 3 — Domains:** F08, F17, F18, F19
**Sprint 4 — Intelligence layer:** F03, F06, F14
**Sprint 5 — Planning layer:** F09, F10, F12
**Sprint 6 — Tools + Library:** F13, F15, F16, F21, F22

---

## §7 — Success Metrics

**Daily:** Mode confirmed < 30s. Thought + lesson visible without navigation. Week shape in one glance. Sacral Anchor < 2min.

**Weekly:** Full Buffer session clears Horizon Rings, email queue, and sets day types. Day Mode scoreboard shows correct counts.

**Monthly:** All domain Goals have current GYR grades. Relationship measurables tracking without forms. Lesson card tapped more days than not.

**Quarterly:** One active Quarterly Habit. Misogi set up as Project. Kevin's Rule slots visible in BAC.

**Annually:** Vivid Vision + Commitments reviewed and updated. Prior Quarterly Habits have identity statements.

**Qualitative signal:** Clint says the app feels like a partner, not a tool.

**Anti-metrics:** Forms filled manually > 2x/week. Horizon Rings > 10 items regularly. Domain with no Spiral in 30+ days. Same daily thought appearing twice in 7 days. Opening separate chat window to run a ritual.

---

## §8 — Timeline

| Milestone | What it means |
|---|---|
| M1 — Shell live | F23 + F01 + F07 + F20 + F11. First real daily use. |
| M2 — Goals + Rings live | F04 + F02 + F05. Core data loop live. |
| M3 — All four domains live | F08 + F17 + F18 + F19. All domain scorecards visible. |
| M4 — Intelligence layer live | F03 + F06 + F14. Machine doing work without being asked. |
| M5 — Full Phase 1 live | All 23 features. Stitser Way is primary daily OS. Phase 2 design begins. |

Phase 2 trigger: Phase 1 has been Clint's daily driver for one full quarter and passed the qualitative success signal.

---

## §9 — Open Questions

| # | Question | Blocks | Status |
|---|---|---|---|
| OQ01 | Goal Type field values in SmartSuite — how are domains tagged? | Domain filtering F04, F17–F19 | ⏳ Clint — pull schema |
| OQ02 | Family profile auth for Phase 1 — how does Clint read family GitHub files? | F12 | ⏳ Tech Spec |
| OQ03 | Strava sync frequency — on-demand or background webhook? | F03 timing, F08 freshness | ⏳ Tech Spec |
| OQ04 | Claude API integration — server action vs. edge function? | All Claude features | ⏳ Tech Spec |
| OQ05 | Phase 1 write-back scope — which SmartSuite tables? | All write operations | ⏳ Tech Spec |
| OQ06 | Gwen Gifford — About Me or Extended Family section? | F12 | ⏳ Clint |
| OQ07 | Learning Engine visual metaphor — mountain trail, ridgeline, or climb profile? | F06 visual design | ⏳ Clint |
| OQ08 | Domain rename for public brand? | All domain labels | ⏳ Clint — evolving |
| OQ09 | Day Mode Log automation timing — morning or end of day? | F01 | ⏳ Tech Spec |
| OQ10 | **Oura API — personal access token (PAT) confirmed for Phase 1.** Claude Code builds the REST integration. PAT stored as Railway env variable. Endpoints: daily sleep, readiness, activity, HR, SpO2. | F08 | ✅ Resolved 2026-06-25 |
| OQ11 | **Apple Health — HealthKit bridge confirmed for Phase 1.** HealthKit cannot be accessed from a mobile web app directly. Claude Code builds a lightweight iOS companion (Swift/React Native) that reads HealthKit and pushes data to the Railway backend via a simple REST endpoint. The web app reads weight data from Railway — not HealthKit directly. Phase 1 scope unchanged. | F08 weight data | ✅ Resolved 2026-06-25 |
| OQ12 | Key Doc storage — JSON config vs. lightweight Supabase? | F21 | ⏳ Tech Spec |
| OQ13 | Spec Sheet storage — local, GitHub, or Supabase? | F22 | ⏳ Tech Spec |
| OQ14 | Project Tool archive — GitHub Gist, Supabase blob, or SmartSuite attachment? | F13 | ⏳ Tech Spec |

---

## §9 Gate 4 Checklist
- [x] Scope defined ✅ Approved by Clint 2026-06-25
- [x] Build sequence defined ✅ Approved by Clint 2026-06-25
- [x] Success metrics defined ✅ Approved by Clint 2026-06-25
- [x] Anti-metrics defined ✅ Approved by Clint 2026-06-25
- [x] Timeline milestones defined ✅ Approved by Clint 2026-06-25
- [x] Open questions captured ✅ Approved by Clint 2026-06-25
- [x] OQ10 + OQ11 resolved 2026-06-25

---

## ✅ PDD COMPLETE

**All four gates passed. Approved by Clint 2026-06-25.**

| Document | Purpose | Input from PDD |
|---|---|---|
| **Data Integration Doc** | Field-level mapping for all 44 entities | §3 + OQ01–OQ05 |
| **Technical Spec** | Architecture, API patterns, auth, deployment | §6 + §9 |
| **UI/UX Doc** | Screen wireframes, components, mobile-first layout | §4 + F23 |

**Hand off to Claude Code with this PDD as the source of truth.**

---

## Discovery Inputs (from session 2026-06-24 / 2026-06-25)

*A–O captured in full in prior commits. Full detail in TSW_memory.md.*
