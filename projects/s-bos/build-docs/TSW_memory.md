# Memory: Stitser Way

> **Purpose:** Running log of key facts, decisions, and context. Read at the start of every session.
> **Update:** Whenever a decision is made or context shifts. Rewritten at session end. Clint reviews.

---

## Project Identity

- **Project Name:** Stitser Way
- **App Description:** Personal operating system web app for Clint and family. Surfaces goals, habits, rituals, daily planning, and life domain tracking (Body / Being / Balance / Business) through a mobile-first UI with Claude integrated into the experience.
- **Goal (Phase 1):** Launch on top of existing SmartSuite Game App data — new UI, no Softr constraints, Claude integrated.
- **Goal (Phase 2):** Migrate data layer from SmartSuite to Supabase (same reasons as S-BOS).
- **Current Phase:** Design — PDD Section 1 drafted, awaiting Gate 1 sign-off.
- **Philosophy:** Same as S-BOS — calibrated honesty, human leads, agent executes precisely, nothing invented.

---

## Relationship to S-BOS

- Separate application, same system. Different PDD, different users, different purpose.
- Shared: Railway/Supabase stack, SmartSuite MCP access, Kompass Claude skills.
- Build docs use `TSW_` prefix in the same `projects/s-bos/build-docs/` folder.

---

## Tech Stack

| Layer | Technology | Notes |
|---|---|---|
| Frontend | Next.js / React / TypeScript / Tailwind v4 | Mobile-first |
| Backend | Next.js Server Actions | SmartSuite key server-side |
| Data (Phase 1) | SmartSuite Game App via Kompass MCP | Goals, Stats, GYR, Priorities |
| Data (Phase 2) | Supabase (Postgres) | Migrates from SmartSuite |
| LLM | Claude via Anthropic API | Spiral, coaching, day-mode suggestions |
| External | Strava MCP, Google Calendar MCP, Gmail MCP | Training, calendar, email |
| Hosting | Railway | Separate deployment from S-BOS |

---

## SmartSuite Game App Schema (Phase 1 data source)

Pulled 2026-06-24. Solution ID: `68216f48f98789b5bb095a36`

| Table | App ID | Key fields |
|---|---|---|
| Goals | `6824d4d1885a8769bd2dfc0d` | title, `se7c39059c` GYR, `s80be76f59` target, `sc9f2f3411` % metric complete, `sbc4aa3064` % time complete, `s73396d72f` balance to finish, `s3b0264e51` days left, `s65ce469a2` due date, `s7c4c1e1ad` start date, `s9c754688f` reporting grade, `s97u3fw0` linked priorities, `sm1w4mhd` actuals to date, `sfdc627a49` linked GYR reports, `s865ea7ebf` goal type |
| Current Priorities | `68216f48f98789b5bb095a4b` | title, `se7c39059c` GYR, `s5486ed538` target, `sef1d308b5` % metric complete, `sfab58c780` % time complete, `s22ea0498a` balance to finish, `sc8d42e777` days remaining, `s8a1658fc7` due date, `sb4b7e320c` start date, `s66efa99b0` related goal, `snmwmiij` linked milestones, `s2m6294x` actuals, `s0yurwcz` status reports |
| Milestones | `68216f48f98789b5bb095a37` | (schema not yet pulled) |
| GYR Status Reports | `68216f48f98789b5bb095a51` | (schema not yet pulled) |
| Stats | `6840927ebcfa2d2bfef039e2` | title, `sd6cc86075` associated goal, `s38ac950e1` associated priority, `s793df2063` begin date, `sb5657209d` end date, `s6471266f2` amount for period, `sfa08338c5` monthly or weekly, `sc05da1445` attachments |
| Stats Menu | `68409420391d32d925740e28` | title (measurable name), `see2194bd6` summary type, `s929ektu` linked goals, `sjcd04ul` linked priorities |
| Tasks | `683e80437ee1bca637ba6fde` | (schema not yet pulled) |

---

## Confirmed Design Decisions

1. **Four life domains:** Body / Being / Balance / Business
2. **Day-mode model:** Focus / Buffer / Free — suggested by Kompass (calendar + rings + recent pattern), confirmed by gut check (uh-huh / uh-uh), max two questions
3. **Four-tab nav:** Today / Horizon / Me / Shortcuts
4. **Me tab = domain menu:** Domain cards → Goals → universal initiative screen
5. **Universal Goal engine (five steps):** Current Score → Goal + Deadline → Rhythm & Reminders → Progress Tracking → Celebrate Wins
6. **Horizon Rings — dual mode:** Stacked list (default) + Circles overview (toggle top-right). Five rings: Overdue / This Week / Active / Coming Soon / Parked
7. **Sacral decision model:** Kompass suggests with reasoning, Clint gut-checks, max two questions
8. **Table Talk ritual:** Hi / Lo / Buffalo (and Rose / Thorn / Bud variant). Family member selector. Today tab + Journal history
9. **Kids own their data:** Each family member has their own profile and enters their own data
10. **Body section protocol:** Three phases sourced from evidence-based fat loss + MTB training research. Strava integration. Meal tracker, drinking log, weight/body fat, health vault
11. **SmartSuite is Phase 1 live database.** Supabase migration is Phase 2 (post S-BOS infrastructure build)
12. **Build docs live in:** `SBos-Knowledge-Base/projects/s-bos/build-docs/` with `TSW_` prefix
13. **Separate Railway deployment** from S-BOS — same stack, different app

---

## Open Questions

- [ ] Goal Type field values — how are domains (Body/Being/Balance/Business) tagged in SmartSuite?
- [ ] Family profile auth model for Phase 1 (kids' data isolation before Supabase)
- [ ] Strava sync frequency and data freshness approach
- [ ] Claude API integration pattern for Spiral and coaching (server action vs edge function)
- [ ] Phase 1 write-back scope — which tables, which fields
- [ ] Milestones, GYR Status Reports, Tasks schemas — not yet pulled

---

## Discovery Session Artifacts (2026-06-24)

The following were built as interactive prototypes during the discovery session. They are design exploration inputs — not finished design — and their decisions are captured above and in the PDD.

- `kompass-personal-os.html` — v1 prototype (5-tab shell)
- `kompass-os-v2.html` — v2 prototype (6-tab + living spec sheet)
- `horizon-rings-comparison.html` — side-by-side A/B visual comparison
- `horizon-rings-full.html` — dual-mode Horizon Rings (stacked + circles)
- `kompass-os-v3.html` — v3 prototype (day-mode: Focus/Free/Buffer launch screen)
- Discovery log filed to SmartSuite Journals/Rituals record `6a397fd470d165349ea0549e`

---

## Notes

- Decision-maker is **Clint**.
- A Softr version of the personal app already exists on top of SmartSuite Game App — Stitser Way is a new UI, not a new data model.
- The Universal Goal engine applies to everything: Body weight loss, alcohol reduction, training, business phases, relationship habits, reading goals. Same five steps, different stat types on top.
- The GYR Spiral is the transformation engine: Facts → Feelings → Root Cause → Focus → Massive/Relevant Actions → Fruit. Claude guides it; the result files to SmartSuite GYR Status Reports.
