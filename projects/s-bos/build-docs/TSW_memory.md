# Memory: Stitser Way

> **Purpose:** Running log of key facts, decisions, and context. Read at the start of every session.
> **Update:** Whenever a decision is made or context shifts. Rewritten at session end.

---

## Project Identity

- **Project Name:** Stitser Way (working name — may stand the test of time)
- **App Description:** Personal operating system web app for Clint and family. Surfaces goals, habits, rituals, daily planning, and life domain tracking (Body / Being / Balance / Business) through a mobile-first UI with Claude integrated into the experience.
- **Current Phase:** Design — PDD Discovery Inputs Sections A–M complete. Gate 1 pending sign-off.
- **Brand identity:** Developing. Sage-Architect-Builder archetype. Austrian Sacred Heart Fire tradition (Herz-Jesu-Feuer) as the brand story. Kronerer identity most resonant. No final public name yet.

---

## Relationship to S-BOS

- Separate application, same system. Different PDD, different users, different purpose.
- Shared: Railway/Supabase stack, SmartSuite MCP access, Kompass Claude skills.
- Build docs use `TSW_` prefix in the same `projects/s-bos/build-docs/` folder.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js / React / TypeScript / Tailwind v4 — mobile-first |
| Backend | Next.js Server Actions — SmartSuite key server-side |
| Data (Phase 1) | SmartSuite Game App via Kompass MCP |
| Data (Phase 2) | Supabase (Postgres) — migrates from SmartSuite |
| LLM | Claude via Anthropic API — Spiral, coaching, day-mode |
| External | Strava MCP, Google Calendar MCP, Gmail MCP |
| Hosting | Railway — separate deployment from S-BOS |

---

## SmartSuite Game App Schema (Phase 1)

Solution ID: `68216f48f98789b5bb095a36`

| Table | App ID | Key fields |
|---|---|---|
| Goals | `6824d4d1885a8769bd2dfc0d` | title, GYR `se7c39059c`, target `s80be76f59`, % metric `sc9f2f3411`, % time `sbc4aa3064`, balance `s73396d72f`, days left `s3b0264e51`, due `s65ce469a2`, grade `s9c754688f` |
| Current Priorities | `68216f48f98789b5bb095a4b` | GYR, target, % metric, % time, days remaining, due, linked goal, milestones, actuals |
| Stats | `6840927ebcfa2d2bfef039e2` | goal link, priority link, begin/end date, amount `s6471266f2`, monthly/weekly |
| Stats Menu | `68409420391d32d925740e28` | measurable name, summary type, linked goals/priorities |
| Tasks | `683e80437ee1bca637ba6fde` | (schema not yet pulled) |
| GYR Status Reports | `68216f48f98789b5bb095a51` | (schema not yet pulled) |
| Milestones | `68216f48f98789b5bb095a37` | (schema not yet pulled) |

---

## Confirmed Design Decisions

1. **Four life domains:** Body / Being / Balance / Business
2. **Day-mode model:** Focus / Buffer / Free — Kompass suggests, Clint gut-checks (uh-huh / uh-uh)
3. **Four-tab nav:** Today / Horizon / Me / Shortcuts
4. **Me tab = domain menu** → Goals → universal initiative screen
5. **Universal Goal engine:** Current Score → Goal + Deadline → Rhythm & Reminders → Progress Tracking → Celebrate Wins
6. **Horizon Rings — dual mode:** Stacked list (default) + Circles overview (toggle). Five rings: Overdue / This Week / Active / Coming Soon / Parked
7. **Sacral decision model:** Kompass suggests with reasoning, Clint gut-checks, max two questions
8. **Table Talk ritual:** Hi / Lo / Buffalo (and Rose / Thorn / Bud). Brynn-originated.
9. **Kids own their data:** Each family member has their own profile and enters their own data
10. **Body section protocol:** Three phases. Strava integration. Meal tracker, drinking log, weight/body fat, health vault
11. **SmartSuite is Phase 1 live database.** Supabase migration is Phase 2
12. **Container Model:** Three-layer empty state (glow + invitation + progress ring). Claude-guided build. Refresh cycles.
13. **Learning Engine:** Ebbinghaus / Calmio model. One concept per day. 6-hour safety rail. Fires on habits, Body protocol, new containers, Spiral, family profiles, S-BOS skills.
14. **About Me section:** GitHub-sourced profile files. Full markdown render. Person selector. Search across all profiles.
15. **Key Docs:** Google Drive link registry. Links open natively. No API auth. Nine categories.
16. **Big Ass Calendar:** Year-at-a-glance. Misogi + Kevin’s Rule + Quarterly Habit. Two layers: backward (look how far) + forward (what’s coming).
17. **Quarterly Habit:** One at a time. Consecutive Appetite. Five-stage arc. Freud’s sense of achievement at completion.
18. **Vivid Vision & Annual Commitments:** Sub-section of About Me. Google Doc opens natively. Daily rotating reminder. Annual review prompt.
19. **Brand identity:** Stitser Way (working name). Austrian fire tradition (Herz-Jesu-Feuer) as brand story. Kronerer as most resonant identity archetype. No public name locked yet.
20. **Positioning statements locked:** The Machine quote + The Sage Builder quote (both in messaging doc)
21. **Messaging doc created:** `03-stitser-way/messaging.md` in Clint-s-Kompass repo
22. **S-BOS messaging doc created:** `projects/s-bos/build-docs/S-BOS_Messaging.md`

---

## Open Questions

- [ ] Goal Type field values in SmartSuite — how are domains tagged? (blocks Me tab domain filtering)
- [ ] Family profile auth model for Phase 1 (kids’ data isolation before Supabase)
- [ ] Strava sync frequency approach
- [ ] Claude API integration pattern (server action vs edge function)
- [ ] Phase 1 write-back scope — which tables, which fields
- [ ] Milestones, GYR Status Reports, Tasks schemas — not yet pulled
- [ ] Gwen Gifford — which section does she belong in? (About Me vs Extended Family)
- [ ] Visual metaphor for Learning Engine progress artifact — mountain trail? ridgeline? bike climb profile?
- [ ] Kronerer identity — find a phonetically smooth English-friendly variant (2 syllables, warm not abrasive)
- [ ] Domain rename — what replaces Body/Being/Balance/Business for the public brand?

---

## Discovery Session Artifacts (2026-06-24)

HTML prototypes: `kompass-personal-os.html`, `kompass-os-v2.html`, `horizon-rings-comparison.html`, `horizon-rings-full.html`, `kompass-os-v3.html`

Research reports produced (artifacts, not filed to GitHub):
- Aspirational identity branding — Warriors Way vs Sage-Architect-Builder
- Steward synonyms — Keeper, Cultivator, Tender
- Austrian alpine fire tradition — Herz-Jesu-Feuer, Bergfeuer, Kronerer
- colere/Latin root exploration — Cultivator, Cultor (superseded by fire tradition direction)

Discovery log filed to SmartSuite Journals/Rituals record `6a397fd470d165349ea0549e`
