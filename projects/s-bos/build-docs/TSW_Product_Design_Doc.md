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

> These are confirmed design decisions from the discovery session. They will be formalized into §3–§5 once Gate 1 is signed off. Organized into seven areas. Source documents referenced where applicable.

---

### A — Navigation & Shell

- Four persistent bottom tabs: **Today / Horizon / Me / Shortcuts**
- Me tab is a domain menu — four domain cards (Body/Being/Balance/Business), tap domain → Goals list, tap Goal → universal initiative screen
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
- Kompass makes one suggestion with reasoning (e.g. "Today looks like a Buffer Day — 3 overdue, 2 replies ready, last buffer was Tuesday")
- Clint responds: **uh-huh** (confirms) or **uh-uh** (triggers three-option picker: Focus / Buffer / Free)
- Max two questions, never open-ended
- Mode badge displayed in nav — tap to switch

**Focus Day behavior:**
- Tab bar hides. Only anchor item + Pomodoro timer visible
- Kompass runs silently in background (email sweep continues, no digest surfaced)
- Grounding affirmation shown — sourced from operating manual
- Break-glass button: low-visibility, requires intentional tap, reveals task/email/stack/capture options
- Meeting filter: only meetings serving a named active Focus initiative are permitted

**Buffer Day behavior:**
- Full app visible. Horizon Rings is the command center
- Three sub-tabs: Horizon (rings + dual-mode toggle) / Email (reply queue with drafts ready) / Big 3 (set today's dominos)
- Buffer session runs Tuesday (team meeting anchor) and Wednesday (development meeting anchor)
- Kompass presents one item at a time with full context and a draft or recommendation attached
- Two guardrail questions fire on any commitment decision involving another person's agenda:
  - Q1: *"Staying focused on your ideal circumstances — what do you actually want here?"*
  - Q2: *"Is assuming this situation a people-pleasing tendency, or a hell yes from your gut?"*

**Free Day behavior:**
- Calendar + wins ("Look how far we've come") + Table Talk prompt only
- No tasks, no inbox, no agenda
- Mandate displayed: "Your job today is to wander"
- Kompass off — app gets out of the way
- Protects the Channel of Inspiration (1-8) — aimless time is where best ideas surface

**Week architecture:**

| Day type | Protection rule |
|---|---|
| Focus Day | Full day — no reactive admin, meetings only if serving a named active initiative |
| Buffer Day (Tue) | Team meeting anchor + clearing session |
| Buffer Day (Wed) | Development meeting anchor + clearing session |
| Free Day | Full off — zero business |

---

### C — Horizon Rings (daily planning screen)
*Sources: `06-platform-design/horizon-rings-design-spec.md` + `skills/daily-horizon-scan/SKILL.md` in Clint-s-Kompass repo*

**Core principle:** Items earn their way into view through signal, not assignment.

**Five rings:**

| Ring | Label | Signal |
|---|---|---|
| 🔴 | Overdue | Past due date |
| 🟡 | This Week | Due within 7 days, OR aging 48hr+ untouched |
| 🔵 | Active | No due date, currently in motion |
| 🟢 | Coming Soon | Due 8–21 days out |
| ⚪ | Parked | Due 22+ days, or explicitly snoozed |

**Three data sources (all pull into the same ring logic):**
1. SmartSuite Check List Tasks — assigned to Clint, not complete/cancelled (App ID: `68a8e17251dc814e8c529f3f`)
2. Notes & Comments follow-ups — assigned to Clint, follow-up date ≤ today + 7 days (App ID: `6894e64f621641b3ef90fa99`)
3. GYR Status Reports — Clint is follow-up owner, follow-up not completed, Red or Yellow status

**Display rules:**
- Maximum 7–10 items in the active scan view at any time
- Priority stack order: Overdue → GYR Red → Notes past due → Active Big 3 → Aging This Week
- Coming Soon and Parked collapsed by default — expandable on tap
- Each item shows: title, source icon (Task ✅ / GYR 📊 / Note 📝), linked project, due date or "no date"
- Phase Context Strip above rings: one chip per active product line showing current phase (tappable → expands to full phase checklist)

**Dual-mode toggle (top-right icon, not a tab):**
- **Stacked list (default):** Full triage functionality. Every item readable and actionable. Done/Park/Kill/Snooze inline on each card.
- **Circles overview:** Concentric rings, spatial layout. Items as dots on their ring. Tap ring to filter. Tap dot to inspect — detail card appears below with actions. Tap "→ Focus" to return to stacked with that item highlighted in gold.
- The bridge: orient in circles → identify what has pull → tap Focus → land on item in list → act.

**Actions per item:** Done / Park (pushes due date 14 days) / Kill / Snooze (N days) / Pin as Anchor

**Sacral Anchor:**
- Prompt: *"What has the most pull for you today?"*
- Surfaces top 3 actionable items as options
- User selects one — becomes the day's anchor, pinned at top for the rest of the day
- No algorithmic prioritization — gut only
- The Sacral Anchor answer informs Domino #1 of the Big 3 (the bridge between Horizon and Today tab)

**Quick Clear mode (bulk triage):**
- Triggered by button — not the default view
- Surfaces items with no due date AND no activity in 7+ days
- Presents in batches of 3–5
- Commands: Done / Park / Kill / Keep — parsed and batch-filed at end of session

**What Horizon Rings is NOT:**
- Not a full task manager — items are managed in SmartSuite, not here
- Not a calendar — no time blocks, no scheduling
- Not pressure-generating — no countdown timers, no urgency language

**Relationship to Claude:** When Claude runs a Horizon Scan conversationally (via the `daily-horizon-scan` skill), it reads the same data and applies the same ring logic. The app screen and Claude are two surfaces of the same model. Claude can update item status from conversation; the app reflects the change.

---

### D — Kompass Operating Platform (three-layer architecture)
*Source: `06-platform-design/kompass-operating-platform.md` in Clint-s-Kompass repo*

The platform exists to match Clint's burst-and-drift wiring. Everything requiring sustained attention, sequential processing, or remembering to do something is removed from Clint and handled by the system.

**Three layers:**

| Layer | Guardrail | What it solves |
|---|---|---|
| 1 | Second Brain | Working memory is a sieve — externalize everything automatically |
| 2 | Buffer Anchor | Executive Function Gap + unfinished pile pressure — clear with body doubling |
| 3 | Genius Schedule | Genius gets colonized by others' urgency — protect, design, and review daily |

**Layer 1 — Second Brain (Capture & Externalize):**
`/capture` activates intake from any input: typed text, voice memo, Remarkable photo, dictation.

Classification routing table:

| Type | Signals | Destination |
|---|---|---|
| Task / commitment | Action verb, person, deadline implied | Check List Tasks |
| Journal / reflection | Emotional content, first-person processing | Journals/Rituals |
| Idea / brainstorm | Speculative, "what if" | Principles & Realizations |
| Communication draft | Named person + message intent | Held for Buffer session |
| Decision needed | Open question, competing options | Decisions app |
| Stack trigger | Frustration, anger, friction language | Flagged for next Morning Ritual |
| Knowledge update | Factual update about business/people | GitHub — Clint-s-Kompass |
| Calendar / meeting note | Date, time, meeting reference | BAC-Calendar Events |
| Email request / inbound ask | Someone needs something from Clint | Check List Task + Notes & Comments thread |

Ambiguous submissions get two options and a gut check — never an open-ended question.

**Layer 2 — Buffer Anchor:**
- Email triage model: Gmail → Kompass reviews → Creates Check List Task (title = clear action, `Who` field = requestor) + Notes & Comments thread (type = "Email Request") → Surfaces in Buffer session with draft reply attached → Clint approves → Kompass sends → status updated
- **`Who` field privacy rule:** Tasks with no `Who` tag (or Clint-only) are invisible to anyone else. Kompass only adds a person to `Who` when the task genuinely involves them.
- Meeting minutes intake: Kompass reviews Google Calendar event details + uploaded Plaud transcripts → extracts commitments, decisions, follow-ups → routes to Check List Tasks or next Buffer session

**Layer 3 — Genius Schedule:**
- Daily (not weekly) calendar design and review
- Morning: review day against day type, flag violations, confirm Big 3 protected
- Midday (if needed): re-plan afternoon
- Evening: review actual vs. planned, surface commitments and follow-ups
- Weekly Monday audit: Focus Day violations, Buffer Days without clearing sessions, Free Days with business creep, weeks with no Free Day

---

### E — Universal Goal Engine (five steps)

One template powers every initiative across every domain. Domain-specific fields layer on top.

1. **Current Score** — where you are now vs target (pulled from SmartSuite Stats + Goal formulas)
2. **New Goal + Deadline** — what you're aiming for, by when (SmartSuite Goal record: target amount + due date)
3. **Rhythm & Reminders** — reporting frequency (weekly/monthly), chunked execution, staged learning connected to the why
4. **Progress Tracking** — visual + numerical. SmartSuite Stats as the data source. Fields: % Metric Complete, % Time Complete, Balance to Finish, Reporting Grade
5. **Celebrate Wins** — ritual of confirmation, GYR grade update, scoreboard, phase advancement

**Applies to everything:** Body weight loss, alcohol reduction, training, business phase gates, relationship habits, reading goals. Same five steps, different stat types on top.

---

### F — Shortcuts Tab (full inventory)
*Sources: plugin layer (`/mnt/skills/plugins/`), user layer (`/mnt/skills/user/`), Clint-s-Kompass skills*

**Claude Skills — Personal:**

| Skill | Trigger | Plugin |
|---|---|---|
| Morning Ritual | "morning ritual", "start my ritual" | stitser-way |
| Evening Ritual | "evening ritual", "let's do the evening ritual" | stitser-way |
| Weekly Review | "weekly review" | stitser-way |
| WAR Stack | "WAR stack", "let's do a WAR stack" | stitser-way |
| Cash Stack | "cash stack" | stitser-way |
| Irritation Stack | "irritation stack" | stitser-way |
| Anger / Rage Stack | "anger stack", "rage stack" | stitser-way |
| Guilt Stack | "guilt stack" | stitser-way |
| Gratitude Stack | "gratitude stack" | stitser-way |
| Excitement Stack | "excitement stack" | stitser-way |
| Discovery Stack | "discovery stack" | stitser-way |
| Free Write / Journal | "free write", "I want to journal" | stitser-way |
| Strategic Vision | "strategic vision" | stitser-way |
| Project Debrief | "project debrief" | stitser-way |
| Daily Horizon Scan | "horizon scan", "what's alive", "scan my tasks" | kompass-ea / user |
| Quick Capture | "capture this", "file this" | user |
| Relationship Coach | "[family member name] + relational context" | user / Clint-s-Kompass |
| Buffer Session | "let's do the buffer session", "clear the queue" | kompass-ea / user |
| Email Triage | "sweep my email", "what's in my inbox" | kompass-ea / user |

**Claude Skills — Business:**

| Skill | Trigger | Plugin |
|---|---|---|
| Baseline a Project | "baseline this project" | user |
| Pay App | "kick off pay app" | user |
| Pay App Audit Checklist | per-invoice PM audit | user |
| Underwrite a Deal | "let's underwrite a deal" | arbitrage-cfo / user |
| Strategic Review | "run Strategic Review for [entity]" | arbitrage-cfo |
| GP Cash Flow Schedule | "GP cash flow schedule", "forward GP" | arbitrage-cfo |
| Entity Budget | "finalize the [entity] budget" | arbitrage-cfo |
| Refresh Actuals | "refresh actuals for [entity]" | arbitrage-cfo |
| Year-End Audit | "run year-end audit for [entity]" | arbitrage-cfo |
| Interco Allocation Review | "review the interco file" | arbitrage-cfo |
| Platform Performance | "open the platform dashboard" | arbitrage-cfo |
| Brokerage Compliance Audit | "audit [property/contract]" | user |
| S-BOS Schema Registry | "refresh the schema" | sbos-system-admin |
| S-BOS Data Dictionary | "rebuild the data dictionary" | sbos-system-admin |
| S-BOS Training Docs | "write training docs for [feature]" | sbos-system-admin |
| Skill Management | "I want to create a skill" | user |
| Duplicate Contact Merge | "merge these two records" | user / organization |

**External Tools Grid:**

| Tool | Category | Deep link |
|---|---|---|
| Google Calendar | Personal | calendar.google.com |
| Gmail | Personal | mail.google.com |
| Google Drive | Personal | drive.google.com |
| Strava | Personal / Body | strava.com |
| S-BOS / SmartSuite | Business | app.smartsuite.com |
| Phase Anchor | Business | sb-planning-tools-production.up.railway.app |
| Sage Intacct | Business | intacct.com |
| GP Cash Flow | Business | (Railway app) |

---

### G — In-App Spec Sheet (living design doc)
*Source: v2 prototype (`kompass-os-v2.html`) — built during discovery session*

The spec sheet is built into the app as its own screen. It is the checklist of principles, tools, and features Clint references during build and testing. Not a separate document — lives inside the product it describes.

**Structure:**
- Grouped by section with collapsible headers
- Each row: Feature / Principle & Framework / Tool / Status
- Status cycles (tap to advance): Designed → In Build → Live → Parked
- Cells are tap-to-edit inline
- Add row button per section
- Summary pill strip at top: count per status across all sections

**Ten sections with seed content:**

**Section 1 — Today**

| Feature | Principle / Framework | Tool |
|---|---|---|
| Daily greeting + sub-status | Live with intention — morning anchor sets direction | SmartSuite Journals |
| GYR domain strip (4 tiles) | GYR Audit — Claude surfaces current status per domain | SmartSuite + Claude |
| Spiral nudge (context-aware) | GYR Spiral — surfaces Yellow/Red domains for processing | Claude / Spiral engine |
| Big 3 dominos | RPM / 80-20 — what 3 moves actually move the needle | SmartSuite Tasks |
| Ritual habit strip | Habit Identity Loop — proof of showing up | SmartSuite Journals |
| Table Talk (Hi/Lo/Buffalo) | Brynn's Ritual — family check-in, celebration, connection | SmartSuite (custom) |

**Section 2 — Plan**

| Feature | Principle / Framework | Tool |
|---|---|---|
| Big Ass Calendar (monthly) | Look how far we've come + what's coming | Google Calendar API |
| Week at a Glance | Guided chunking — focus in 7-day window | Google Calendar |
| Quarterly Habit Builder | Spaced repetition + Phases of Proficiency | SmartSuite Habits |
| Habit streak + dot tracking | Habit Identity Loop — streak = identity proof | SmartSuite |
| Upcoming events with domain tagging | Calendar as scoreboard for life balance | Google Calendar |

**Section 3 — Tasks (Horizon Rings)**

| Feature | Principle / Framework | Tool |
|---|---|---|
| All open tasks (filterable, max 7–10) | Items earn their way in through signal, not assignment | SmartSuite Tasks |
| Notes & Comments surface | Capture → triage → file → act | SmartSuite Notes |
| GYR Follow-Up tracker | GYR Audit output — Red/Yellow items need action | SmartSuite + Claude |
| Domain tagging on every item | Body/Being/Balance/Business — everything has a home | SmartSuite |
| Overdue flagging | Facts vs goals — can't hide from reality | SmartSuite |
| Dual-mode toggle (stacked/circles) | Step back to orient, zoom in to act | Claude / UI |
| Sacral Anchor prompt | Gut over algorithm — what has pull | UI |
| Quick Clear bulk triage | Noise removal without pressure | SmartSuite |
| Phase Context Strip | Bigger picture always visible | Phase Anchor |

**Section 4 — Domains**

| Feature | Principle / Framework | Tool |
|---|---|---|
| 4 life domain cards (B4) | Body / Being / Balance / Business — the four containers | SmartSuite + Phase Anchor |
| Phase gate progress per domain | Phase-based accountability — define done, track it | Phase Anchor |
| How We Know checklist (tap-to-check) | Team defines HOW, Clint defines WHAT done looks like | SmartSuite |
| Product Line sub-cards with links | Allocator seat — context at a glance, drill on demand | Phase Anchor + Railway |
| Spiral entry point per domain | GYR Spiral — transformation engine per life area | Claude |
| Domain links (Strava, Drive, S-BOS) | Single system discipline — one tap to source of truth | Various |

**Section 5 — Journal**

| Feature | Principle / Framework | Tool |
|---|---|---|
| Filterable journal feed | Date and progress-based filing system | SmartSuite Journals |
| New Journal / Stack launcher (FAB) | Capture immediately — voice or text | Claude Skills |
| Table Talk history | Brynn's Ritual — permanent family record | SmartSuite |
| All Stack types (WAR, Cash, Irritation…) | Stacks = structured emotional processing | Claude + SmartSuite |
| Spiral Journals (full 7-step) | Facts → Feelings → Root → Focus → Actions → Fruit | Claude + SmartSuite |

**Section 6 — Spiral**

| Feature | Principle / Framework | Tool |
|---|---|---|
| 7-step guided Spiral flow | GYR Spiral — the transformation engine | Claude |
| Domain selector at step 1 | Spiral in all areas — Body, Being, Balance, Business | Claude |
| Prior answers visible as you progress | Facts-first — build the full picture before acting | Claude |
| File to Journal on completion | Capture → file — no lost insight | SmartSuite Journals |
| Claude contextual coaching per step | Kompass AI — context-aware guidance | Claude API |

**Section 7 — More / Tools**

| Feature | Principle / Framework | Tool |
|---|---|---|
| External tools grid | Single-system discipline — one place to reach everything | Various deep links |
| Claude Skills launcher (28+ skills) | Kompass Launcher — prompt copied + project opens | Claude Projects |
| Scoreboard (all domains + product lines) | Facts vs goals — always the North Star | Phase Anchor + SmartSuite |
| Family profiles | Customized to each person's wiring — Human Design | GitHub + Claude |
| Settings / preferences | Don't fight natural rhythm — app adapts to user | Local storage |

**Section 8 — Principles & Frameworks**

| Feature | Principle / Framework | Tool |
|---|---|---|
| GYR Spiral | Facts → Feelings → Root Cause → Focus → Massive/Relevant Actions → Fruit | Claude (guided) |
| Body / Being / Balance / Business | Four containers — everything in life has a home | All screens |
| Habit Identity Loop | I am the kind of person who… — identity before behavior | Plan tab / Habits |
| Phase-Based Accountability | Phase gates replace willpower-based quarterly plans | Domains tab |
| Human Needs (Tony Robbins) | Growth, Contribution, Stability, Variety — wired into all coaching | Claude context |
| Phases of Proficiency | Install → Beginner → Intermediate → Expert → Teach → Sub-Conscious | Domains / Habits |
| Brynn's Table Talk Ritual | Hi / Lo / Buffalo — dinner table IS the ritual | Today + Journal |
| Machine Definition | A device that procedurally takes inputs and converts them to outputs | Entire app |
| Kids Own Their Data | Age-appropriate autonomy — data entered by each person | Family profiles |
| Never in 100% Balance | Balance is a myth — intentional imbalance with awareness | Today / GYR strip |
| Spaced Repetition | Sleep locks in learning — reminders timed to protocol stages | Body section |
| 80/20 + Chunking | Most results come from few actions — focus on the needle movers | Big 3 / Horizon |
| Sacral Decision Model | Gut over algorithm — uh-huh / uh-uh, max two questions | Day Mode / Horizon |
| Consecutive Appetite | One thing at a time, full completion before switching | Focus Day / Buffer |
| Celebrate Progress (The Gap) | Acknowledge how far you've come, not just how far to go | Scoreboard / Wins |

**Section 9 — Body Domain**

| Feature | Principle / Framework | Tool |
|---|---|---|
| Three-phase protocol | Foundation → Engine → Race Block (evidence-based fat loss + MTB training) | SmartSuite Goals |
| Phase gate criteria (How We Know) | Sleep locked + alcohol cut / Strength added + riding polarized / Race-fueled | SmartSuite |
| Weight tracker (daily log → weekly trend) | Waist-to-height ratio primary; weight trend secondary | SmartSuite Stats |
| Body fat % history (DXA upload) | DXA gives true visceral fat baseline — better than BMI | File upload + Stats |
| Meal tracker (Mediterranean green diet) | High polyphenols, high protein, no liquid sugar, walnuts + green tea | SmartSuite Stats |
| Alcohol / drinking log (streak + daily count) | Dose-dependent visceral fat driver — reduction is highest-yield change | SmartSuite Stats |
| Training log (Strava pull) | Polarized riding: 80% Zone 2, 20% hard intervals; not more volume | Strava MCP |
| Staged learning reminders | Connected to the why — cortisol/testosterone triangle, sleep ROI, etc. | Claude / reminders |
| Health vault (DXA, blood, eyes, skin) | Historical records — upload + date, searchable | File upload |
| Strava race countdown | Downieville July 23, Grizzly 100 Sep 5 — shift to maintenance Jul 15 | Strava MCP |
| Metrics dashboard | Waist circ, HRV/RHR, watts on Mt. Dyer, sleep consistency | Strava + SmartSuite |

**Section 10 — Data & Infrastructure**

| Feature | Principle / Framework | Tool |
|---|---|---|
| SmartSuite Game App (Phase 1) | Existing data model — no new schema to launch | Kompass MCP |
| Supabase migration (Phase 2) | API limits + Claude-serviceability + no vendor lock-in | Supabase |
| Strava MCP integration | Live training data, no manual entry | Strava MCP |
| Google Calendar MCP | Day-mode suggestion engine + event display | Google Calendar MCP |
| Gmail MCP | Email triage in Buffer Day | Gmail MCP |
| Claude API (Anthropic) | Spiral, coaching, day-mode, capture classification | claude-sonnet-4-6 |
| Railway hosting | Separate deployment from S-BOS | Railway |
| SmartSuite Journals/Rituals | All rituals, stacks, table talk filed here | App ID: `68f8f8fe3757414d70d94ae0` |
