# Product Design Doc: Stitser Way

> **Status:** 🔄 In Progress — Section 1 drafted, Gate 1 pending sign-off
> **Methodology:** Ryan Falke's Design Templates, adapted for Stitser Way
> **Decision-maker:** Clint Stitser
> **Last updated:** 2026-06-24

---

## Gate System

| Gate | Sections | Condition to advance |
|---|---|---|
| Gate 1 | §1 Problem + §2 Users | Clint approves both |
| Gate 2 | §3 Core Entities | All entities named, no orphans |
| Gate 3 | §4 Features + §5 Workflows | All features have success criteria; all workflows are walkable |
| Gate 4 | §6 Scope + §7 Metrics + §8 Timeline + §9 Open Questions | All open questions resolved or deferred with owner |
| ✅ PDD Done | All gates passed | Data Integration Doc + Tech Spec + UI/UX Doc can begin |

---

## §1 — Problem Statement

### The problem

Clint has a working personal goal-tracking infrastructure in SmartSuite (the Game App — Goals, Priorities, Stats, GYR, Milestones). A Softr portal surfaces it today. But the Softr UI has three fundamental limitations:

1. **No LLM integration.** Claude cannot be woven into the daily experience — no Spiral processing, no day-mode suggestions, no coaching, no context-aware reminders. Claude lives in a separate chat window and has no awareness of what's happening in the data.

2. **No family profiles.** Softr cannot support per-member profiles with age-appropriate autonomy. The personal app needs to serve Clint differently than it serves Brynn or Max — and each family member needs to own and enter their own data.

3. **Softr constraints.** UI is limited to Softr's component library. Mobile experience is compromised. Navigation, day-mode behavior, dual-view screens (e.g., Horizon Rings stacked/circles toggle), and the universal Goal card pattern cannot be built without fighting the platform.

### What a good solution looks like

- A mobile-first web app that surfaces the existing SmartSuite Game App data through a purpose-built UI
- Claude integrated natively — the Spiral runs inside the app, day-mode is suggested and confirmed in-app, coaching appears in context
- Family profiles with per-member data ownership
- Day-mode aware: the app changes posture based on Focus / Buffer / Free day type
- Every life domain (Body, Being, Balance, Business) has goals, phases, progress tracking, rituals, and Claude-assisted processing — all in one place
- Built on the same Railway/Supabase stack as S-BOS so it can migrate off SmartSuite when that infrastructure is ready

### What this is NOT

- Not a replacement for S-BOS (the business operating system — that's a separate app)
- Not a new data model — the SmartSuite Game App infrastructure is preserved and extended
- Not a native mobile app (Phase 1 is a mobile-optimized web app)
- Not a multi-tenant product (Phase 1 is personal, for the Stitser family only)

---

## §1 Gate 1 Checklist

- [ ] Problem clearly stated — Clint confirms this is the real problem ✳️ *Pending sign-off*
- [ ] Solution criteria stated — Clint confirms these are the right criteria ✳️ *Pending sign-off*
- [ ] Scope boundaries stated — Clint confirms what this is NOT ✳️ *Pending sign-off*

---

## §2 — Target Users

### Primary user: Clint Stitser

- Personal operating system, daily driver
- Uses the app across all four life domains: Body, Being, Balance, Business (personal)
- Day-mode aware: Focus Day (deep work), Buffer Day (Kompass-assisted clearing), Free Day (incubation)
- Interacts with Claude for Spiral processing, day-mode confirmation, reminders, and coaching
- Runs morning ritual, evening ritual, weekly review, and stacks (WAR, Cash, Irritation, etc.) through the app
- Operating manual profile: interest-based nervous system, object permanence challenges, consecutive appetite (one thing at a time), Channel of Inspiration (1-8), non-linear thinking is a structural advantage

### Secondary users: Family members

| Member | Role | Notes |
|---|---|---|
| Christie | Partner | Full profile, own data |
| Avery | Child | Own profile, age-appropriate autonomy, owns their data |
| Brynn | Child | Initiated the Table Talk ritual. Own profile, owns their data |
| Max | Child | Own profile, owns their data |

**Design principle:** Kids enter their own data. The app is intrinsically motivating — not parent-assigned homework. Customized to each family member's wiring and learning style.

### User needs by domain

| Domain | What Clint needs the app to do |
|---|---|
| Body | Track weight, body fat, meals, alcohol, training rides (Strava), health records. Phase-based protocol (Foundation → Engine → Race Block). Reminders and staged learning connected to the why. |
| Being | Morning/evening rituals, stacks, affirmations, decisions, mindset tracking. Spiral processing. Phase gate for being/mindset development. |
| Balance | Family coordination, Table Talk (Hi/Lo/Buffalo), relationship tracking, gratitude, celebrations. Christie + kids profiles. |
| Business | Personal business goals — separate from S-BOS team goals. Phase anchor for Clint's role as allocator/CEO. GYR status per product line from his personal vantage point. |

---

## §2 Gate 1 Checklist (continued)

- [ ] All user types identified ✳️ *Pending sign-off*
- [ ] Primary user needs per domain stated ✳️ *Pending sign-off*
- [ ] Family profile model confirmed ✳️ *Pending sign-off*

---

## §3 — Core Entities

> ⏳ Not started. Begins after Gate 1 sign-off.

---

## §4 — Core Features

> ⏳ Not started. Begins after Gate 2.

---

## §5 — User Workflows

> ⏳ Not started. Begins after Gate 2.

---

## §6 — Scope & Phasing

> ⏳ Not started. Begins after Gate 3.

---

## §7 — Success Metrics

> ⏳ Not started. Begins after Gate 3.

---

## §8 — Timeline

> ⏳ Not started. Begins after Gate 3.

---

## §9 — Open Questions

> ⏳ Not started. Running list maintained in `TSW_memory.md` until this section is opened.

---

## Discovery Inputs (from session 2026-06-24)

> These are confirmed design decisions from the discovery session. They will be formalized into §3–§5 once Gate 1 is signed off. Organized into thirteen areas.

---

### A — Navigation & Shell

- Four persistent bottom tabs: **Today / Horizon / Me / Shortcuts**
- Me tab is a domain menu — four domain cards (Body/Being/Balance/Business), tap domain → Goals list, tap Goal → universal initiative screen
- **Me menu sections (full list):** Body / Being / Balance / Business domains + About Me & People Around Me + Key Docs + Big Ass Calendar + Quarterly Habit + Journal + Tools + Spec Sheet
- Spiral and Spec Sheet accessible from top-right nav icons on any screen (not main tabs)
- Tab bar hides entirely in Focus Day mode — the app changes posture, not just content

---

### B — Day Mode
*Source: `06-platform-design/kompass-operating-platform.md` in Clint-s-Kompass repo*

Three modes: **Focus Day / Buffer Day / Free Day**

**Suggestion logic — Kompass reads three signals:**
1. Calendar — meetings, blocks, commitments today
2. Horizon Rings — count of Overdue + This Week items
3. Recent energy — days since last Buffer Day, last Free Day

**Confirmation flow:**
- Kompass makes one suggestion with reasoning
- Clint responds: **uh-huh** (confirms) or **uh-uh** (triggers three-option picker: Focus / Buffer / Free)
- Max two questions, never open-ended
- Mode badge displayed in nav — tap to switch

**Focus Day:** Tab bar hides. Only anchor item + Pomodoro timer visible. Kompass runs silently. Grounding affirmation shown. Break-glass available.

**Buffer Day:** Full app visible. Horizon Rings is the command center. Three sub-tabs: Horizon / Email / Big 3. Two guardrail questions on any commitment decision.

**Free Day:** Calendar + wins + Table Talk prompt only. No tasks, no inbox. Mandate: “Your job today is to wander.” Kompass off.

**Week architecture:** Focus Day (meetings only if serving named initiative) / Buffer Day Tue (team meeting anchor) / Buffer Day Wed (dev meeting anchor) / Free Day (zero business)

---

### C — Horizon Rings
*Sources: `06-platform-design/horizon-rings-design-spec.md` + `skills/daily-horizon-scan/SKILL.md`*

Five rings: 🔴 Overdue / 🟡 This Week / 🔵 Active / 🟢 Coming Soon / ⚪ Parked. Three data sources: SmartSuite Tasks, Notes & Comments, GYR Status Reports. Max 7–10 items. Dual-mode toggle: Stacked list (default triage) + Circles overview (spatial orientation, tap → Focus bridge). Sacral Anchor prompt. Quick Clear mode. Phase Context Strip.

---

### D — Kompass Operating Platform
*Source: `06-platform-design/kompass-operating-platform.md`*

Three layers: Second Brain (capture & externalize) / Buffer Anchor (email triage, body-doubling) / Genius Schedule (daily calendar design and review). Nine-type capture routing table. `Who` field privacy rule. Weekly Monday audit.

---

### E — Universal Goal Engine

Five steps: Current Score → Goal + Deadline → Rhythm & Reminders → Progress Tracking → Celebrate Wins. Same template for every initiative across every domain. Domain-specific fields layer on top.

---

### F — Shortcuts Tab

19 personal Claude skills + 17 business Claude skills + 8-item external tools grid. Full inventory in `TSW_Design_Context.md` and `TSW_memory.md`.

---

### G — In-App Spec Sheet

Living design doc built into the app. 10 sections, 60+ feature rows. Feature / Principle / Tool / Status columns. Status cycles tap-to-advance. Cells tap-to-edit. Add row per section. Summary pill strip at top.

---

### H — About Me & People Around Me

Full profiles for Clint (4 files) + Christie, Avery, Brynn, Maxwell, Gwen. Data source: GitHub `Clint-s-Kompass` repo via GitHub API — the only section not reading from SmartSuite. Person selector, full markdown render, search across all profiles, last-updated date, edit opens GitHub natively.

**H1 — Vivid Vision & Annual Commitments:** 10-year vision + 1-year commitments surfaced inside Clint’s profile. Google Doc opens natively (Drive link, no API auth). Daily rotating reminder tied to morning ritual. Annual update prompt each New Year.

---

### I — Big Ass Calendar
*Source: thebigasscalendar.com*

Year-at-a-glance visual. Three elements: Misogi (year-defining event) + Kevin’s Rule (bimonthly adventures) + Quarterly Habit. Two views: full-year color-coded (backward “look how far” layer + forward “what’s coming” layer) + month/week drill-in. Surfaces on Free Day Today tab.

---

### J — Quarterly Habit

One habit at a time — Consecutive Appetite model. Five-stage arc: Install → Beginner → Intermediate → Expert → Complete. Freud’s sense of achievement at completion. Staged learning (3–5 lessons, one per day, first 2 weeks). Celebration at 7/14/21/30/60/90 days + quarter end. Identity statement generated at completion.

---

### K — Key Docs

Google Drive link registry for critical personal and family documents. Links open Drive natively — no API auth. Nine categories. Person selector + Family tab. Emergency-access flag. Cross-references Body health vault.

---

### L — Container Model & Learning Engine

**L1 — Container Model:** Three-layer empty state (soft glow + clear invitation + progress ring). Every container has a “why this matters” message before the build prompt. Build flow: Claude-guided conversation → filed to right destination → container fills → progress ring updates → celebration. Refresh cycles per container type.

**L2 — Learning Engine:** Ebbinghaus forgetting curve + spaced repetition + sleep as the training session. One concept per card. Named bounded lessons with time estimates. Single Next button. 6-hour safety rail. Fires on: Quarterly Habit install, Body protocol, new container build, first Spiral, first family profile, S-BOS skill onboarding (cross-product).

---

### M — Brand Identity & Positioning
*Source: Identity research session 2026-06-24. Full research reports in `TSW_messaging.md` (Clint-s-Kompass) and artifact docs.*

**Working name:** Stitser Way — a good placeholder that may stand the test of time.

**Identity archetype confirmed:** Sage-Architect-Builder — someone who sees clearly, designs intentionally, builds and tends daily. Embodied through pursuit, never finished. Starting posture is gratitude, not combat.

**Closest identity noun:** The Steward — one who tends what’s been entrusted from a place of gratitude. First instinct after research (Clint): *“The Steward’s Way.”* Steward synonyms explored: Keeper (top candidate), Cultivator, Tender. Sovereign/Craftsman/Author — all over-claimed, avoid.

**Austrian heritage thread (developing — not yet named):**
Clint’s Austrian heritage is the authentic source of the brand identity — the same instinct that produced “Kompass” as a product name. The Austrian Sacred Heart Fire tradition (Herz-Jesu-Feuer / Bergfeuer / Sonnwendfeuer), witnessed firsthand at the 2026 summer solstice in Tyrol, is the most resonant brand story found:

- **Tradition origin:** 1796, Tyrolean communities vow to tend their land and renew the vow annually by lighting fires on every peak simultaneously
- **The Kronerer:** In Oberammergau, 15 elite men hold the ancient right to light the crown fire on the summit — a title earned through generational training, selection, and grit. This is the most resonant identity archetype found.
- **The analogies (full detail in messaging doc):**
  - The climb = daily practice and habit installation
  - Generational knowledge = the Learning Engine
  - Every peak lit simultaneously = all four life domains tended at once
  - The fire as beacon = leadership and legacy
  - The vow renewed = annual ritual and recommitment
  - There is always another peak = pursuit, not destination
  - The Kronerer = the earned identity
- **Names explored:** Bergfeuer, Bergfeurer, Kronerer, Krone, Krone Way, Feura, Luma Way, Feuerhüter, Lichtweg. None landed as *the one* in this session.
- **Session conclusion (Clint):** “I like the identity of Bergfeurer but not the abrasive names. German can be a tough and abrupt language.” Played with Kronerer phonetic variants. Running with Stitser Way as placeholder while the fire identity develops organically.

**Confirmed positioning statements:**
- *“The app is a machine that procedurally produces a better life. You don’t need to arrive fully formed. The machine installs you into clarity over time.”*
- *“No need to be a warrior. You don’t even need to explore. With the world’s intelligence at your fingertips, you can be a sage builder of your life.”*

**Domain rename in development (differentiating from Warriors Way CORE 4):**
First instinct: *“Your money, your mind, your people, your Mecca.”* Direction is right — personal, possessive, aspirational. Words not yet final.

**Next steps for identity:**
- Continue playing with Kronerer/Bergfeurer phonetic variants (2 syllables or less, warm not abrasive, smooth in English)
- Test the fire-lit ridgeline as the visual progress metaphor in app prototype
- Trademark/domain clearance when a name lands
- Test “I am a ___” with Brynn, Christie, and 5 strangers who believe in intentional living
- Explore multi-brand potential: Stitser Way as family platform, public brand using fire identity once it lands
