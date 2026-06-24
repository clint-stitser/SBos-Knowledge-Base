# Restart: Stitser Way

> **Purpose:** Capture exactly where we stopped so the next session picks up without re-explaining.
> **Update:** Before ending every session. Overwrite entirely — current stopping point only.

---

## Session Info

- **Date:** 2026-06-24
- **Project:** Stitser Way
- **Status:** Design Phase — PDD Section 1 drafted, Gate 1 pending sign-off

---

## What We Were Doing

First design session for the Stitser Way App. Established the project as distinct from S-BOS, named it, set doc location, confirmed build target. Created all five continuity files. Started the PDD.

Prior to formal project setup, this session also produced significant discovery work: day-mode model (Focus/Free/Buffer), four-tab nav, universal Goal engine, Horizon Rings dual-mode design, Body protocol from research paper, SmartSuite Game App schema pull. All captured in `TSW_memory.md` and `TSW_Design_Context.md` as confirmed decisions and inputs to the PDD.

---

## Where We Stopped

PDD **Section 1 (Problem Statement) drafted** — awaiting Gate 1 sign-off:
- Problem Statement: Softr UI constraints + no LLM integration + no family profiles
- Target Users: Clint (primary), family members (secondary), kids own their data
- Core principle: personal OS across Body / Being / Balance / Business

---

## Next Steps (in order)

1. [ ] **Gate 1 sign-off** — Clint approves Problem Statement + Target Users section of the PDD
2. [ ] **Pull missing schemas** — Milestones, GYR Status Reports, Tasks (three tables not yet pulled from SmartSuite)
3. [ ] **Confirm Goal Type field values** — how are Body/Being/Balance/Business tagged in SmartSuite Goals table? (open question blocks Me tab domain filtering)
4. [ ] **Core Entities (PDD §3)** — define the data entities the app works with: Goal, Priority, Milestone, Stat, GYR Report, Task, Family Profile
5. [ ] **Core Features (PDD §4)** — formalize the confirmed design decisions into feature specs: Day Mode, Horizon Rings, Universal Goal Card, Today tab, Me tab, Shortcuts tab, Journal, Spiral, Table Talk, Body section
6. [ ] **User Workflows (PDD §5)** — walk through key flows: morning launch, buffer session, logging a stat, running the Spiral, setting a new goal
7. [ ] Then: Data Integration Doc (Phase 1 field map), Technical Spec, UI/UX Doc

---

## Open Questions / Decisions Pending

- Goal Type field values in SmartSuite (blocks domain filtering)
- Family profile auth model for Phase 1
- Strava sync frequency
- Claude API integration pattern (server action vs edge function)
- Phase 1 write-back scope

---

## Environment Notes

- GitHub connector live in claude.ai
- SmartSuite schema pulled via Kompass MCP (2026-06-24) — Game App solution confirmed
- Five continuity files created this session (first session for this project)
- S-BOS PDD still at Gate 1 — separate track, not blocked by Stitser Way work

---

## How to Resume

1. Read `TSW_Operating_Agreement.md`, `TSW_memory.md`, `TSW_restart.md`, `TSW_Design_Context.md`
2. Open `TSW_Product_Design_Doc.md` to Gate 1
3. Say "let's go" — first action is Gate 1 sign-off, then missing schema pulls
