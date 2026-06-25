# Memory: Stitser Way

> **Purpose:** Running log of key facts, decisions, and context. Read at the start of every session.
> **Update:** Whenever a decision is made or context shifts. Rewritten at session end.

---

## Project Identity

- **Project Name:** Stitser Way (working name — may stand the test of time)
- **App Description:** Personal operating system web app for Clint only (Phase 1). Surfaces goals, habits, rituals, daily planning, health tracking, and life domain management (Body / Being / Balance / Business) through a mobile-first UI with Claude integrated into the experience.
- **Current Phase:** Design — Gate 1 ✅ Complete. Gate 2 ✅ Complete. Gate 3 (§4 Core Features + §5 User Workflows) in progress.
- **Brand identity:** Stitser Way (working name). Sage-Architect-Builder archetype. Austrian Sacred Heart Fire tradition (Herz-Jesu-Feuer) as brand story. Kronerer identity most resonant. No final public name yet.
- **Phase model:** Phase 1 = Clint only. Phase 2 = Family. Architecture accounts for family expansion but Phase 1 does not build it.

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

| Gate | Sections | Status |
|---|---|---|
| Gate 1 | §1 Problem + §2 Users | ✅ Approved by Clint 2026-06-25 |
| Gate 2 | §3 Core Entities | ✅ Approved by Clint 2026-06-25 (44 entities) |
| Gate 3 | §4 Features + §5 Workflows | 🔄 In progress |
| Gate 4 | §6–9 | ⏳ Not started |

---

## Core Decisions Confirmed

1. **Phase 1 = Clint only.** Family entirely Phase 2.
2. **Four life domains:** Body / Being / Balance / Business
3. **Day-mode model:** Focus / Buffer / Free — Kompass suggests, uh-huh/uh-uh confirmation, Day Mode Log auto-created in Journals via automation
4. **Four-tab nav:** Today / Horizon / Me / Shortcuts
5. **Universal Goal engine:** Five steps. `type` tag differentiates Standard / Quarterly Habit / Misogi / Kevin’s Rule — all in the same Game App Goals table
6. **Horizon Rings — dual mode:** Stacked + Circles. Five rings. Three SmartSuite sources (Tasks + Notes + GYR)
7. **Sacral decision model:** Kompass suggests, max two questions
8. **Table Talk:** Phase 1 — Clint records. Phase 2 — per-member entry.
9. **Container Model:** Three-layer empty state (glow + invitation + progress ring). Claude-guided build.
10. **Learning Engine:** Ebbinghaus/Calmio model. Two lesson types: In-App (6-hour safety rail) + External Practice (daily checkbox, no safety rail). Data store: SB Training & Certifications.
11. **About Me:** GitHub source. Phase 1: Clint’s 4 files interactive, family profiles read-only.
12. **Vivid Vision + Annual Commitments:** GitHub as source of truth (confirmed 2026-06-25). Google Doc is a reading copy.
13. **Key Docs:** Google Drive link registry. Links open natively. No Drive API auth.
14. **Big Ass Calendar:** BAC tables in The Stitser Way SmartSuite solution — already live.
15. **Quarterly Habit:** One at a time (Consecutive Appetite). Five-stage arc. Freud’s sense of achievement.
16. **Project pillars (universal):** Budget / Alignment / Schedule / Checklists — same four in S-BOS and Stitser Way.
17. **Bills & Invoices:** Phase 1 confirmed — actively used for Europe trip budget.
18. **Relationship measurables (Balance domain):** Use existing Stat + Goal engine. No new entities. Logged via Stat Inference Engine — Claude notices, asks once, Clint confirms.
19. **Stat Inference Engine:** Claude scans Strava + journals + calendar — surfaces one-tap log prompts for active Goals. Never automatic, never a form.
20. **Health data sources:** Oura (sleep + readiness — primary), Apple Health (weight — primary), Strava (training). Drive links for Body Scan / Bloodwork / Eye Prescription (Health Vault).
21. **Daily Reminder Engine:** Draws from Vivid Vision + Annual Commitments + Principles & Realizations. One per day, never repeats two days running.
22. **SB Training & Certifications** `68d480e2727607560a7f0d22` is the installation arc data store for both Stitser Way (personal skills/habits) and S-BOS (business operational skills).
23. **External Practice tracking:** Daily checkbox only — did it happen, not what happened. Streak tracked in Progress Record.
24. **Build docs in:** `SBos-Knowledge-Base/projects/s-bos/build-docs/` with `TSW_` prefix
25. **Messaging docs in:** `Clint-s-Kompass/03-stitser-way/messaging.md`

---

## 44 Core Entities (Phase 1)

Full definitions in PDD §3. Summary:

**Game App:** Goals (#1), Priorities (#2), Milestones (#3), Stats (#4), Stat Menu Items (#5), GYR Status Reports (#6)

**Stitser Way solution:** Journal Entries (#8), Day Mode Log (#9), Decisions (#10), Principles & Realizations (#11), BAC Day Types (#12), BAC Calendar Events (#13), BAC Goals (#14)

**Game App Goals (tagged):** Quarterly Habit (#15), Misogi (#16), Kevin’s Rule Event (#17)

**Tasks (composite — three sources):** Check List Tasks + Notes & Comments + GYR follow-ups (#7)

**SB Project MGT:** Projects (#18), Check Lists (#19), Check List Tasks (#20), Project Budget G702 (#21), Budget Line Items G703 (#22), Baseline Budget Items (#23), Bills & Invoices (#24), Project Schedule (#25), Project Dates (#26)

**SB Biz Dev:** Notes & Comments (#27)

**Claude artifact:** Project Tool (#28)

**External APIs:** Strava (#29), Oura Sleep (#30), Oura Readiness (#31), Apple Health Weight (#32)

**Drive links:** Body Scan (#33), Bloodwork (#34), Eye Prescription (#35)

**GitHub:** Vivid Vision (#36), Annual Commitments (#37), Clint’s Profile (#38), Family Profiles (#39)

**App config:** Key Doc (#40)

**SB Training & Certifications:** Lesson (#41), Course (#42), Learning Track (#43), Progress Record (#44)

---

## Open Questions

- [ ] Goal Type field values in SmartSuite — how are Body/Being/Balance/Business tagged? (blocks Me tab domain filtering)
- [ ] Family profile auth model for Phase 1 (kids’ data isolation before Supabase)
- [ ] Strava sync frequency
- [ ] Claude API integration pattern (server action vs edge function)
- [ ] Phase 1 write-back scope — which tables, which fields
- [ ] Gwen Gifford — About Me vs Extended Family section
- [ ] Visual progress metaphor for Learning Engine — mountain trail? fire-lit ridgeline? bike climb profile?
- [ ] Kronerer identity — find phonetically smooth English variant (2 syllables, warm not abrasive)
- [ ] Domain rename — what replaces Body/Being/Balance/Business for the public brand?
- [ ] UX entry point for project tool creation — from inside a pillar, or Claude skill trigger from anywhere?
- [ ] Day Mode Log — automation timing (morning after confirmation? end of day?)

---

## Session Notes

- Session 1 (2026-06-24): First design session. 3 HTML prototypes built. All discovery inputs A–M captured. Messaging docs created.
- Session 2 (2026-06-25): Gate 1 ✅, Gate 2 ✅. 44 entities defined. Stat Inference Engine (O). Health data sources. External Practice lesson type. Phase model confirmed. Clint on 9-hour flight — target handoff to Claude Code before landing.
