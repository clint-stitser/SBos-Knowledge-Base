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

> These are confirmed design decisions from the discovery session. They will be formalized into §3–§5 once Gate 1 is signed off. Organized into eleven areas. Source documents referenced where applicable.

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
| Big Ass Calendar (year view) | Intentional year design — offense not defense | Google Calendar API |
| Week at a Glance | Guided chunking — focus in 7-day window | Google Calendar |
| Quarterly Habit (one at a time) | Freud's sense of achievement + Consecutive Appetite | SmartSuite Goals |
| Habit streak + dot tracking | Habit Identity Loop — streak = identity proof | SmartSuite Stats |
| Misogi — year-defining event | One bold goal that defines the year — slightly terrifying | SmartSuite Goals |
| Kevin's Rule — bimonthly adventure | Newness = alive — one thing you wouldn't normally do, every other month | SmartSuite / Calendar |

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
| Big Ass Calendar System | Designed year = remembered year — offense not defense | Plan tab |
| Freud's Sense of Achievement | Completing a habit installs identity AND produces joy — not just behavior change | Quarterly Habit |
| Misogi | One year-defining event — slightly terrifying, deeply personal | Big Ass Calendar |
| Kevin's Rule | Newness is aliveness — one adventure every other month for 30 years = 180 life experiences | Big Ass Calendar |

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
| GitHub (Clint-s-Kompass) | Profile files for About Me section — read via GitHub API | GitHub MCP |

---

### H — About Me & People Around Me
*Sources: `01-user-profile/` + `07-family/` in Clint-s-Kompass repo*

**What it is:** A dedicated section within the Me menu that surfaces full personal and family profiles — scrollable, searchable, and always available. The equivalent of a relationship and self-knowledge library living inside the app. Not a summary — the full profile for each person.

**Navigation location:** Me menu → "About Me & People Around Me" (own section alongside the four domain cards, Journal, Tools, and Spec Sheet)

**Data source:** GitHub — `Clint-s-Kompass` repo, markdown files fetched via GitHub API at read time.

> ⚠️ **Integration note:** This is the only section of the app that reads from GitHub rather than SmartSuite. The app fetches and renders markdown files directly. This is a different integration pattern from all other sections and must be called out explicitly in the Technical Spec and Data Integration Doc. Phase 2 consideration: migrate profile content to Supabase so it's editable in-app without a GitHub commit.

**Profile files (Phase 1 sources):**

| Person | File | Contents |
|---|---|---|
| Clint | `01-user-profile/operating-manual.md` | Human Design (Sacral MG, 5/2, Channel 1-8), ADHD assessment scores, cognitive mechanics, vulnerabilities/shadows, AI interaction principles, tactical guardrails, decision-making protocol |
| Clint | `01-user-profile/quick-reference.md` | Condensed operating manual — key reminders at a glance |
| Clint | `01-user-profile/vivid-vision-2036.md` | 10-year vivid vision (also linked as Google Doc — see Vivid Vision section below) |
| Clint | `01-user-profile/2026-commitments.md` | 1-year commitments (also linked as Google Doc — see Vivid Vision section below) |
| Christie | `07-family/christie-stitser.md` | Full profile |
| Avery | `07-family/avery-stitser.md` | Full profile |
| Brynn | `07-family/brynn-stitser.md` | Full profile |
| Maxwell | `07-family/maxwell-stitser.md` | Full profile |
| Gwen | `07-family/gwen-gifford.md` | Full profile |

**UI structure:**

```
Me → About Me & People Around Me
├── Clint (primary — shown first)
│   ├── Operating Manual (full, scrollable)
│   ├── Quick Reference (condensed)
│   ├── Vivid Vision & Commitments (see dedicated sub-section below)
│   └── [Other profile files]
├── Christie
├── Avery
├── Brynn
├── Maxwell
└── Gwen
```

**Features:**
- Person selector at top — tap avatar to switch profiles
- Full markdown content rendered per person — scrollable, not truncated
- Search bar — searches across all profiles simultaneously (name, keyword, concept)
- Clint's profile shown first and by default
- Each profile shows last-updated date (pulled from GitHub file metadata)
- Edit button per profile → opens GitHub file in browser (Phase 1) or in-app editor (Phase 2)
- Profile completeness indicator — surfaces which family members don't yet have a full profile

**Relationship to Claude:** When Claude runs any coaching session, relationship session, or day-mode suggestion, it reads these same profile files from GitHub for context. The app surface and Claude are reading from the same source. Changes to a profile file update both Claude's context and the app simultaneously.

**Phase 2 addition:** In-app profile editing — edits save directly to Supabase (no GitHub commit required). GitHub remains the backup/version history.

**Open question (flag for §9):** Gwen Gifford — relationship to Clint not explicitly stated in current profile files. Confirm whether she belongs in this section or a separate "Extended Family / Key People" section before building.

---

#### H1 — Vivid Vision & Annual Commitments (sub-section of About Me)
*Source: Google Doc `1KpYWZdRgeM93V79mp0sSStE57y-f9iWKwRu5pyYcIsI` — "2026 Annual SB Plan"*

**What it is:** A dedicated view inside Clint's profile that surfaces the 10-year Vivid Vision and 1-year commitments — the north star and the near-horizon in one place. Designed to be read regularly, not filed and forgotten. The document makes these intentions sticky by keeping them visible.

**The document contains four layers — only two belong in the personal app:**

| Layer | Belongs in Stitser Way? | Notes |
|---|---|---|
| Vivid Vision: January 2036 | ✅ Yes | 10-year ideal circumstances — Health, Environment, Family, Business, Wealth/Legacy |
| 2026 Commitments | ✅ Yes | 1-year vision across Body, Being, Balance, Business with measurable targets |
| The Manifesto + Daily Huddle | ❌ No — S-BOS | Company identity and code — business-facing |
| Annual Initiatives & Q1 Plan | ❌ No — S-BOS | Stitser BUILT strategic plan — business-facing |

**10-year Vivid Vision highlights (January 2036):**
- Health: 50 years old, more alive and strong than a decade ago — still competing in biking, swimming, running; snowboarding, fishing, golf as rituals
- Environment: Reno home as sanctuary + fluid movement between Reno, beach, mountains — lake house and beach house as refuges, private plane hours
- Family: Christie and Clint in beautiful flow, "interdependence" mastered — kids successfully launched, deeply connected, stewards of the platform; grandkids welcome; deep holiday traditions
- Business: Fully in the Allocator seat 4 days/week — no operations, no hiring, no daily "doing." Company is asset-based powerhouse, proprietary tools/tech, partners and customers see undeniable value
- Wealth: $200K/month cash flow from assets — succession plan in place, multi-generational security + individual pursuit balanced
- Legacy epitaph: *"Wise, Connected & Aligned, Shining Example of a Human Being"*

**2026 Commitments highlights:**
- **Morning & Evening Rituals:** Worthy of my own attention — day starts and ends with the Stitser Way ritual
- **Body:** Cardio age 7+ years under actual (Oura), heart rate trend flat or declining, skin clear and moisturized, weight 198–203 lbs
- **Being:** Choose to be a great man, not a nice man — connected to, worthy of, and capable of experiencing ideal circumstances — self-care as commitment, not luxury — present and connected
- **Balance:** In flow with people I care about — embracing and celebrating the strengths of those closest to me
- **Business:** Achieving at high level with efficient resources — all companies self-managing and self-funded — 6-month liquidity buffer — monthly strategy meetings at home — kids on salary + 401(k)

**UI design for this sub-section:**

```
Clint → Vivid Vision & Commitments
├── 🔭 10-Year Vision (January 2036)
│   ├── Health & Vitality
│   ├── Environment & Lifestyle
│   ├── Family & Connection
│   ├── Business & Career
│   └── Wealth & Legacy
├── 🎯 2026 Commitments
│   ├── Rituals
│   ├── Body
│   ├── Being
│   ├── Balance
│   └── Business
└── ✏️ Open in Google Doc → [Drive link opens natively]
```

**"Keep it alive" mechanics (from the document's own Next Steps):**
The document itself identifies what would make these intentions stick. The app implements them:
- **Daily Review:** A daily reminder surfaces one line from the Vivid Vision or 2026 Commitments — rotating, never the same two days in a row. Tied to the morning ritual.
- **Annual Collage:** Flagged as a future feature — visual representation of the Vivid Vision as a mood board (Phase 2)
- **Monthly with Christie:** A recurring calendar reminder — monthly long lunch with Christie to review and keep the vision alive (surfaced as a Kevin's Rule-style event in the BAC)
- **Annual Update:** Each New Year, Kompass prompts a review of both the Vivid Vision and commitments — last year's vision reviewed, next year's written

**Data source:** Google Doc opened natively via Drive link (no Drive API auth needed — sharing handled at Drive level per Clint's decision). GitHub repo also maintains markdown versions (`vivid-vision-2036.md` and `2026-commitments.md`) as Claude's reading copy.

**Relationship to 2026 Commitment Targets:**
The measurable 2026 commitments (198–203 lbs, cardio age 7+ years under actual, skin clear) map directly to SmartSuite Goals in the Body domain. The app connects the vision to the scoreboard — tap any commitment → see the corresponding Goal card with current progress.

---

### I — Big Ass Calendar
*Source: thebigasscalendar.com/pages/our-system — Jesse Itzler / Taylor Prokes system*

**What it is:** A year-at-a-glance visual that turns time into a designed artifact. Not a scheduling tool — a meaning-making and anticipation tool. The goal is to live on offense: design the year intentionally, then follow the plan.

**Core philosophy:** A great year doesn't happen by accident — it's designed. The Big Ass Calendar makes the entire year visible at once, solving Clint's object permanence challenge directly. When everything is visible, it becomes easier to say yes to what matters and no to what doesn't.

**The three elements of the BAC system:**

**1. The Misogi (year-defining event)**
One bold event that will define the entire year. Slightly terrifying, deeply personal. When you look back, you'll remember the year as "the year I did that." Could be a physical challenge, a trip, a business milestone, a life bucket item. Must be within the realm of possibility but go BIG. For Clint: Downieville Downhill (Jul 23) and Grizzly 100 (Sep 5) are current candidates.

**2. Kevin's Rule (bimonthly adventures)**
Every other month, do one thing you wouldn't normally do. Not expensive — just new. A hike, a polar plunge, a cooking class, a museum. Six new experiences per year. Over 30 years = 180 life-enriching experiences that wouldn't have happened otherwise. Intentionally scheduled on the calendar — newness doesn't happen by accident.

**3. Quarterly Habit** (see Section J — treated as its own feature, feeds back into the calendar visually)

**Navigation location:** Me menu → "Big Ass Calendar" (own section) AND surfaces as the primary view on Free Days (Today tab shows the calendar in Free Day mode — what's coming + look how far we've come)

**Two views:**

**Year view (the "big ass" view):**
- Full year on one screen — all 12 months visible simultaneously
- Color-coded by category: Misogi events, Kevin's Rule adventures, family milestones, races, trips, phase completions, quarterly habit milestones
- Backward layer (look how far we've come): completed events shown in a muted "achieved" color
- Forward layer (what's coming): upcoming events shown in full color with anticipation
- Object permanence solution: everything visible at once means nothing disappears from awareness

**Month/week drill-in:**
- Tap any month → expands to month view
- Tap any day → surfaces event detail + Google Calendar sync
- Add event button → tags by category (Misogi / Adventure / Family / Race / Milestone / Habit)

**Data source:** Google Calendar MCP (reads scheduled events) + SmartSuite Goals (reads phase completion dates, race dates, habit milestones) + manual entries for Misogi and Kevin's Rule adventures

**Relationship to Free Day:** On Free Day, the Today tab shows only the Big Ass Calendar year view (forward-looking) + the wins panel (backward-looking). No tasks, no inbox — just the designed year and how far you've come.

**Spec Sheet rows added:** Misogi, Kevin's Rule, year-view calendar, backward/forward layers

---

### J — Quarterly Habit (one at a time)
*Source: Big Ass Calendar system + Freud's sense of achievement + Phases of Proficiency framework*

**What it is:** One new daily winning habit per quarter. One at a time — full focus, Consecutive Appetite model. Not a habit tracker in the traditional sense — a habit installation arc that ends in identity-level change and the felt sense of achievement Freud identified as a core human need.

**The Freud connection:** Completing a habit arc isn't just behavioral — it produces genuine joy. The anticipation of mastery, the moment of competence, and the retrospective pride of "I did that" are what make habit-building feel meaningful rather than obligatory. The app's job is to make that arc visible and to celebrate each stage.

**The five-stage habit arc (maps to Phases of Proficiency):**

| Stage | Label | What it means | App behavior |
|---|---|---|---|
| 1 | Install | Habit chosen, first week | Daily prompt, staged learning about why this habit |
| 2 | Beginner | Weeks 2–4, streaks building | Streak tracking, encouragement, "you're doing it" |
| 3 | Intermediate | Month 2, consistency forming | Weekly reflection prompt, connection to domain goal |
| 4 | Expert | Month 3, feels automatic | Reduced prompting, identity language ("you're someone who…") |
| 5 | Complete | End of quarter | Celebration ritual, Freudian achievement moment, BAC milestone marked |

**One habit at a time — why:**
- Consecutive Appetite: Clint's digestion type is one thing at a time, full completion before switching
- Attention dilution: multiple habits compete for the same ignition energy
- Identity installation: one habit done fully becomes "who I am" — three habits done partially become "things I'm trying to do"

**What triggers a new habit each quarter:**
- Quarter ends (Jan 1, Apr 1, Jul 1, Oct 1)
- Kompass surfaces a Sacral question: *"What habit would make the biggest difference this quarter?"*
- One option suggested based on current domain GYR and Goal progress — Clint gut-checks yes/no
- Previous habit optionally continued or promoted to "installed identity" and removed from active tracking

**Staged learning (the why layer):**
Each habit comes with a short learning sequence — 3–5 bite-sized lessons delivered over the first 2 weeks via daily reminder. For Body habits: the science behind why (e.g. why morning rides improve cortisol/testosterone ratio). For Being habits: the framework behind why (e.g. why evening journaling improves morning clarity). Learning is connected to the habit — never disconnected motivation content.

**Celebration mechanics:**
- Week 1 completion: "Day 7 — the hardest week is behind you"
- Streak milestones: 14, 21, 30, 60, 90 days — each acknowledged with a specific message
- Quarter completion: full celebration ritual — journal prompt, BAC milestone marked, identity statement generated ("You're now someone who [habit]"), optional Table Talk entry
- The celebration IS the Freudian achievement moment — it has to feel real, not gamified

**Data source:** SmartSuite Goals (habit as a Goal record with daily Stats) + SmartSuite Journals (celebration entries)

**Current Q3 2026 habit candidate (from Body protocol):** Morning ride consistency — 3x per week minimum, targeting 5x by Race Block

**Spec Sheet rows:** Quarterly Habit arc, staged learning, celebration ritual, Misogi connection, Freud's sense of achievement

---

### K — Key Docs
*Navigation: Me menu → "Key Docs" (sibling section alongside About Me & People Around Me)*

**What it is:** A curated library of important personal and family documents — surfaced as named links directly into Google Drive. Not a document storage system — a one-tap reference panel that solves the "where did I save that?" problem for life's most important files.

**Core problem it solves:** Critical personal documents (birth certificates, passports, insurance, estate docs) exist in Google Drive but are buried and hard to find under pressure. When you need them — at a border, during a medical emergency, for a school enrollment — object permanence means they effectively don't exist unless they're visible. Key Docs makes them permanently findable in one place.

**Navigation location:** Me menu → "Key Docs" — own section, listed directly below "About Me & People Around Me" for natural grouping

**Data source:** Google Drive deep links — each entry is a named link that opens the corresponding Drive file or folder directly. No file storage in the app itself. Phase 2: links stored in Supabase; Phase 1: links maintained as a simple structured JSON config.

**Auth model (confirmed):** Links open Google Drive natively in the browser — no Drive API auth required. Sharing and access permissions are managed by Clint at the Drive level. The app is a link registry only. No Drive MCP needed for this section.

**UI structure:**
- Grouped by person and category
- Each entry: document name + category icon + last-updated date (pulled from Drive metadata where possible) + one-tap open
- Search bar — filter by name, person, or category
- "Add doc" button — opens a simple form: name, person, category, Drive link

**Category taxonomy (seed list — extensible):**

| Category | Icon | Examples |
|---|---|---|
| Identity | 🪪 | Birth certificate, passport, Social Security card, driver's license |
| Health | 🏥 | Immunization records, insurance cards, medical history, prescriptions |
| Legal | ⚖️ | Trust documents, will, POA, operating agreements |
| Financial | 💰 | Tax returns, account statements, estate inventory |
| Property | 🏠 | Deeds, titles, HOA docs, insurance policies |
| Education | 🎓 | Diplomas, transcripts, certifications |
| Vehicle | 🚗 | Titles, registrations, insurance |
| Travel | ✈️ | Passports, visas, travel insurance |
| Other | 📄 | Anything that doesn't fit above |

**Per-person structure:**

```
Key Docs
├── Clint
│   ├── 🪪 Birth Certificate → [Drive link — opens natively]
│   ├── 🪪 Passport → [Drive link — opens natively]
│   ├── 🏥 Immunization Records → [Drive link — opens natively]
│   ├── ⚖️ Trust Documents → [Drive link — opens natively]
│   └── ... (extensible)
├── Christie
├── Avery
├── Brynn
├── Maxwell
├── Gwen
└── Family (shared docs)
    ├── 🏠 Property Deeds → [Drive folder link]
    ├── 💰 Tax Returns → [Drive folder link]
    └── ...
```

**Features:**
- Person filter at top (same avatar selector pattern as About Me)
- "Family" tab for shared documents not belonging to one person
- Last-accessed indicator — shows when a doc was last opened from the app
- "Copy link" option per document — for sharing without opening
- Emergency access indicator — flag any doc as "emergency accessible" so it surfaces first under pressure

**Relationship to Body section:** Health vault in the Body domain (DXA reports, blood tests, eye prescriptions, skin records) cross-references Key Docs — a DXA report filed in Body health vault can also appear as a linked doc in Key Docs under the Health category for that person. Single source of truth — surfaced in two relevant places.
