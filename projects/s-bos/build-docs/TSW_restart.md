# Restart: Stitser Way

> **Purpose:** Capture exactly where we stopped so the next session picks up without re-explaining.
> **Update:** Before ending every session. Overwrite entirely.

---

## Session Info

- **Date:** 2026-06-25
- **Project:** Stitser Way
- **Status:** Design Phase — Gate 1 ✅ Gate 2 ✅. Gate 3 in progress (§4 Core Features next).
- **Context:** Clint on a 9-hour flight. Variable wifi. Target: work through §4 + §5, reach Claude Code handoff before landing.

---

## What We Did This Session

1. **Established the full project** from scratch — 5 continuity files, 2 messaging docs, all discovery inputs A–N captured in the PDD.
2. **Gate 1 signed off** — Problem Statement rewritten with 4 compounding problems (Fragmentation, Absence, Activation Gap, No Project Tool Layer). Machine quote as the solution anchor. §2 Target Users phased (Phase 1 = Clint only).
3. **Gate 2 signed off** — 44 Core Entities defined with real SmartSuite app IDs from live schema pull. Key decisions:
   - Goals are universal tag-based (Quarterly Habit / Misogi / Kevin’s Rule = Goal records)
   - Tasks pull from three sources (Check List Tasks + Notes & Comments + GYR follow-ups)
   - Day Mode → Journal Entry via automation (dashboard print tag pattern)
   - Vivid Vision + Annual Commitments → GitHub as source of truth
   - Bills & Invoices confirmed Phase 1 (Europe trip budget live)
   - SB Training & Certifications confirmed as installation arc data store
   - Two lesson types: In-App (6-hour safety rail) + External Practice (daily checkbox, no safety rail)
4. **Discovery Input O added** — Stat Inference Engine: Claude scans Strava/journals/calendar and surfaces one-tap log prompts for Balance Goals. No new entities. No forms.
5. **Health data layer added** — Oura (sleep + readiness), Apple Health (weight), Strava (training). Drive links for Body Scan / Bloodwork / Eye Prescription (Health Vault).
6. **Phase model confirmed** — Phase 1 = Clint only. Family entirely Phase 2. Architecture supports expansion.
7. **Brand identity captured** — Austrian fire tradition (Herz-Jesu-Feuer / Bergfeuer). Kronerer as most resonant archetype. Running with Stitser Way as placeholder.

---

## Where We Stopped

**Gate 2 complete. §4 Core Features not yet started.**

All 44 entities defined. Entity map clean. Phase boundary clear. Ready to write §4.

---

## Next Steps (in order)

1. [ ] **§4 Core Features** — formalize every confirmed design decision into a feature spec with: name, what it does, entities it reads/writes, success criteria. Work through the 15 major features systematically.
2. [ ] **§5 User Workflows** — walk through 5–7 key daily flows end to end: morning launch, buffer session, logging a stat, running the Spiral, setting a new goal, building a project tool.
3. [ ] **Gate 3 sign-off** — Clint approves §4 + §5.
4. [ ] **Gate 4** — §6 Scope + §7 Metrics + §8 Timeline + §9 Open Questions.
5. [ ] **PDD Done** — Data Integration Doc + Technical Spec + UI/UX Doc can begin.
6. [ ] **Claude Code handoff** — target before end of flight.

---

## §4 Feature List (seed — to be formalized)

The following 15 features need specs written in §4:

| # | Feature | Source |
|---|---|---|
| 1 | Day Mode Engine | Discovery Input B |
| 2 | Horizon Rings | Discovery Input C |
| 3 | Stat Inference Engine | Discovery Input O |
| 4 | Universal Goal Engine | Discovery Input E |
| 5 | GYR Spiral | Discovery Input F (Spec Sheet §6) |
| 6 | Learning Engine (In-App + External Practice) | Discovery Input L |
| 7 | Daily Reminder Engine | §3 Key Clarifications |
| 8 | Body Domain — Health Tracking & Vault | Discovery Inputs (Body protocol) |
| 9 | Big Ass Calendar | Discovery Input I |
| 10 | Quarterly Habit Arc | Discovery Input J |
| 11 | Container Model — Empty State & Build Flow | Discovery Input L |
| 12 | About Me + Vivid Vision | Discovery Input H |
| 13 | Project + Tool Layer | Discovery Input N |
| 14 | Kompass Operating Platform (Capture, Buffer, Genius Schedule) | Discovery Input D |
| 15 | Shortcuts Tab | Discovery Input F |

---

## Open Questions (running list for §9)

- Goal Type field values in SmartSuite (blocks domain filtering in Me tab)
- Family profile auth model for Phase 1
- Strava sync frequency
- Claude API integration pattern (server action vs edge function)
- Phase 1 write-back scope — which tables, which fields
- Gwen Gifford — About Me vs Extended Family section
- Learning Engine visual progress metaphor — mountain trail? ridgeline? bike climb?
- Kronerer identity — smooth English variant
- Domain rename for public brand
- UX entry point for project tool creation
- Day Mode Log automation timing

---

## Environment Notes

- GitHub connector live in claude.ai
- SmartSuite schema pulled 2026-06-25 — all 5 solutions confirmed
- 44 entities in PDD §3 with real app IDs
- Messaging doc at `Clint-s-Kompass/03-stitser-way/messaging.md`
- S-BOS PDD still at Gate 1 — separate track

---

## How to Resume

1. Read `TSW_Operating_Agreement.md`, `TSW_memory.md`, `TSW_restart.md`, `TSW_Design_Context.md`
2. Open `TSW_Product_Design_Doc.md` to §4
3. Say “let’s go” — first action is writing §4 Core Features
4. Work through the 15 features above systematically
5. Then §5 User Workflows (5–7 key daily flows)
6. Then Gate 3 sign-off
7. Then Gate 4
8. Then Claude Code handoff
