# Product Design Doc: Stitser Way

> **Status:** ✅ PDD COMPLETE — All gates passed. Ready for Data Integration Doc + Technical Spec + UI/UX Doc.
> **Methodology:** Ryan Falke's Design Templates, adapted for Stitser Way
> **Decision-maker:** Clint Stitser
> **Last updated:** 2026-06-26

---

## Gate System

| Gate | Sections | Status |
|---|---|---|
| Gate 1 | §1 Problem + §2 Users | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 2 | §3 Core Entities | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 3 | §4 Features + §5 Workflows | ✅ Complete — approved by Clint 2026-06-25 |
| Gate 4 | §6 Scope + §7 Metrics + §8 Timeline + §9 Open Questions | ✅ Complete — approved by Clint 2026-06-25 |
| ✅ PDD Done | All gates passed | Data Integration Doc + Technical Spec + UI/UX Doc can begin |

> **Post-approval additions:** Meetings capture (entity #45, feature F24) added 2026-06-26 during build — see the Addendum. Weight-capture workaround also added 2026-06-26 — see Addendum 2 + OQ16.

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

> 45 entities (44 approved 2026-06-25; Meetings #45 added 2026-06-26). Full definitions and app IDs in TSW_memory.md and prior commit history.

**Game App:** Goals (#1), Priorities (#2), Milestones (#3), Stats (#4), Stat Menu Items (#5), GYR Status Reports (#6)
**Stitser Way:** Journal Entries (#8), Day Mode Log (#9), Decisions (#10), Principles/Realizations (#11), BAC Day Types (#12), BAC Calendar Events (#13), BAC Goals (#14)
**Game App Goals tagged:** Quarterly Habit (#15), Misogi (#16), Kevin's Rule (#17)
**Tasks composite:** Check List Tasks + Notes & Comments + GYR follow-ups (#7)
**SB Project MGT:** Projects (#18–26), Notes & Comments (#27)
**Claude artifact:** Project Tool (#28)
**External APIs:** Strava (#29), Oura Sleep (#30), Oura Readiness (#31), Oura Weight (#32)
**Drive links:** Body Scan (#33), Bloodwork (#34), Eye Prescription (#35)
**GitHub:** Vivid Vision (#36), Annual Commitments (#37), Clint's Profile (#38), Family Profiles (#39)
**App config:** Key Doc (#40)
**SB Training & Certifications:** Lesson (#41), Course (#42), Learning Track (#43), Progress Record (#44)
**Meetings capture (added 2026-06-26):** Meeting (#45) — SmartSuite Meetings table `6a0cff32f77ad06285909dcf`, captured from Plaud + Google Meet. See Addendum.

> **Goal entity — Domain field (confirmed 2026-06-25):**
> Field name: Domain. Field ID: `s5deb9616e`. Type: Multi-select. Values: Body / Being / Balance / Business.
> Table: Game App Goals `6824d4d1885a8769bd2dfc0d`.
> **Data population required before Sprint 2:** Existing Goal records have no Domain value applied yet. Must be tagged before the app can filter Goals by domain. Options: (a) Clint manually tags each Goal in SmartSuite, or (b) Claude Code script batch-tags from context. Do before Sprint 2 starts.

> **Entity #32 clarification (2026-06-25):** Originally "Apple Health Weight Record." Updated to "Oura Weight Record" — Oura natively syncs weight from Apple Health. Single integration point. No iOS companion needed.

> **Entity #32 CORRECTION (2026-06-26):** The Oura REST API exposes **no weight endpoint**, so the assumed Apple Health → Oura → API weight path does **not** work. Phase-1 weight is captured by a personal workaround: **Siri Shortcut → Apple Health → Claude `log-weight` skill → Stats** (logged against the "Weight & BMI" priority). ⚠️ Personal-only — does **not** scale to licensed/multi-user. See Addendum 2 and OQ16.

---

## §3 Gate 2 Checklist
- [x] All Phase 1 entities named — ✅ Approved by Clint 2026-06-25
- [x] No orphan entities — ✅ Approved by Clint 2026-06-25
- [x] Phase 1 / Phase 2 boundary clear — ✅ Approved by Clint 2026-06-25

---

## §4 — Core Features

> **24 features — 23 approved by Clint 2026-06-25; F24 added 2026-06-26**

F01 Day Mode Engine — F02 Horizon Rings — F03 Stat Inference Engine — F04 Universal Goal Engine — F05 GYR Spiral — F06 Learning Engine — F07 Daily Reminder Engine — F08 Body Domain Health Tracking & Vault — F09 Big Ass Calendar — F10 Quarterly Habit Arc — F11 Container Model — F12 About Me + Vivid Vision — F13 Project + Tool Layer — F14 Kompass Operating Platform — F15 Shortcuts Tab — F16 Journal & Decisions Library — F17 Being Domain — F18 Balance Domain — F19 Business Domain — F20 Week at a Glance — F21 Key Docs — F22 In-App Spec Sheet — F23 App Shell & Navigation — F24 Meetings Capture / Second Brain *(added 2026-06-26)*

---

## §4 Gate 3 Checklist
- [x] All 23 features named and described — ✅ Approved by Clint 2026-06-25
- [x] All features have entities read/write — ✅ Approved by Clint 2026-06-25
- [x] All features have success criteria — ✅ Approved by Clint 2026-06-25
- [x] No feature references an entity not in §3 — ✅ Approved by Clint 2026-06-25
- [x] Full review complete — no gaps ✅
- [ ] F24 Meetings Capture — added post-approval 2026-06-26; pending Clint's gate sign-off (see Addendum)

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
| W08 | Capturing a Meeting *(added 2026-06-26)* | F24 | passive — auto-ingested from Plaud / Google Meet |
| W09 | Logging Weight *(added 2026-06-26)* | F08 | passive — Siri Shortcut → Apple Health → Claude `log-weight` skill → Stats (Phase-1 workaround) |

---

## §5 Gate 3 Checklist
- [x] All key workflows documented — ✅ Approved by Clint 2026-06-25
- [x] All workflows walkable end to end — ✅ Approved by Clint 2026-06-25
- [x] All workflows reference only features in §4 — ✅ Approved by Clint 2026-06-25

---

## §6 — Scope & Phasing

### Phase 1 — Clint Only

**In scope:** All 23 features, 44 entities, 7 workflows. Single user. SmartSuite data layer via Kompass MCP. Oura REST API (PAT). Strava MCP. GitHub API (read-only). Google Drive links (native, no API). Claude via Anthropic API. Railway. Next.js / React / TypeScript / Tailwind v4 — mobile-first.

> **Added 2026-06-26:** F24 Meetings Capture + entity #45. Plaud → SmartSuite Meetings bridge (in-app, Railway) plus an in-app Meetings view reached from the Shortcuts tab. Google Meet is the phase-2 provider behind the same ingestion seam.

**Health data architecture (corrected 2026-06-26):**
```
Sleep / Readiness / Activity / HRV:
  Oura ring → Oura REST API (PAT, Railway env var)
    → Railway backend (on app open / morning sweep) → Stitser Way + Claude

Weight (Phase-1 PERSONAL WORKAROUND — Oura API has no weight endpoint):
  Siri Shortcut → Apple Health (logs weight)
    → same shortcut sends the value to Claude with the `log-weight` skill
    → Stats table, against the "Weight & BMI" priority (silent, trusted source)
```
> ⚠️ **The weight path is a personal workaround.** It depends on Clint's own Siri Shortcut, his Apple Health, and his Claude. It is **not productizable** for a licensed / multi-user release — that requires a per-user health source (HealthKit companion or a per-user Oura/Health integration). Tracked in OQ16.

**Out of scope for Phase 1:** Family profiles (interactive), per-member auth, family Table Talk, Supabase migration, native mobile app, iOS HealthKit companion *(but a licensed multi-user product will need exactly this — or a per-user health integration — to replace the Phase-1 weight workaround; see OQ16)*, offline mode, push notifications, public/multi-tenant.

### Build sequence

**Sprint 1 — Shell + Today:** F23, F01, F07, F20, F11
**Sprint 2 — Goals + Rings:** F04, F02, F05 *(tag Goal domains before this sprint)*
**Sprint 3 — Domains + Oura:** F08, F17, F18, F19
**Sprint 4 — Intelligence layer:** F03, F06, F14
**Sprint 5 — Planning layer:** F09, F10, F12
**Sprint 6 — Tools + Library:** F13, F15, F16, F21, F22
**Out of sequence — Meetings:** F24 built 2026-06-26 (read view + Plaud bridge scaffold) ahead of the numbered sprints, at Clint's direction.

---

## §7 — Success Metrics

**Daily:** Mode < 30s. Thought + lesson visible. Week shape at a glance. Sacral Anchor < 2min.
**Weekly:** Buffer clears Rings + email + day types. Scoreboard correct.
**Monthly:** All Goals have GYR grades. Measurables tracking without forms. Lesson tapped more days than not.
**Quarterly:** One active Habit. Misogi as Project. Kevin's Rule in BAC.
**Annually:** Vivid Vision reviewed. Identity statements for past Habits.
**Meetings:** Every Plaud / Google Meet conversation lands in the Meetings table with no manual entry; searchable in-app within minutes of the recording finishing.
**Qualitative:** App feels like a partner, not a tool.
**Anti-metrics:** Forms > 2x/week. Rings > 10 items. No Spiral in 30+ days. Same thought twice in 7 days. Separate chat for rituals.

---

## §8 — Timeline

| Milestone | What it means |
|---|---|
| M1 — Shell live | F23 + F01 + F07 + F20 + F11. First daily use. |
| M2 — Goals + Rings live | F04 + F02 + F05. Core data loop. |
| M3 — All four domains live | F08 + F17 + F18 + F19. Oura live. |
| M4 — Intelligence layer live | F03 + F06 + F14. Machine working autonomously. |
| M5 — Full Phase 1 live | All 23 features. Primary daily OS. Phase 2 begins. |

Phase 2 trigger: one full quarter as daily driver + qualitative signal passed.

---

## §9 — Open Questions

| # | Question | Blocks | Status |
|---|---|---|---|
| OQ01 | **Domain field confirmed — `s5deb9616e`, multi-select, Body/Being/Balance/Business. Existing Goal records unpopulated — must be tagged before Sprint 2.** | Domain filtering F04, F17–F19 | ✅ Field confirmed. ⚠️ Data population pending before Sprint 2. |
| OQ02 | Family profile auth for Phase 1 | F12 | ⏳ Tech Spec |
| OQ03 | Strava sync frequency | F03, F08 | ⏳ Tech Spec |
| OQ04 | Claude API integration — server action vs. edge function? | All Claude features | ⏳ Tech Spec |
| OQ05 | Phase 1 write-back scope — which SmartSuite tables? | All write operations | ⏳ Tech Spec |
| OQ06 | Gwen Gifford — About Me or Extended Family? | F12 | ⏳ Clint |
| OQ07 | Learning Engine visual metaphor | F06 | ⏳ Clint |
| OQ08 | Domain rename for public brand | All labels | ⏳ Clint — evolving |
| OQ09 | Day Mode Log automation timing | F01 | ⏳ Tech Spec |
| OQ10 | **Oura PAT confirmed. Single REST integration — sleep, readiness, activity, HR, SpO2. (Weight NOT available — see OQ16.) Pull on app open + morning sweep.** PAT in hand 2026-06-26 — ready to wire. | F08 | ✅ Resolved 2026-06-25 |
| OQ11 | ~~No iOS companion needed; Oura syncs weight from Apple Health automatically.~~ ⚠️ **Superseded by OQ16 (2026-06-26)** — Oura API has no weight endpoint, so this assumption was wrong. | F08 | ⚠️ Superseded by OQ16 |
| OQ12 | Key Doc storage — JSON vs. Supabase? | F21 | ⏳ Tech Spec |
| OQ13 | Spec Sheet storage — local, GitHub, or Supabase? | F22 | ⏳ Tech Spec |
| OQ14 | Project Tool archive — Gist, Supabase blob, or SmartSuite attachment? | F13 | ⏳ Tech Spec |
| OQ15 | **Plaud ingestion auth (added 2026-06-26).** Official OAuth dev API applied for. Fallback confirmed viable: the `pld_tokenstr` web-session token works as `Authorization: Bearer` against `api.plaud.ai` (web API: `GET /file/simple/web`, `GET /file/detail/{id}`), ~300-day lifetime. Also pending: SmartSuite Meetings `Source` single-select option codes. | F24 | ⏳ Wire token (in progress 2026-06-26) |
| OQ16 | **Oura weight source — workaround in place (added 2026-06-26).** Oura REST v2 has no weight endpoint. **Phase-1 resolution:** Siri Shortcut logs to Apple Health and sends the value to Claude; the `log-weight` skill writes it to Stats (against the "Weight & BMI" priority `68c893f4065d17a960dd8f6f`). ⚠️ **Workaround only — NOT productizable:** depends on Clint's personal Siri Shortcut + Apple Health + Claude. A licensed/multi-user release needs a per-user health source (HealthKit companion or per-user Oura/Health integration). | F08 | ✅ Phase-1 workaround live / ⏳ open for licensing + Phase 2 |

---

## §9 Gate 4 Checklist
- [x] Scope defined ✅
- [x] Build sequence defined ✅
- [x] Success metrics defined ✅
- [x] Anti-metrics defined ✅
- [x] Timeline milestones defined ✅
- [x] Open questions captured ✅
- [x] OQ01 field confirmed — data population flagged ✅
- [x] OQ10 resolved; OQ11 superseded by OQ16 ✅

---

## ✅ PDD COMPLETE

**All four gates passed. Approved by Clint 2026-06-25.** *(Meetings capture + weight-capture workaround added 2026-06-26 — see Addenda.)*

| Document | Purpose |
|---|---|
| **Data Integration Doc** | Field-level mapping — §3 + resolved OQs |
| **Technical Spec** | Architecture, APIs, auth, deployment — §6 + §9 |
| **UI/UX Doc** | Wireframes, components, mobile-first — §4 + F23 |

**Hand off to Claude Code with this PDD as source of truth.**

---

## Addendum — Meetings Capture / Second Brain (added 2026-06-26)

Added during the build at Clint's direction, after the 2026-06-25 gate approvals. Captured here so the PDD stays the single source of truth.

### Entity #45 — Meeting
- **Source of truth:** SmartSuite Meetings table `6a0cff32f77ad06285909dcf`.
- **Purpose:** Every meeting and conversation is captured — transcript, summary, decisions, action items, attendees — so the full record is searchable. The "second brain."
- **Key fields:** Meeting Title, Meeting Date / Start / End, Duration, Meeting Type, **Source** (single-select), Status, Attendees, Summary/Overview, Discussion Notes, Decisions Made, Action Items (+ Source Text), **Plaud Recording Link / Visual Link / Recording ID**, **Meet Recording / Transcript Link**, Transcript Available, Tags.

### F24 — Meetings Capture / Second Brain
- **In-app (read):** a `/meetings` list + detail view (summary, decisions, action items, attendees, transcript), reached from the Shortcuts tab. Reads via the same `DataSource` seam as every other feature (fixtures now, SmartSuite live later).
- **Ingestion bridge (write):** a Railway-hosted job in the app that pulls new recordings from a provider, maps them to the Meetings fields, and upserts idempotently on **Plaud Recording ID**. Triggered via `POST /api/meetings/sync` (secret-guarded).
- **Provider 1 — Plaud:** reverse-engineered web API (`api.plaud.ai`), `Authorization: Bearer <pld_tokenstr>`. Official OAuth dev API applied for as the durable path (see OQ15).
- **Provider 2 — Google Meet (phase 2):** Calendar + Drive transcript → the same `Source` = Google Meet path, populating the Meet* fields. Behind the same normalized-recording seam.

### Status (2026-06-26)
- Read view + bridge scaffold built and verified; bridge runs in scaffold mode (no-op) until Plaud + SmartSuite credentials are set.
- Open: OQ15 (Plaud auth / Source option codes), OQ16 (Oura weight source).

---

## Addendum 2 — Weight Capture (added 2026-06-26)

**Why:** Oura's REST API exposes no weight endpoint, so the original "Oura aggregates weight" assumption (OQ10/OQ11) does not hold.

**Phase-1 workaround:** Clint's Siri Shortcut logs weight to Apple Health, then sends the value to Claude. The `log-weight` skill (`skills/log-weight/SKILL.md` in Clint-s-Kompass) parses the message and writes a Stats record (app `6840927ebcfa2d2bfef039e2`) against the "Weight & BMI" priority (`68c893f4065d17a960dd8f6f`, under the Body goal "CRS- Personal Body by 12/31/25") — silent, trusted source, no inference prompt. A "Log Weight" launcher in the app's Shortcuts tab fires the iOS shortcut.

**⚠️ Not productizable.** This depends on one person's Siri Shortcut + Apple Health + Claude. For a licensed / multi-user product, weight must come from a per-user source — a HealthKit companion app, or a per-user health integration — not this manual bridge. Do **not** ship the Siri-Shortcut path to other users. Tracked in OQ16 for Phase 2 / licensing.

---

## Discovery Inputs (from session 2026-06-24 / 2026-06-25)

*A–O captured in full in prior commits. Full detail in TSW_memory.md.*
