# Restart: Stitser Way

> **Purpose:** Capture exactly where we stopped so the next session picks up without re-explaining.
> **Update:** Before ending every session. Overwrite entirely.

---

## Session Info

- **Date:** 2026-06-25
- **Project:** Stitser Way
- **Status:** ✅ PDD COMPLETE. All four gates passed. Claude Code handoff ready.

---

## What We Did This Session

1. **Reviewed and updated §1 Problem Statement** — rewrote as four compounding problems (Fragmentation, Absence, Activation Gap, No Project Tool Layer). Added Machine quote as solution anchor. Gate 1 signed off.

2. **Completed §3 Core Entities** — 44 entities with real SmartSuite app IDs from live schema pull. Key decisions: Goals as universal tag-based entity, Tasks from three sources, Day Mode → Journal automation, Vivid Vision → GitHub source of truth, Bills & Invoices Phase 1, SB Training & Certifications as installation arc store, two lesson types, External Practice checkbox, Misogi + Kevin's Rule as project types. Gate 2 signed off.

3. **Wrote §4 Core Features** — 23 features, each with entities read/write, UX behavior, and success criteria. Full systematic cross-check against all Discovery Inputs, entities, and decisions — no gaps found. Gate 3 §4 signed off.

4. **Wrote §5 User Workflows** — 7 workflows walkable end to end. Gate 3 §5 signed off.

5. **Wrote Gate 4** — §6 Scope (Phase 1 in/out of scope, build sequence), §7 Success Metrics (daily/weekly/monthly/quarterly/annual + qualitative signal + anti-metrics), §8 Timeline (5 milestones + Phase 2 trigger), §9 Open Questions (14 questions with owners and blockers). Gate 4 signed off.

6. **Brand identity work** — Austrian Sacred Heart Fire tradition (Herz-Jesu-Feuer / Bergfeuer) confirmed as brand story. Kronerer as most resonant identity archetype. Names explored: Bergfeuer, Kronerer, Krone, Feura, Luma Way — none landed. Running with Stitser Way. Sage Builder quote and Steward's Way direction captured in messaging doc.

7. **Messaging docs updated** — `Clint-s-Kompass/03-stitser-way/messaging.md` — fire tradition analogies, brand story, Kronerer identity notes.

---

## Where We Stopped

**PDD is complete. Ready for Claude Code handoff.**

---

## Next Steps (in order)

1. [ ] **Claude Code handoff** — provide the PDD as the source of truth. Claude Code builds the app starting with Sprint 1 (Shell + Today).
2. [ ] **Data Integration Doc** — field-level mapping for all 44 entities. Resolves OQ01–OQ05 first.
3. [ ] **Technical Spec** — architecture decisions, API patterns, auth model, deployment config.
4. [ ] **UI/UX Doc** — screen-by-screen wireframes, component library, mobile-first layout spec.
5. [ ] **Resolve OQ11 (Apple Health API)** — confirm whether accessible from mobile web app before Sprint 3.
6. [ ] **Resolve OQ01 (Goal Type field values)** — pull SmartSuite schema before Sprint 2.
7. [ ] **Brand identity** — continue Kronerer/fire identity development. Trademark + domain clearance when a name lands.

---

## Open Questions (resolve before building)

- **OQ01** Goal Type field values (blocks Sprint 2 domain filtering)
- **OQ02** Family profile auth model
- **OQ03** Strava sync frequency
- **OQ04** Claude API integration pattern
- **OQ05** Phase 1 write-back scope
- **OQ06** Gwen Gifford section placement
- **OQ07** Learning Engine visual metaphor
- **OQ08** Domain rename
- **OQ09** Day Mode Log automation timing
- **OQ10** Oura API auth model
- **OQ11** Apple Health API — mobile web accessible? (may force scope change)
- **OQ12** Key Doc storage layer
- **OQ13** Spec Sheet storage layer
- **OQ14** Project Tool archive location

---

## Claude Code Handoff Briefing

When starting a Claude Code session, provide this context:

> "You are building Stitser Way — a personal operating system web app for Clint Stitser. The full Product Design Doc is at `SBos-Knowledge-Base/projects/s-bos/build-docs/TSW_Product_Design_Doc.md`. Read it before writing any code. The tech stack is Next.js / React / TypeScript / Tailwind v4, mobile-first, deployed on Railway. SmartSuite is the Phase 1 data layer via the Kompass MCP. Start with Sprint 1: F23 App Shell + F01 Day Mode Engine + F07 Daily Reminder Engine + F20 Week at a Glance + F11 Container Model. The app must feel like a machine, not a collection of tools. Every interaction is a conversation or a tap — never a form."

---

## Environment

- GitHub connector: live in claude.ai
- SmartSuite schema: pulled 2026-06-25 — all 5 solutions confirmed
- PDD: `SBos-Knowledge-Base/projects/s-bos/build-docs/TSW_Product_Design_Doc.md`
- Messaging: `Clint-s-Kompass/03-stitser-way/messaging.md`
- S-BOS PDD: still at Gate 1, separate track
