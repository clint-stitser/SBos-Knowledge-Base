# The Six Intelligence Stores

Beyond the project hierarchy, every Kompass Brain contains six intelligence stores that the LLM draws from when engaged on any decision or task.

## 1. People Profiles

**What:** Team capacity, counterparty track record, relationship context, communication style, risk tolerance

**When the LLM pulls it:** Any task involving a person by name

**Structure:** 7-section model
- Identity and role
- Relationship history and origin
- Communication style and preferences
- Track record and performance
- Risk tolerance and decision filters
- Current status and open items
- Notes and signals

## 2. Vendor Ratings

**What:** Performance history by job, quality scores, on-time rate, category tags, key contacts

**When the LLM pulls it:** Any task involving vendor selection, bid evaluation, or invoice review

**Key fields:** Vendor name, category, license/bond status, contact, jobs worked, on-time %, budget adherence %, quality score, issues log, would-use-again flag

## 3. Decision Matrices

**What:** Pre-built go/no-go criteria for each recurring decision type — deal, hire, vendor, exit

**When the LLM pulls it:** Any task framed as a decision or evaluation

**Decision types by product:**
- Developer: deal go/no-go, partner selection, exit criteria, lender selection
- Contractor: bid go/no-go, subcontractor selection, change order approval, hire criteria
- Agent: listing go/no-go, offer acceptance criteria, client acceptance criteria
- Personal: commitment go/no-go, purchase decisions, relationship boundaries

## 4. Decision Log

**What:** What was decided, why, who was involved, what happened — filterable by project, domain, outcome

**When the LLM pulls it:** Any task where precedent matters — "what did we decide last time?"

**Record structure:**
- Decision date and project
- Decision type (from matrix taxonomy)
- Question / options considered
- Decision made and rationale
- Who was involved
- Outcome (populated later)
- Pattern tags

## 5. Knowledge Library

**What:** Domain reference — "what does this mean and why does it matter" for each vertical

**When the LLM pulls it:** Any task where domain expertise is required

**Entry structure:**
- Concept definition (plain language)
- Why it matters (operational implication)
- Key decision points
- Common mistakes
- Related concepts
- Tools and resources

See each product folder for the domain-specific knowledge library structure.

## 6. Project Data

**What:** Active project status, budget, schedule, GYR health, open issues

**When the LLM pulls it:** Any task scoped to a specific project

**Sourced from:** SmartSuite project records, Smartsheet schedule, QuickBooks budget actuals
