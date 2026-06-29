# Kompass Goal Decomposition Guide
## How to Map Any Goal Into the Two-Layer System

*For Claude: Read this before proposing any goal structure. This is the instruction set for decomposing goals, commitments, and protocols into the correct records in the Kompass / S-BOS system.*

---

## The Two Layers — Always Identify Which You're Looking At

Every input belongs to one or both layers. Identify this first before doing anything else.

### Strategic Layer — Working ON the Business/Life
The compass. Where you're going and how you're organizing the pursuit.

```
Goal
  └── Phase / Priority / Sprint  (a time-bound focus window)
        └── Milestone / Mini-Sprint  (a discrete completion event)
```

**Signals that something is Strategic:**
- It describes a state you want to arrive at ("I weigh 198–203 lbs")
- It has a horizon (a year, a quarter, a phase of life)
- Success is measured, not just done
- Removing it would change the direction of the work

### Operational Layer — Working IN the Business/Life
The chores. What needs to happen regardless of whether it's tied to a strategy.

```
Project
  └── Outcome / Phase
        └── Tasks · Budget · Schedule · Checklists · People
```

**Signals that something is Operational:**
- It describes a thing to do or produce ("run the pay app for Lot 7")
- It has a start and end
- Done means done — it doesn't recur or accumulate
- It would exist even without a strategic goal attached

### The Connection — Roll-Up, Not Parent-Child
A Project **contributes to** a Priority. It does not **belong to** it.

- Never force a project under a priority as a child record
- Instead, tag the project with a contribution link to the relevant Priority
- One project can contribute to multiple Priorities across different time windows
- A Priority aggregates contributions — it watches and adds up, it doesn't own

---

## The Five Record Types — How to Distinguish Them

### 1. GOAL
**What it is:** The destination. A measurable end state with a horizon.

**How to recognize it:**
- Describes where you want to be, not what you're doing
- Has a clear success condition you can evaluate yes/no
- Lives at the annual or multi-year level

**Examples from Clint's 2026 Commitments:**
- "I weigh 198–203 pounds" → Goal (Body domain)
- "My cardio age is 7+ years under actual" → Goal (Body domain)
- "Each Stitser BUILT company is self-funded with operating cash flow" → Goal (Business domain)
- "CCSFT has 12 months liquid cash net of short-term liabilities" → Goal (Business domain)

**What it is NOT:** A thing you do. A process. A habit. A project.

---

### 2. PRIORITY / PHASE / SPRINT
**What it is:** A time-bound focus window that organizes pursuit of a Goal. The answer to "what are we concentrating on right now to move toward the Goal?"

**How to recognize it:**
- Has a time boundary (Q1, Summer, 90-day block)
- Narrows focus to a specific aspect of the Goal
- Multiple Priorities can exist under one Goal across time
- Progress toward it is measurable within the window

**Examples:**
- Goal: "Weigh 198–203 lbs" → Priority: "Cut phase — target 205 by end of Q2"
- Goal: "Cardio age 7+ years under actual" → Priority: "Base training block — Zone 2 focus, March–May"
- Goal: "Self-managing organizations" → Priority: "Install Division Leader for Construction by Q3"
- Goal: "6-month liquidity buffer" → Priority: "Q1 cash flow — close $X in GP before March"

**What it is NOT:** A goal (too big, no time box). A project (not a thing to produce). A habit (not time-bounded in the same way).

---

### 3. MILESTONE / MINI-SPRINT
**What it is:** A discrete completion event within a Priority. Binary — done or not done.

**How to recognize it:**
- A specific date or event that marks progress
- Completion is unambiguous
- Usually unlocks the next phase or confirms the Priority is on track

**Examples:**
- Priority: "Cut phase Q2" → Milestone: "Weigh-in at 205 by April 30"
- Priority: "Base training block" → Milestone: "Complete first metric century ride — May 15"
- Priority: "Install Division Leader" → Milestone: "Offer letter signed by August 1"
- Priority: "Q1 cash flow" → Milestone: "Surge Flats first home closed"

**What it is NOT:** A recurring task. A habit. A project with ongoing work.

---

### 4. PROJECT
**What it is:** A bounded initiative with defined scope and a lifecycle (create → active → complete → archived). Lives in the Operational Layer. May or may not be tagged to a Priority.

**How to recognize it:**
- Has a clear scope — you know when it's done
- Has its own budget, schedule, people, and tasks
- Would exist whether or not a Goal is attached to it
- Produces something (a closed deal, a filed permit, a completed job)

**Examples:**
- Surge Flats Lot 7 — Construction → Project (tags to Q1 GP Priority as contributor)
- Cold Creek Entitlement → Project (tags to Developer Kompass deal pipeline Priority)
- Europe Trip Budget Tracker → Project (personal, may tag to Balance domain Priority)
- S-BOS Build → Project (tags to Business self-managing systems Priority)

**What it is NOT:** A Priority (too specific, has a deliverable). A Milestone (too small, has ongoing work). A habit (not bounded).

---

### 5. STAT / TRACKED HABIT
**What it is:** A recurring data point logged over time against a Goal or Priority. Not a project — it's a measurement protocol.

**How to recognize it:**
- Happens on a repeating cadence (daily, weekly)
- Each instance is a data point, not a deliverable
- Accumulates into a trend that tells you if you're on track
- Has a Stat Menu Item (the type) and a linked Goal

**Examples from Clint's commitments:**
- Daily weight → Stat (type: Weight, linked to "198–203 lb" Goal)
- Oura readiness score → Stat (type: Readiness, linked to "Cardio age" Goal)
- Alcohol units → Stat (type: Drinks, linked to Body domain Goal)
- Rides with Max → Stat (type: Rides With Max, linked to Balance domain Goal)

**A prescribed diet or workout protocol** is NOT a project. It is a **series of Stats + a Habit Arc**:
- The protocol itself → documented as a Knowledge Library entry or checklist attached to the Goal
- Each day's execution → logged as a Stat
- The phase of the protocol → a Priority (e.g., "Cut phase," "Base training block")
- A race or event → a Milestone

---

## Domain Classification

Every record belongs to a domain. Assign it before proposing a structure.

| Domain | What lives here |
|---|---|
| **Body** | Health, fitness, weight, sleep, nutrition, training protocols |
| **Being** | Rituals, mindset, spiritual practice, personal development |
| **Balance** | Family, relationships, experiences, Kevin's Rule events |
| **Business** | Revenue, deal pipeline, team, organizational development |
| **Wealth** | Asset cash flow, liquidity, investment, legacy |

---

## How to Handle Complex Inputs — The Decomposition Protocol

When given a goal, commitment, or protocol, follow this sequence:

### Step 1 — Read the full input before doing anything
Don't decompose the first line. Read all of it. Patterns only emerge from the whole.

### Step 2 — Separate the destination from the protocol
- The destination = Goal(s)
- The time-bound focus = Priority / Phase
- The completion events = Milestones
- The repeating execution = Stats / Habit Arc
- The bounded deliverables = Projects

### Step 3 — Identify roll-up relationships
Which Projects or Stats naturally contribute to which Priorities? Don't force a parent-child. Just note the contribution tag.

### Step 4 — Flag ambiguous items
Some things could be either a Goal or a Priority, or either a Project or a Milestone. Flag them explicitly with the options rather than deciding unilaterally.

### Step 5 — Propose, don't create
Surface the proposed structure as a table or outline and ask for confirmation before any records are created. The format:

```
DOMAIN: Body

GOAL: I weigh 198–203 pounds
  PRIORITY (Q2): Cut phase — target 205 by April 30
    MILESTONE: Weigh-in at 205 — April 30
    MILESTONE: Weigh-in at 201 — June 30
    STAT (daily): Weight (lbs) — logged via Siri → Oura
    STAT (daily): Readiness score — Oura
    HABIT ARC: Cut protocol (Phase 1: 2400 cal / macro split / training schedule)

  PRIORITY (Q3–Q4): Maintain phase — hold 198–203
    STAT (daily): Weight (lbs)
    STAT (weekly): Body fat %
```

---

## Worked Examples — Clint's 2026 Commitments

### Example 1: Body Weight Goal

**Input:** "I weigh 198–203 pounds"

**Decomposition:**
```
DOMAIN: Body

GOAL: Weight 198–203 lbs (measured: Oura / Apple Health)

  PRIORITY (Q2 2026): Cut phase — reach 205 by June 30
    MILESTONE: 207 lbs by April 30
    MILESTONE: 205 lbs by June 30
    STAT: Daily weight (Apple Health → Oura)
    HABIT ARC: Cut protocol — nutrition + training (document as Knowledge entry)

  PRIORITY (Q3–Q4 2026): Maintenance phase — hold 198–203
    STAT: Daily weight
    STAT: Weekly readiness (Oura)
    MILESTONE: 201 lbs by September 30 (midpoint confirmation)
```

**Roll-up:** No Projects attached. This is pure Strategic Layer + Stat tracking.

---

### Example 2: Cardio Age Goal

**Input:** "My cardio age is 7+ years under actual — Oura"

**Decomposition:**
```
DOMAIN: Body

GOAL: Cardio age 7+ years under actual age (measured: Oura VO2 max estimate)

  PRIORITY (Q1–Q2 2026): Base training block — Zone 2 foundation
    MILESTONE: First metric century ride — May 2026
    STAT: Weekly training load (TSS — Strava)
    STAT: Oura readiness (daily)
    STAT: Oura cardio age (weekly check-in)
    HABIT ARC: Zone 2 protocol — 3x week minimum

  PRIORITY (Q3 2026): Race block — event prep
    MILESTONE: [Race/event TBD]
    STAT: TSS, readiness, cardio age
```

**Roll-up:** Any triathlon or bike events → Projects that tag to the Race Block Priority as contributors.

---

### Example 3: Business Self-Funding Goal

**Input:** "The Stitser BUILT companies are self-managing organizations, each self-funded with operating cash flow. Each company maintains a 6-month liquidity buffer on a clear budget."

**Decomposition:**
```
DOMAIN: Business

GOAL: Each SB company self-funded with operating cash flow + 6-month liquidity buffer

  PRIORITY (Q1–Q2 2026): BUILT — GP cash flow positive
    MILESTONE: Surge Flats first home closed (GP contribution)
    MILESTONE: Budget vs. actual review — Q2 close
    PROJECTS CONTRIBUTING: Surge Flats (all active lots), Cold Creek entitlement
    STAT: Monthly GP cash flow (QuickBooks → S-BOS)

  PRIORITY (Q3 2026): Establish 6-month liquidity buffer baseline
    MILESTONE: All entity budgets updated with liquidity position
    MILESTONE: CCSFT 12-month cash confirmed
    PROJECTS CONTRIBUTING: Any deal generating liquidity event

  PRIORITY (ongoing): Self-managing org install
    MILESTONE: Division Leader for Construction hired + onboarded
    MILESTONE: S-BOS fully adopted by ops team
    PROJECT CONTRIBUTING: S-BOS Build
```

**Roll-up:** Multiple Projects (Surge Flats, Cold Creek, S-BOS Build) contribute to Business Priorities. None are owned by them — they exist independently.

---

### Example 4: A Protocol Input (not a goal — needs decomposition)

**Input:** A prescribed diet and workout plan with phases, macros, and weekly schedules.

**This is NOT a single Goal or Project. Decompose it as:**

| What it contains | Record type |
|---|---|
| The outcome state (target weight, body composition) | Goal |
| The phase of the protocol (cut, base, maintain) | Priority |
| Key events or measurements (weigh-in, race, DXA scan) | Milestone |
| Daily/weekly execution (workouts, meals logged) | Stats + Habit Arc |
| The protocol document itself | Knowledge Library entry attached to Goal |

**What Claude should NOT do:**
- Create a Project called "Follow diet and workout plan" — this is not a bounded deliverable
- Create a single Goal called "Get fit" — too vague, needs a measurable end state
- Attach the whole protocol as a checklist under a Milestone — the protocol is ongoing, not binary

---

## Red Flags — When Claude Should Stop and Clarify

Stop and ask before proposing if:

- The input has no measurable success condition → ask "How will we know when this is achieved?"
- The input could be either a Goal or a Commitment statement → ask "Is this where you're going, or a rule you're operating by?"
- The input spans multiple domains → split into separate Goal records per domain, confirm
- The input describes a process with no defined end → it's a Habit Arc or Stat, not a Project or Goal
- The input is a Project that clearly contributes to something strategic but no Priority exists yet → propose the Priority first, then the Project

---

## What Claude Should Always Produce

When proposing a decomposition, always output:

1. **A labeled structure** — Domain → Goal → Priorities → Milestones → Stats/Projects, clearly indented
2. **Roll-up tags** — which Projects contribute to which Priorities (noted, not nested)
3. **Record type for each item** — so the operator knows exactly what will be created in SmartSuite
4. **Open questions** — anything ambiguous, flagged explicitly before creation
5. **A confirmation ask** — "Does this structure look right before I create any records?"

---

*Document owner: Clint Stitser / Stitser BUILT*
*Last updated: June 2026*
*Lives at: `projects/kompass/platform/shared/goal-decomposition-guide.md`*
