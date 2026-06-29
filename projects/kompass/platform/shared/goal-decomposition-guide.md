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

## The Third Pattern — Recurring Cadence (Department Mini-Sprints)

Some work is neither a one-time project nor a strategic goal. It is a **recurring rhythm** — a department or function that produces the same unit of work on a predictable schedule, indefinitely.

**The mental model:** The accounting department doesn't have one project. It has **12 projects a year** — one per month — each with the same structure, cloned from a template, completed and archived, then started again. Plus a handful of special projects (annual taxes, quarterly projections) that fall outside the monthly cadence but belong to the same department.

**This is the Recurring Cadence pattern.**

### Structure

```
DEPARTMENT / FUNCTION  (Category in S-BOS)
  └── Annual Cadence
        ├── Recurring Project × N  (monthly, weekly, quarterly — cloned from template)
        │     └── Checklist · Schedule · People · Budget
        └── Special Project × N  (one-off instances that belong to this department)
              └── Checklist · Schedule · People · Budget
```

### How to Recognize It

A function follows the Recurring Cadence pattern when:
- The same type of work repeats on a fixed schedule
- Each instance has a defined start, end, and completion checklist
- Success means completing the instance — not arriving at a destination
- The department isn't pursuing a Goal — it's **maintaining a standard**

### Key Distinction — Cadence vs. Goal

| Recurring Cadence | Strategic Goal |
|---|---|
| Accounting closes the books every month | Accounting closes within 10 business days by Q4 |
| The function exists to maintain a standard | The Goal exists to raise the standard |
| Projects are cloned from a template | Projects are scoped individually |
| Done = checklist complete | Done = measurable outcome achieved |
| No horizon — runs indefinitely | Has a horizon — arrives somewhere |

**The department is not pursuing a Goal. It is executing a rhythm.**

If you want to *improve* the rhythm — that's when a Goal and Priority enter the picture (see Improvement Layer below).

### Examples of Recurring Cadence Functions

| Department / Function | Recurring Project Cadence | Special Projects |
|---|---|---|
| Accounting | 12 monthly closes | Annual taxes, Q1/Q3 tax projections, audit prep |
| Payroll | 26 biweekly runs | Year-end W-2s, new hire setup |
| Pay App (Construction) | 1 per billing period per job | Lien releases, final billing |
| Email Triage | Weekly sweep | Unsubscribe batch, inbox zero sprint |
| Marketing | Weekly content publish | Campaign launch, brand refresh |
| HR / People | Monthly 1-on-1 cycle | Annual reviews, comp adjustments |
| Compliance (Brokerage) | Per-transaction audit | Annual license renewals, E&O renewal |

### How to Structure a Recurring Cadence in S-BOS

**Step 1 — Name the Department/Function** (this becomes the Category tag)

**Step 2 — Define the recurring project template**
Every instance of the recurring project should have the same core structure:
- Standard checklist (the steps that must be completed every time)
- Schedule (when it opens, when it's due)
- People (who owns it, who reviews it)
- Budget (cost of this instance — e.g., accounting bill for the month)

**Step 3 — Identify special projects**
What falls outside the regular cadence but belongs to this department? Name them separately. They are not recurring — they are one-off Projects that share the department Category.

**Step 4 — Clone, don't recreate**
Each new instance of the recurring project is cloned from the template. Completed instances are archived. The template itself never gets archived.

### Worked Example — Accounting Department

**Input:** "Accounting runs monthly closes, quarterly tax projections, and an annual tax filing."

**Decomposition:**
```
CATEGORY: Accounting

RECURRING PROJECT TEMPLATE: Monthly Close
  Checklist:
    □ All transactions reconciled
    □ Bank statements matched
    □ P&L reviewed and approved
    □ Budget vs. actual updated
    □ Management report distributed
  Schedule: Opens 1st of month, due by Day 15
  People: Lisa (owner), Clint (review/approval)
  Budget: $X/month (accounting firm fee)

RECURRING INSTANCES (clone from template):
  January Close → February Close → March Close → ... → December Close

SPECIAL PROJECTS (same category, not recurring):
  Q1 Tax Projection (March)
  Q3 Tax Projection (September)
  Annual Tax Filing (April deadline)
  Annual Audit Prep (if applicable)
```

**Roll-up:** Monthly Close projects contribute to any Business Goal that tracks financial health or self-funding metrics. No parent-child — the closes exist whether or not a Goal is attached.

---

### The Improvement Layer — When You Want to Get Better at the Cadence

The Recurring Cadence runs the department. The Improvement Layer raises the standard.

When an operator wants to improve a recurring function — close faster, reduce errors, cut cost — that improvement initiative becomes a **Goal + Priority**, and the recurring Projects become **contributors** to the improvement Priority.

```
STRATEGIC LAYER (improvement initiative)
GOAL: Accounting closes within 10 business days by Q4 2026
  PRIORITY (Q2): Reduce close from Day 15 to Day 12
    MILESTONE: First Day-12 close — April
    PROJECTS CONTRIBUTING: April Close, May Close, June Close
    STAT: Close day (logged per month)

  PRIORITY (Q3): Reduce close from Day 12 to Day 10
    MILESTONE: First Day-10 close — September
    PROJECTS CONTRIBUTING: July Close, August Close, September Close
    STAT: Close day

OPERATIONAL LAYER (cadence continues unchanged)
Monthly Close × 12  (still runs every month regardless of improvement Goal)
```

The cadence doesn't stop or change because there's an improvement Goal. It continues. The Goal simply watches the cadence and measures whether it's getting better.

---

### How Claude Should Handle Recurring Cadence Inputs

**Signals to look for:**
- "We do X every month / week / quarter"
- "Department X runs Y on a regular basis"
- "X has N projects a year" (Clint's framing)
- Any process described as ongoing, repeating, and standard

**What to propose:**
1. Name the Department/Function (Category)
2. Identify the recurring project template and its cadence
3. List the special projects that belong to the same department
4. Ask: "Is there an improvement goal attached to this, or is this just the standard cadence?"
   - If yes → layer in the Strategic Goal + Priority structure
   - If no → the cadence stands alone, no Goal needed

**What NOT to do:**
- Don't create a Goal called "Run accounting" — that's a cadence, not a destination
- Don't create a single Project called "Accounting 2026" — it's 12+ projects
- Don't attach recurring projects as children of a Goal unless there's an explicit improvement initiative

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

**What it is NOT:** A thing you do. A process. A habit. A recurring cadence.

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

**What it is NOT:** A goal (too big, no time box). A project (not a thing to produce). A recurring cadence (not improvement-oriented, just maintenance).

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
- Produces something (a closed deal, a filed permit, a completed month-end close)

**Examples:**
- Surge Flats Lot 7 — Construction → Project (tags to Q1 GP Priority as contributor)
- January Close → Project (cloned from Accounting template, tags to financial health Goal if one exists)
- Cold Creek Entitlement → Project
- Annual Tax Filing → Special Project under Accounting category

**What it is NOT:** A Priority. A Milestone. A habit. A Goal.

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
- Monthly close day → Stat (type: Close Day, linked to "10-day close" Goal)

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

For business functions, the domain is Business and the **Category** is the department name (Accounting, Payroll, Construction Ops, Compliance, etc.).

---

## How to Handle Complex Inputs — The Decomposition Protocol

When given a goal, commitment, or protocol, follow this sequence:

### Step 1 — Read the full input before doing anything
Don't decompose the first line. Read all of it. Patterns only emerge from the whole.

### Step 2 — Identify the pattern first
Before assigning record types, ask: is this Strategic, Operational, or a Recurring Cadence?

- **Strategic:** Describes a destination. Has a horizon. Success is measured.
- **Operational:** Describes a thing to produce. Bounded. Done when done.
- **Recurring Cadence:** Describes a repeating rhythm. Maintains a standard. Runs indefinitely.

### Step 3 — Separate the destination from the protocol
- The destination = Goal(s)
- The time-bound focus = Priority / Phase
- The completion events = Milestones
- The repeating execution = Stats / Habit Arc / Recurring Projects
- The bounded deliverables = Projects

### Step 4 — Identify roll-up relationships
Which Projects or Stats naturally contribute to which Priorities? Don't force a parent-child. Just note the contribution tag.

### Step 5 — Flag ambiguous items
Some things could be either a Goal or a Priority, or either a Project or a Milestone, or either a Recurring Cadence or a one-off Project. Flag them explicitly with the options rather than deciding unilaterally.

### Step 6 — Propose, don't create
Surface the proposed structure and ask for confirmation before any records are created.

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

**Roll-up:** No Projects attached. Pure Strategic Layer + Stat tracking.

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

---

### Example 4: Accounting Department (Recurring Cadence)

**Input:** "Accounting has 12 monthly closes per year, plus quarterly tax projections and an annual tax filing."

**Pattern identified:** Recurring Cadence — not a Goal, not a one-off Project.

**Decomposition:**
```
CATEGORY: Accounting

RECURRING PROJECT TEMPLATE: Monthly Close
  Checklist: Reconcile → Match statements → Review P&L → Update BvA → Distribute report
  Schedule: Opens 1st, due Day 15
  People: Lisa (owner), Clint (approval)
  Budget: Monthly accounting fee

INSTANCES (cloned from template):
  Jan Close · Feb Close · Mar Close · Apr Close · May Close · Jun Close
  Jul Close · Aug Close · Sep Close · Oct Close · Nov Close · Dec Close

SPECIAL PROJECTS (same Category, not recurring):
  Q1 Tax Projection (March)
  Q3 Tax Projection (September)
  Annual Tax Filing (April)

IMPROVEMENT LAYER (only if a Goal exists):
  GOAL: Close within 10 business days by Q4 2026
    PRIORITY (Q2): Reduce to Day 12
      MILESTONE: First Day-12 close — April
      STAT: Close day (monthly)
      PROJECTS CONTRIBUTING: Apr Close, May Close, Jun Close
```

---

### Example 5: A Protocol Input (not a goal — needs decomposition)

**Input:** A prescribed diet and workout plan with phases, macros, and weekly schedules.

| What it contains | Record type |
|---|---|
| The outcome state (target weight, body composition) | Goal |
| The phase of the protocol (cut, base, maintain) | Priority |
| Key events or measurements (weigh-in, race, DXA scan) | Milestone |
| Daily/weekly execution (workouts, meals logged) | Stats + Habit Arc |
| The protocol document itself | Knowledge Library entry attached to Goal |

---

## Red Flags — When Claude Should Stop and Clarify

- No measurable success condition → ask "How will we know when this is achieved?"
- Could be a Goal or a Commitment statement → ask "Is this where you're going, or a rule you're operating by?"
- Spans multiple domains → split into separate records per domain, confirm
- Describes a process with no defined end → likely Recurring Cadence or Habit Arc, not a Project or Goal
- Recurring Cadence input with no improvement intent → don't add a Goal; just build the template
- Recurring Cadence input with improvement intent → build the template AND the Goal + Priority layer
- A Project clearly contributes to something strategic but no Priority exists yet → propose the Priority first

---

## What Claude Should Always Produce

1. **Pattern identification first** — Strategic / Operational / Recurring Cadence
2. **A labeled structure** — Domain → pattern-appropriate hierarchy, clearly indented
3. **Roll-up tags** — which Projects contribute to which Priorities (noted, not nested)
4. **Record type for each item** — so the operator knows what will be created in SmartSuite
5. **Open questions** — anything ambiguous, flagged explicitly
6. **A confirmation ask** — "Does this structure look right before I create any records?"

---

*Document owner: Clint Stitser / Stitser BUILT*
*Last updated: June 2026*
*Lives at: `projects/kompass/platform/shared/goal-decomposition-guide.md`*
