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

1. **Reviewed and updated §1 Problem Statement** — rewrote as four compounding problems (Fragmentation, Absence, Activation Gap, No Project Tool Layer). Machine quote as solution anchor. Gate 1 signed off.

2. **Completed §3 Core Entities** — 44 entities with real SmartSuite app IDs. Key decisions: Goals as universal tag-based entity, Tasks from three sources, Day Mode → Journal automation, Vivid Vision → GitHub source of truth, Bills & Invoices Phase 1, SB Training & Certifications as installation arc store, two lesson types, External Practice checkbox, Misogi + Kevin's Rule as project types. Oura confirmed as single health aggregator (entity #32 updated from Apple Health Weight to Oura Weight). Gate 2 signed off.

3. **Wrote §4 Core Features** — 23 features with entities read/write, UX behavior, success criteria. Full cross-check — no gaps. Gate 3 §4 signed off.

4. **Wrote §5 User Workflows** — 7 workflows walkable end to end. Gate 3 §5 signed off.

5. **Wrote Gate 4** — §6 Scope + §7 Metrics + §8 Timeline + §9 Open Questions (14 questions, 2 resolved). Gate 4 signed off.

6. **Resolved OQ10 + OQ11** — Oura is the single health aggregator. Clint logs weight via Siri → Apple Health → Oura syncs automatically → Stitser Way pulls all health data (sleep, readiness, activity, weight) from a single Oura REST API integration. One PAT. No iOS companion. No HealthKit bridge.

7. **Brand identity work** — Austrian Sacred Heart Fire tradition as brand story. Kronerer as most resonant identity archetype. Running with Stitser Way as placeholder.

8. **Messaging docs updated** — `Clint-s-Kompass/03-stitser-way/messaging.md`.

---

## Where We Stopped

**PDD is complete. Ready for Claude Code handoff.**

---

## Next Steps (in order)

1. [ ] **Claude Code handoff** — use briefing below. Claude Code reads the PDD and starts Sprint 1.
2. [ ] **Resolve OQ01 (Goal Type field values)** — pull SmartSuite Goals schema to confirm how Body/Being/Balance/Business are tagged. Blocks Sprint 2 domain filtering.
3. [ ] **Data Integration Doc** — field-level mapping for all 44 entities. Build alongside Sprint 1–2.
4. [ ] **Technical Spec** — architecture decisions, API patterns, auth model, deployment config. Resolves OQ02–OQ09, OQ12–OQ14.
5. [ ] **UI/UX Doc** — screen-by-screen wireframes, component library, mobile-first layout spec.
6. [ ] **Brand identity** — continue Kronerer/fire identity development. Trademark + domain clearance when a name lands.

---

## Open Questions (12 remaining — resolve before or during Technical Spec)

- **OQ01** ⚠️ Goal Type field values — blocks Sprint 2 domain filtering. Pull before Sprint 2 starts.
- **OQ02** Family profile auth model for Phase 1
- **OQ03** Strava sync frequency — on-demand or background webhook?
- **OQ04** Claude API integration — server action vs. edge function?
- **OQ05** Phase 1 write-back scope — which SmartSuite tables?
- **OQ06** Gwen Gifford — About Me or Extended Family section?
- **OQ07** Learning Engine visual metaphor — mountain trail, ridgeline, or bike climb profile?
- **OQ08** Domain rename for public brand
- **OQ09** Day Mode Log automation timing — morning or end of day?
- ~~**OQ10**~~ ✅ Resolved — Oura PAT confirmed. Single REST integration. See PDD §9.
- ~~**OQ11**~~ ✅ Resolved — No iOS companion needed. Oura syncs weight from Apple Health automatically. See PDD §9.
- **OQ12** Key Doc storage — JSON config vs. lightweight Supabase?
- **OQ13** Spec Sheet storage — local, GitHub, or Supabase?
- **OQ14** Project Tool archive — GitHub Gist, Supabase blob, or SmartSuite attachment?

---

## Claude Code Handoff Briefing

Use this exact text when starting a Claude Code session:

> "You are building Stitser Way — a personal operating system web app for Clint Stitser. The full Product Design Doc is at `SBos-Knowledge-Base/projects/s-bos/build-docs/TSW_Product_Design_Doc.md`. Read it in full before writing any code.
>
> **Tech stack:** Next.js / React / TypeScript / Tailwind v4. Mobile-first. Deployed on Railway.
>
> **Data layer:** SmartSuite (Phase 1) via Kompass MCP. GitHub API for profile files (read-only). Oura REST API (personal access token, stored as Railway env var) for all health data — sleep, readiness, activity, and weight. Strava via MCP. Google Drive links open natively (no Drive API).
>
> **Health data flow:** Clint logs weight via Siri → Apple Health → Oura syncs automatically → Stitser Way pulls from Oura REST API. Single integration point. Weight stats are logged silently (trusted source — no inference prompt).
>
> **Start with Sprint 1:** F23 App Shell + F01 Day Mode Engine + F07 Daily Reminder Engine + F20 Week at a Glance + F11 Container Model.
>
> **The one rule:** The app must feel like a machine, not a collection of tools. Every interaction is a conversation or a tap — never a form."

---

## Environment

- GitHub connector: live in claude.ai
- SmartSuite schema: pulled 2026-06-25 — all 5 solutions confirmed
- PDD: `SBos-Knowledge-Base/projects/s-bos/build-docs/TSW_Product_Design_Doc.md`
- Messaging: `Clint-s-Kompass/03-stitser-way/messaging.md`
- S-BOS PDD: still at Gate 1, separate track
