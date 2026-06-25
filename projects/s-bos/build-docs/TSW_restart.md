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

2. **Completed §3 Core Entities** — 44 entities with real SmartSuite app IDs. Key decisions: Goals as universal tag-based entity, Tasks from three sources, Day Mode → Journal automation, Vivid Vision → GitHub source of truth, Bills & Invoices Phase 1, SB Training & Certifications as installation arc store, two lesson types, External Practice checkbox, Misogi + Kevin's Rule as project types. Oura confirmed as single health aggregator (entity #32 updated). Domain field confirmed: `s5deb9616e`. Gate 2 signed off.

3. **Wrote §4 Core Features** — 23 features with entities read/write, UX behavior, success criteria. Full cross-check — no gaps. Gate 3 §4 signed off.

4. **Wrote §5 User Workflows** — 7 workflows walkable end to end. Gate 3 §5 signed off.

5. **Wrote Gate 4** — §6 Scope + §7 Metrics + §8 Timeline + §9 Open Questions. Gate 4 signed off.

6. **Resolved OQ10 + OQ11** — Oura is the single health aggregator. Weight via Siri → Apple Health → Oura → Railway → Stitser Way. One PAT. No iOS companion.

7. **OQ01 partially resolved** — Domain field `s5deb9616e` confirmed. Existing Goals unpopulated. Script approach chosen: Claude Code infers domain from Goal titles → review table → Clint approves → writes back to SmartSuite. Runs before Sprint 2.

8. **Brand identity** — Austrian fire tradition as brand story. Kronerer most resonant. Running with Stitser Way as placeholder.

---

## Where We Stopped

**PDD is complete. Ready for Claude Code handoff.**

**One pre-Sprint 2 task:** Goal domain tagging script (see below).

---

## Next Steps (in order)

1. [ ] **Claude Code handoff** — use briefing below. Start Sprint 1.
2. [ ] **Goal domain tagging script** — run before Sprint 2. Claude Code builds and executes per the spec below.
3. [ ] **Data Integration Doc** — field-level mapping for all 44 entities. Build alongside Sprint 1–2.
4. [ ] **Technical Spec** — resolves OQ02–OQ09, OQ12–OQ14.
5. [ ] **UI/UX Doc** — screen-by-screen wireframes, mobile-first layout spec.
6. [ ] **Brand identity** — continue Kronerer/fire identity. Trademark + domain clearance when a name lands.

---

## Goal Domain Tagging Script (run before Sprint 2)

**Purpose:** Populate the Domain field (`s5deb9616e`) on all existing Goal records before Sprint 2 domain filtering goes live.

**Script spec for Claude Code:**

1. Pull all Goal records from SmartSuite Goals table `6824d4d1885a8769bd2dfc0d` via Kompass MCP
2. For each Goal, read: title + any available context fields (description, linked priorities, GYR reports)
3. Infer the most likely domain: Body / Being / Balance / Business
4. Output a review table in chat:

```
| Goal Title | Proposed Domain | Confidence | Notes |
|---|---|---|---|
| Morning Ritual Streak | Being | High | Ritual = Being |
| Rides with Max Q3 | Balance | High | Relationship measurable |
| Fat Loss Protocol | Body | High | Body composition |
| Allocator 4 Days/Week | Business | High | CEO seat metric |
| [unclear goal] | Being? | Low | Needs Clint review |
```

5. **Stop and wait for Clint to review.** Never write without confirmation.
6. Clint approves each row, adjusts any that are wrong, removes any he wants to skip
7. Script writes confirmed Domain values to SmartSuite via field `s5deb9616e`
8. Confirms count of records updated

**Rule:** Low-confidence rows are always flagged. Claude never guesses on ambiguous Goals — it surfaces them for Clint's decision.

---

## Open Questions (11 remaining)

- ~~**OQ01**~~ ✅ Field confirmed (`s5deb9616e`). ⚠️ Data population via script before Sprint 2.
- **OQ02** Family profile auth model for Phase 1
- **OQ03** Strava sync frequency — on-demand or background webhook?
- **OQ04** Claude API integration — server action vs. edge function?
- **OQ05** Phase 1 write-back scope — which SmartSuite tables?
- **OQ06** Gwen Gifford — About Me or Extended Family section?
- **OQ07** Learning Engine visual metaphor — mountain trail, ridgeline, or bike climb profile?
- **OQ08** Domain rename for public brand
- **OQ09** Day Mode Log automation timing — morning or end of day?
- ~~**OQ10**~~ ✅ Oura PAT confirmed. Single REST integration.
- ~~**OQ11**~~ ✅ No iOS companion. Oura syncs weight from Apple Health.
- **OQ12** Key Doc storage — JSON config vs. Supabase?
- **OQ13** Spec Sheet storage — local, GitHub, or Supabase?
- **OQ14** Project Tool archive — Gist, Supabase blob, or SmartSuite attachment?

---

## Claude Code Handoff Briefing

Use this exact text when starting a Claude Code session:

> "You are building Stitser Way — a personal operating system web app for Clint Stitser. The full Product Design Doc is at `SBos-Knowledge-Base/projects/s-bos/build-docs/TSW_Product_Design_Doc.md`. Read it in full before writing any code.
>
> **Tech stack:** Next.js / React / TypeScript / Tailwind v4. Mobile-first. Deployed on Railway.
>
> **Data layer:** SmartSuite (Phase 1) via Kompass MCP. GitHub API for profile files (read-only). Oura REST API (personal access token stored as Railway env var) for all health data — sleep, readiness, activity, and weight. Strava via MCP. Google Drive links open natively (no Drive API).
>
> **Health data flow:** Clint logs weight via Siri → Apple Health → Oura syncs automatically → Stitser Way pulls from Oura REST API. Single integration point. Weight stats logged silently — trusted source, no inference prompt.
>
> **Before Sprint 2:** Run the Goal domain tagging script in `TSW_restart.md` to populate the Domain field (`s5deb9616e`) on all Goal records. Do not start Sprint 2 until this is done.
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
