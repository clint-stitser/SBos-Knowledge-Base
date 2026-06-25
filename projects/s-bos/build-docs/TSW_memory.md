# Memory: Stitser Way

> **Purpose:** Running log of key facts, decisions, and context. Read at the start of every session.
> **Update:** Whenever a decision is made or context shifts. Rewritten at session end.

---

## Project Identity

- **Project Name:** Stitser Way (working name — may stand the test of time)
- **App Description:** Personal operating system web app for Clint only (Phase 1). Surfaces goals, habits, rituals, daily planning, health tracking, and life domain management (Body / Being / Balance / Business) through a mobile-first UI with Claude integrated into the experience.
- **Current Phase:** ✅ PDD COMPLETE — All four gates passed. Ready for Data Integration Doc + Technical Spec + UI/UX Doc + Claude Code handoff.
- **Brand identity:** Stitser Way (working name). Sage-Architect-Builder archetype. Austrian Sacred Heart Fire tradition (Herz-Jesu-Feuer) as brand story. Kronerer identity most resonant. No final public name yet.
- **Phase model:** Phase 1 = Clint only. Phase 2 = Family. Architecture accounts for family expansion.

---

## Relationship to S-BOS

- Separate application, same SmartSuite infrastructure. Different PDD, different users, different purpose.
- Shared: Railway/Supabase stack, SmartSuite MCP access, Kompass Claude skills.
- Build docs use `TSW_` prefix in `projects/s-bos/build-docs/` folder.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js / React / TypeScript / Tailwind v4 — mobile-first |
| Backend | Next.js Server Actions — SmartSuite key server-side |
| Data (Phase 1) | SmartSuite (multiple solutions) via Kompass MCP |
| Data (Phase 2) | Supabase (Postgres) — migrates from SmartSuite |
| LLM | Claude via Anthropic API — Spiral, coaching, day-mode, inference |
| External | Strava MCP, Oura API, Apple Health API, Google Calendar MCP, Gmail MCP |
| Hosting | Railway — separate deployment from S-BOS |

---

## SmartSuite Solutions Used (Phase 1)

| Solution | ID | Role |
|---|---|---|
| The SB Game App | `68216f48f98789b5bb095a36` | Goals, Priorities, Milestones, Stats, GYR Reports |
| The Stitser Way | `68f8f8fd3757414d70d94ade` | Journals, BAC tables, Decisions, Principles & Realizations |
| SB Biz Dev | `68216a706900e8eaf75a05a6` | Projects, Notes & Comments |
| SB Project MGT (WIP) | `68a8c3d1bba73ca6e62d00f0` | Check Lists, Tasks, Budget, Schedule, Checklists |
| SB Training & Certifications | `68d480e2727607560a7f0d22` | Lesson Catalog, Courses, Learning Tracks, Progress Records |

---

## Gate Status

| Gate | Status |
|---|---|
| Gate 1 — §1 Problem + §2 Users | ✅ Approved by Clint 2026-06-25 |
| Gate 2 — §3 Core Entities (44) | ✅ Approved by Clint 2026-06-25 |
| Gate 3 — §4 Features (23) + §5 Workflows (7) | ✅ Approved by Clint 2026-06-25 |
| Gate 4 — §6 Scope + §7 Metrics + §8 Timeline + §9 Open Questions | ✅ Approved by Clint 2026-06-25 |

---

## Core Decisions Confirmed

1. **Phase 1 = Clint only.** Family entirely Phase 2.
2. **Four life domains:** Body / Being / Balance / Business
3. **Day-mode model:** Focus / Buffer / Free — Kompass suggests, uh-huh/uh-uh confirmation, Day Mode Log auto-created in Journals via automation
4. **Four-tab nav:** Today / Horizon / Me / Shortcuts
5. **Universal Goal engine:** Five steps. `type` tag differentiates Standard / Quarterly Habit / Misogi / Kevin's Rule
6. **Horizon Rings:** Dual mode — Stacked + Circles. Five rings. Three SmartSuite sources.
7. **Sacral decision model:** Kompass suggests, max two questions
8. **Table Talk:** Phase 1 — Clint records. Phase 2 — per-member.
9. **Container Model:** Three-layer empty state (glow + invitation + progress ring). Claude-guided build.
10. **Learning Engine:** Two lesson types — In-App (6-hour safety rail) + External Practice (daily checkbox). Data store: SB Training & Certifications.
11. **About Me:** GitHub source. Phase 1: Clint's 4 files interactive, family read-only.
12. **Vivid Vision + Annual Commitments:** GitHub as source of truth. Google Doc is reading copy.
13. **Key Docs:** Drive link registry. Links open natively. No Drive API auth.
14. **Big Ass Calendar:** BAC tables in The Stitser Way SmartSuite solution — already live.
15. **Quarterly Habit:** One at a time. Five-stage arc. Freud's sense of achievement.
16. **Project pillars (universal):** Budget / Alignment / Schedule / Checklists — same in S-BOS and Stitser Way.
17. **Bills & Invoices:** Phase 1 confirmed — actively used for Europe trip budget.
18. **Relationship measurables:** Stat + Goal engine. Logged via Stat Inference Engine — Claude notices, asks once, Clint confirms.
19. **Stat Inference Engine:** Scans Strava + journals + calendar. One-tap prompts. Never automatic, never a form.
20. **Health data:** Oura (sleep + readiness primary), Apple Health (weight primary), Strava (training). Drive links for Body Scan / Bloodwork / Eye Prescription.
21. **Daily Reminder Engine:** Vivid Vision + Annual Commitments + Principles & Realizations. One per day, no 7-day repeat.
22. **SB Training & Certifications** `68d480e2727607560a7f0d22` is the installation arc data store for both Stitser Way and S-BOS.
23. **External Practice tracking:** Daily checkbox — did it happen, not what. Streak in Progress Record.
24. **Misogi + Kevin's Rule:** Both achieved via Project + Tool Layer (F13) + Universal Goal Engine (F04). Same four-pillar infrastructure.
25. **App Shell:** Four-tab nav. Today tab changes layout by day mode. Me menu: 4 domains + 7 sections. Top-right icons: Spiral + Spec Sheet.

---

## 44 Core Entities (Phase 1)

Full definitions in PDD §3.

**Game App:** Goals (#1), Priorities (#2), Milestones (#3), Stats (#4), Stat Menu Items (#5), GYR Status Reports (#6)
**Stitser Way:** Journal Entries (#8), Day Mode Log (#9), Decisions (#10), Principles & Realizations (#11), BAC Day Types (#12), BAC Calendar Events (#13), BAC Goals (#14)
**Game App Goals (tagged):** Quarterly Habit (#15), Misogi (#16), Kevin's Rule Event (#17)
**Tasks (composite):** Check List Tasks + Notes & Comments + GYR follow-ups (#7)
**SB Project MGT:** Projects (#18), Check Lists (#19), Check List Tasks (#20), Budget G702 (#21), Budget Line Items G703 (#22), Baseline Budget Items (#23), Bills & Invoices (#24), Project Schedule (#25), Project Dates (#26)
**SB Biz Dev:** Notes & Comments (#27)
**Claude artifact:** Project Tool (#28)
**External APIs:** Strava (#29), Oura Sleep (#30), Oura Readiness (#31), Apple Health Weight (#32)
**Drive links:** Body Scan (#33), Bloodwork (#34), Eye Prescription (#35)
**GitHub:** Vivid Vision (#36), Annual Commitments (#37), Clint's Profile (#38), Family Profiles (#39)
**App config:** Key Doc (#40)
**SB Training & Certifications:** Lesson (#41), Course (#42), Learning Track (#43), Progress Record (#44)

---

## 23 Core Features (Phase 1)

Full specs in PDD §4.

F01 Day Mode Engine — F02 Horizon Rings — F03 Stat Inference Engine — F04 Universal Goal Engine — F05 GYR Spiral — F06 Learning Engine — F07 Daily Reminder Engine — F08 Body Domain Health Tracking & Vault — F09 Big Ass Calendar — F10 Quarterly Habit Arc — F11 Container Model — F12 About Me + Vivid Vision — F13 Project + Tool Layer — F14 Kompass Operating Platform — F15 Shortcuts Tab — F16 Journal & Decisions Library — F17 Being Domain — F18 Balance Domain — F19 Business Domain — F20 Week at a Glance — F21 Key Docs — F22 In-App Spec Sheet — F23 App Shell & Navigation

---

## 7 User Workflows

Full step-by-step in PDD §5.

W01 Morning Launch — W02 Buffer Day Sweep — W03 Stat Logging (inference) — W04 GYR Spiral — W05 New Goal — W06 Project Tool — W07 Quarter Start Habit

---

## Build Sequence (Phase 1)

**Sprint 1:** F23, F01, F07, F20, F11 — Shell + Today
**Sprint 2:** F04, F02, F05 — Goals + Rings
**Sprint 3:** F08, F17, F18, F19 — Domains
**Sprint 4:** F03, F06, F14 — Intelligence layer
**Sprint 5:** F09, F10, F12 — Planning layer
**Sprint 6:** F13, F15, F16, F21, F22 — Tools + Library

---

## Open Questions (for Technical Spec)

- OQ01: Goal Type field values in SmartSuite (blocks domain filtering)
- OQ02: Family profile auth model for Phase 1
- OQ03: Strava sync frequency (on-demand vs. background webhook)
- OQ04: Claude API integration pattern (server action vs. edge function)
- OQ05: Phase 1 write-back scope — which tables
- OQ06: Gwen Gifford — About Me vs. Extended Family section
- OQ07: Learning Engine visual progress metaphor
- OQ08: Domain rename for public brand
- OQ09: Day Mode Log automation timing
- OQ10: Oura API auth — personal token vs. OAuth
- OQ11: Apple Health API — accessible from mobile web app? May force scope change.
- OQ12: Key Doc storage — JSON config vs. lightweight Supabase
- OQ13: Spec Sheet storage — local, GitHub, or Supabase
- OQ14: Project Tool archive — GitHub Gist, Supabase blob, or SmartSuite attachment

---

## Session Notes

- Session 1 (2026-06-24): First design session. 3 HTML prototypes. Discovery inputs A–M.
- Session 2 (2026-06-25): PDD complete. All four gates passed. 44 entities, 23 features, 7 workflows. Clint on 9-hour flight. PDD handed off to Claude Code.
