# Product Design Doc: Stitser Way

> **Status:** 🔄 In Progress — Section 1 drafted, Gate 1 pending sign-off
> **Methodology:** Ryan Falke's Design Templates, adapted for Stitser Way
> **Decision-maker:** Clint Stitser
> **Last updated:** 2026-06-25

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

Most people who believe in intentional living have the same experience: the knowledge exists, the desire exists, and the tools exist — but they live in pieces. A habit tracker here. A journal there. Goals in a spreadsheet. Reminders on a sticky note. Coaching in a chat window. Training data in an app that doesn't talk to anything else. And at the center of it all, a person who has to manually hold the whole system together in their head every day.

For Clint, this produces three compounding problems:

**1. Fragmentation.** The infrastructure exists — SmartSuite holds the goals and GYR data, Claude runs the rituals and stacks, Google Calendar holds the year, Strava holds the training, GitHub holds the family profiles. But none of these surfaces know what the others are doing. There is no single place where the full picture of a life is visible, connected, and actionable.

**2. Absence.** There is no application designed around the actual frameworks and principles of intentional living — the GYR Spiral, the four life domains, phase-based accountability, the Container Model, spaced learning, the day-mode model, Brynn's Table Talk ritual, the Vivid Vision, the Misogi, the Quarterly Habit arc. These frameworks exist in Clint's operating system. They do not exist as a coherent, daily-use product.

**3. The activation gap.** Even with the knowledge and the frameworks, sustained installation requires a daily machine. Without it, great insights fade. Goals go untracked. Rituals drift. The GYR Spiral runs once, then gets forgotten. The Vivid Vision is written in January and not looked at until December. The knowledge is there — but there is no system that installs it into the body, the habits, and the daily rhythm, one concept at a time, until it becomes identity.

**4. No project-level tool layer.** Life is not just habits and domains — it contains bounded projects with specific, temporary needs. A trip to Europe needs a budget. A child's ear infection needs a medication schedule. A test needs a study app. Today, these tools are either cobbled together manually, outsourced to generic apps, or not built at all. There is no system that spins up a purpose-built Claude tool for a specific project stage, tracks it through completion, and archives it for future reference or sharing.

### The solution in one sentence

> *"The app is a machine that procedurally produces a better life. You don't need to arrive fully formed. The machine installs you into clarity over time."*

### What a good solution looks like

A single application that integrates the entire practice of intentional living — bringing together the principles, tools, rituals, tracking, coaching, family coordination, and project-level tool-building that Clint has built over years — into one coherent daily experience. A machine, not a collection of tools. One that:

- Surfaces the right information at the right time based on day type, domain status, and what's alive
- Installs frameworks and knowledge through staged, spaced learning — not information dumps
- Connects every domain of life (Body, Being, Balance, Business) to goals, phases, habits, and progress in one place
- Integrates Claude natively as the coaching, processing, suggestion, and tool-building layer — not a separate chat window
- Allows Claude to build custom tools for bounded projects — scoped to the project, used through its lifecycle, archived or shared on completion
- Serves Clint first, and by extension his family, with each member owning their own data and experience

### What this is NOT

- Not a task manager — tasks are one input among many
- Not a journal app — journaling is one output among many
- Not a replacement for S-BOS — that is the business operating system; this is the personal one
- Not a collection of tools glued together — it is a machine with a single coherent logic
- Not finished when the data is entered — it is always working, always suggesting, always tending
- Not limited to recurring life domains — bounded projects with temporary, Claude-built tools are a first-class citizen

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

## Discovery Inputs (from session 2026-06-24 / 2026-06-25)

> These are confirmed design decisions from the discovery session. They will be formalized into §3–§5 once Gate 1 is signed off. Organized into fourteen areas.

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

**Free Day:** Calendar + wins + Table Talk prompt only. No tasks, no inbox. Mandate: "Your job today is to wander." Kompass off.

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

**H1 — Vivid Vision & Annual Commitments:** 10-year vision + 1-year commitments surfaced inside Clint's profile. Google Doc opens natively (Drive link, no API auth). Daily rotating reminder tied to morning ritual. Annual update prompt each New Year.

---

### I — Big Ass Calendar
*Source: thebigasscalendar.com*

Year-at-a-glance visual. Three elements: Misogi (year-defining event) + Kevin's Rule (bimonthly adventures) + Quarterly Habit. Two views: full-year color-coded (backward "look how far" layer + forward "what's coming" layer) + month/week drill-in. Surfaces on Free Day Today tab.

---

### J — Quarterly Habit

One habit at a time — Consecutive Appetite model. Five-stage arc: Install → Beginner → Intermediate → Expert → Complete. Freud's sense of achievement at completion. Staged learning (3–5 lessons, one per day, first 2 weeks). Celebration at 7/14/21/30/60/90 days + quarter end. Identity statement generated at completion.

---

### K — Key Docs

Google Drive link registry for critical personal and family documents. Links open Drive natively — no API auth. Nine categories. Person selector + Family tab. Emergency-access flag. Cross-references Body health vault.

---

### L — Container Model & Learning Engine

**L1 — Container Model:** Three-layer empty state (soft glow + clear invitation + progress ring). Every container has a "why this matters" message before the build prompt. Build flow: Claude-guided conversation → filed to right destination → container fills → progress ring updates → celebration. Refresh cycles per container type.

**L2 — Learning Engine:** Ebbinghaus forgetting curve + spaced repetition + sleep as the training session. One concept per card. Named bounded lessons with time estimates. Single Next button. 6-hour safety rail. Fires on: Quarterly Habit install, Body protocol, new container build, first Spiral, first family profile, S-BOS skill onboarding (cross-product).

---

### M — Brand Identity & Positioning

**Working name:** Stitser Way — a good placeholder that may stand the test of time.

**Identity archetype confirmed:** Sage-Architect-Builder. Starting posture is gratitude, not combat. Embodied through daily pursuit, never finished.

**Closest identity noun:** The Steward. Steward synonyms explored: Keeper (top candidate), Cultivator, Tender. Sovereign/Craftsman/Author — over-claimed, avoid.

**Austrian heritage thread (developing):** Herz-Jesu-Feuer / Bergfeuer / Sonnwendfeuer as brand story. Kronerer as most resonant identity archetype. Names explored: Bergfeuer, Kronerer, Krone Way, Feura, Luma Way — none landed. Running with Stitser Way as placeholder.

**Confirmed positioning statements:**
- *"The app is a machine that procedurally produces a better life. You don't need to arrive fully formed. The machine installs you into clarity over time."*
- *"No need to be a warrior. You don't even need to explore. With the world's intelligence at your fingertips, you can be a sage builder of your life."*

**Domain rename in development:** First instinct: *"Your money, your mind, your people, your Mecca."* Direction right — words not yet final.

**Full detail:** `03-stitser-way/messaging.md` in Clint-s-Kompass repo.

---

### N — Project-Based Tool Layer (Claude-Built Tools)

**What it is:** A project-scoped tool-building system where Claude builds custom mini-apps, trackers, checklists, and schedulers for bounded life projects. Tools are created for a specific project stage, used through that stage's lifecycle, then archived for future reference or sharing with others. This is distinct from the recurring life domain framework (Body/Being/Balance/Business) — it handles the temporary, bounded, specific needs that arise throughout life.

**The core insight:** Not everything in life is a habit or a domain. Some things are projects — bounded in time, specific in need, complete when done. A trip to Europe isn't a Body goal. An ear infection isn't a Being ritual. An AP Chemistry test isn't a Business phase gate. These need their own tools, purpose-built for their stage, scoped to their project.

**Project hierarchy (SmartSuite S-BOS project infrastructure as data source):**

```
Master Project     → School Year 2026–27
  Child Project    → AP Chemistry
    Grandchild     → Midterm Exam — Oct 15
      Tool         → Claude-built study app: flashcard quiz + spaced review schedule

Master Project     → Europe Trip — Summer 2027
  Child Project    → Budget
    Tool           → Claude-built trip budget tracker with categories + running total
  Child Project    → Itinerary
    Tool           → Claude-built day-by-day schedule with logistics

Master Project     → Ear Infection — Max, Jun 2026
  Child Project    → Treatment Protocol
    Tool           → Claude-built medication schedule: amoxicillin dosage + timing reminders
```

**Tool lifecycle — four stages:**

| Stage | What happens | App behavior |
|---|---|---|
| **Create** | Claude builds the tool scoped to the project stage | User describes the need → Claude generates the tool → tool attaches to the project |
| **Active** | Tool is in use for the duration of the project stage | Tool accessible from project card, surfaces in Horizon Rings if tasks are attached |
| **Complete** | Project stage ends, tool has served its purpose | Tool marked complete, project stage closes |
| **Archived** | Tool stored for future reference or sharing | Accessible from project archive, shareable with other users or family members |

**Tool types (Claude generates based on need — not a fixed menu):**

| Type | Example |
|---|---|
| Tracker | Trip budget, weight during a protocol, medication doses |
| Checklist | Packing list, pre-race checklist, medication schedule |
| Study app | Flashcard quiz, spaced review schedule, concept summary |
| Scheduler | Day-by-day itinerary, treatment timeline, exam prep calendar |
| Calculator | Budget with categories, dosage calculator, grade projector |
| Reference | Research summary, rules for a sport, recipe scaled for group |

**Data model (confirmed):** Projects live in the existing S-BOS SmartSuite project infrastructure — Master Project → Child Project → Grandchild Project hierarchy. Tools are Claude artifacts attached to a project record. Phase 1: tools stored as Claude artifact HTML files linked to the SmartSuite project record. Phase 2: tool storage migrates to Supabase alongside the rest of the data layer.

**Relationship to the rest of the app:**
- Projects surface in **Horizon Rings** — active project tasks appear in the rings by due date and urgency
- Projects belong to a **life domain** — a school project is Balance (family/kids), a trip is Balance or Being, a medical protocol is Body
- Completed tools go to the **Archive** — accessible from the project record and browsable in a tool library
- Shareable tools flow through **Family Profiles** — a study app built for Avery can be shared with Brynn for a similar test

**The archive as a library:** Over time, the collection of completed Claude-built tools becomes a personal library — a study app for AP Chemistry could be adapted for AP Biology; a medication schedule for one ear infection becomes the template for the next. The archive is searchable by project type, domain, family member, and tool type.

**What this is NOT:**
- Not S-BOS project management — S-BOS handles business projects (construction, development, brokerage). This handles personal/family projects.
- Not a pre-built app library — Claude builds each tool fresh for the specific need. The archive stores the outputs, not a catalog of templates.
- Not permanent infrastructure — tools have a lifecycle and are expected to be completed and archived, not maintained indefinitely.

**Open question (flag for §9):** What is the UX for creating a new tool? Is it: (a) tap into a project → "Build a tool for this stage" → Claude conversation, or (b) a Claude skill trigger from anywhere ("build me a tool for...") → attaches to the project? Both may be valid entry points.
