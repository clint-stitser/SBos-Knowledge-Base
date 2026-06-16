# Product Design Doc: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized
> **Fill order:** This is the first design doc filled. Everything else flows from it.
>
> **Agent role:** Scribe, depth-adder, and consistency enforcer. The human is the designer — all design decisions belong to them. Nothing is written here that the human did not say or confirm.
>
> **The three rules while filling this doc:**
> 1. Everything written traces back to something the human said. No invented design decisions.
> 2. If something is unclear, stop and ask — do not proceed on a guess.
> 3. Output must be precise enough that the next agent in the pipeline (Schema, Tech Spec, UI/UX) can work from it without guessing.
>
> **When this doc is fully filled and approved:** Remove every `🤖 AGENT INSTRUCTIONS` block, every `❓ AGENT PAUSE` prompt, and the agent-facing instruction prose inside `🚦 GATE` blocks. Keep the gate checklists, sign-off lines, and all filled content. The finished doc reads clean for humans — no agent scaffolding visible.
>
> **Cleanup verification (before declaring the doc done):**
> - Search the file for `🤖` — zero hits
> - Search the file for `❓ AGENT PAUSE` — zero hits
> - Search the file for "Remove this block" — zero hits
> - Every `🚦 GATE` block contains only its checklist and sign-off line — no agent prose

---

## Status & Next Steps

| Section | Status | Notes |
|---------|--------|-------|
| Problem Statement | ⏳ Not Started | |
| Target Users / Personas | ⏳ Not Started | |
| 🚦 Gate 1 | ⏳ Not Started | |
| Core Entities | ⏳ Not Started | |
| 🚦 Gate 2 | ⏳ Not Started | |
| Core Features | ⏳ Not Started | |
| User Workflows | ⏳ Not Started | |
| 🚦 Gate 3 | ⏳ Not Started | |
| Out of Scope | ⏳ Not Started | |
| Success Metrics | ⏳ Not Started | |
| Technical Constraints / Assumptions | ⏳ Not Started | |
| Timeline / Phases | ⏳ Not Started | |
| Open Questions / Decisions Needed | ⏳ Not Started | |
| 🚦 Gate 4 | ⏳ Not Started | |

**Status scheme:** ⏳ Not Started → 🔄 In Progress → ❓ Needs Discussion → ✅ Done

---

## 1. Problem Statement

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Translate the human's description of the problem into a precise, unambiguous statement that tells downstream agents *what* is broken, *who* it's broken for, and *why solving it matters*. A vague problem statement produces vague features. Fix ambiguity here or everything downstream inherits it.
>
> **A complete Problem Statement covers:**
> - The specific pain or gap that exists today (not a solution — the actual problem)
> - Who experiences it (name the user type, not a demographic abstraction)
> - The consequence of not solving it (what does the user lose, miss, or suffer?)
> - The current workaround, if one exists (what are they doing instead, and why is it insufficient?)
>
> **Incomplete looks like:**
> - "Users need a better way to manage X" — better than what? what's wrong with now?
> - "There's no good solution for X" — what do they do today? why is it bad?
> - A solution described as a problem — "We need an app that does X" is a solution, not a problem
>
> **Ask triggers — stop and ask the human before writing if:**
> - The human described a solution without stating a problem
> - The current workaround is unknown
> - The consequence of not solving is unclear
> - Multiple disconnected problems are described — which is the primary one?
>
> **❓ AGENT PAUSE format:**
> > ❓ **[short label]** — [one sentence stating what's unclear]. Needed to write [specific thing blocked].
>
> **Remove this block before delivering the filled doc.**

**Problem:**
[What is broken, missing, or painful today?]

**Affected users:**
[Who experiences this problem? Be specific — job role, context, behavior, not demographic.]

**Consequence:**
[What does the user lose, miss, or have to do as a result?]

**Current workaround:**
[What do they do today instead? Why is it insufficient?]

---

## 2. Target Users / Personas

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define each user type precisely enough that the Features and Workflows agent knows whose needs drive each decision — and the UI/UX agent knows who they're designing for. Vague personas produce features that try to serve everyone and satisfy no one.
>
> **A complete persona covers:**
> - Name (a label, not a real person) and role
> - Their primary goal when using this app — one sentence, specific
> - Their most common action in the app
> - Their biggest frustration or constraint (time, skill level, environment, device, etc.)
> - Whether they are a primary or secondary user (primary = app designed around their needs; secondary = supported but not the design center)
>
> **Incomplete looks like:**
> - "Admin users who manage the system" — what do they actually do? what's their goal?
> - A list of roles with no goal or context — roles are not personas
> - "Power users" with no definition of what power means in this app
>
> **Ask triggers — stop and ask the human before writing if:**
> - A role is named but its goals and constraints are unknown
> - It's unclear which persona is primary
> - Two personas seem to have conflicting needs — which one wins?
> - A persona's technical comfort level is unknown and it would affect design
>
> **Remove this block before delivering the filled doc.**

### Persona: [Name / Role]

- **Type:** Primary / Secondary
- **Primary goal:** [One sentence — what are they trying to accomplish when they open this app?]
- **Most common action:** [The thing they do most often]
- **Key constraint or frustration:** [What slows them down, frustrates them, or limits them?]
- **Technical comfort:** [Low / Medium / High — and what that means in context]

*(Add additional Persona blocks as needed)*

---

## 🚦 Gate 1 — Problem + Users Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] Problem Statement describes a problem, not a solution
- [ ] Current workaround is documented
- [ ] Consequence of not solving is documented
- [ ] All named user types have a goal, a primary action, and a key constraint
- [ ] Primary persona is identified
- [ ] No two personas have directly conflicting needs without a resolution noted

**Sign-off:**
> 🚦 **Gate 1** — Problem Statement and Target Users are complete and internally consistent. Ready to define Core Entities.
>
> **Human sign-off:** ☐ Approved — proceed to Core Entities

---

## 3. Core Entities

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define every major "thing" in the system with enough precision that the Schema agent can derive fields, types, and relationships without guessing. This is the most upstream section — ambiguity here propagates into every downstream doc.
>
> **A complete entity definition covers:**
> - Entity name (singular, PascalCase — this name is used across all docs)
> - What it represents in one sentence
> - Its primary identifier (how the system uniquely knows it)
> - Its core attributes — the minimum fields needed to describe it meaningfully (not an exhaustive field list — that's the Schema doc — but enough that the Schema agent knows what this entity is)
> - Its key relationships: which other entities does it belong to, contain, or reference?
> - Its lifecycle: does it get created, modified, and deleted? Or is it append-only? Are there states?
>
> **Incomplete looks like:**
> - "User — a person who uses the app" — what fields? what relationships? what lifecycle?
> - An entity listed without relationships — almost nothing in a real app is isolated
> - States mentioned (active, inactive, pending) without a lifecycle described
> - Attributes listed as "various fields TBD" — that's not a definition, that's a placeholder
>
> **Ask triggers — stop and ask the human before writing if:**
> - An entity is named in the Problem or Features but not defined here
> - Two entities seem to describe the same thing — are they one entity or two?
> - A relationship direction is ambiguous (does A belong to B, or does B belong to A?)
> - An entity has status values but the lifecycle isn't described
> - It's unclear whether an entity is created by the user or by the system
>
> **Remove this block before delivering the filled doc.**

### Entity: [EntityName]

- **What it is:** [One sentence]
- **Identified by:** [Primary key / unique identifier]
- **Core attributes:** [Minimum fields to describe it — name, type, brief description. Not exhaustive.]
- **Relationships:**
  - belongs to: [EntityName] (M:1)
  - has many: [EntityName] (1:M)
  - references: [EntityName]
- **Lifecycle:** [Created by whom/what → modified when → deleted or archived? Any status states?]

*(Add additional Entity blocks as needed)*

---

## 🚦 Gate 2 — Core Entities Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] Every entity mentioned in Problem Statement and Target Users is defined here
- [ ] Every entity has a primary identifier
- [ ] Every entity has at least the minimum core attributes documented
- [ ] All relationships are documented with direction and cardinality
- [ ] No two entities appear to describe the same thing without explanation
- [ ] Every entity with status values has a lifecycle described

**Sign-off:**
> 🚦 **Gate 2** — Core Entities are complete and internally consistent. Ready to define Features and Workflows.
>
> **Human sign-off:** ☐ Approved — proceed to Core Features

---

## 4. Core Features

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define each feature with enough precision that the Tech Spec agent can derive API endpoints, state machines, and business logic rules without guessing — and the UI/UX agent can derive screens and interactions without guessing.
>
> **A complete feature definition covers:**
> - Feature name and one-sentence description
> - The user(s) who use it (reference personas from Section 2)
> - The entities it creates, reads, updates, or deletes (reference entities from Section 3)
> - The core business rules that govern it — what is allowed, what is not, what triggers what
> - The expected outcome when it succeeds
> - A failure modes table: what can go wrong, and what happens when it does
>
> **Incomplete looks like:**
> - "Users can manage their profile" — manage means what? create? edit? delete? what fields?
> - Business rules listed as "standard validation" — not specific enough for a coding agent
> - No failure modes — every feature can fail; if none are listed, the agent invented them or skipped them
> - Outcomes described only as "user sees success message" — what state changed? what was persisted?
>
> **Ask triggers — stop and ask the human before writing if:**
> - A feature is described but the business rules are unknown
> - It's unclear which entities are created, modified, or deleted by the feature
> - A failure mode is obvious (auth failure, network error, constraint violation) but resolution is unspecified
> - Two features appear to overlap — are they one feature or two?
> - Permission rules are missing — who can use this feature, and who can't?
>
> **Remove this block before delivering the filled doc.**

### Feature: [Feature Name]

**Description:** [One sentence — what does this feature do?]

**Used by:** [Persona name(s)]

**Entities affected:**
| Entity | Operation | Notes |
|--------|-----------|-------|
| [EntityName] | Create / Read / Update / Delete | [Any constraint or rule] |

**Business rules:**
- [Rule 1 — specific and unambiguous. Not "validate input" — state what valid means.]
- [Rule 2]
- [Rule N]

**Success outcome:** [What state exists in the system after this feature executes successfully?]

**Failure modes:**
| Failure condition | Trigger | System response | User resolution |
|-------------------|---------|-----------------|-----------------|
| [e.g., Duplicate entry] | [e.g., User submits name that already exists] | [e.g., Return 409, no record created] | [e.g., Show inline error: "Name already in use"] |
| [e.g., Unauthorized] | [e.g., User without permission attempts action] | [e.g., Return 403] | [e.g., Redirect to login or show access denied] |

*(Add additional Feature blocks as needed)*

---

## 5. User Workflows

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Document every significant user journey with enough precision that the UI/UX agent knows exactly what screens, states, and decision points to design — including every error path. A workflow that only documents the happy path is incomplete. Ambiguous steps produce screens the designer has to invent.
>
> **A complete workflow covers:**
> - Workflow name and the persona who executes it
> - Preconditions: what must be true before this workflow can start?
> - Steps: numbered, each step is a single discrete action or decision
> - Decision points: where does the path branch? What causes each branch?
> - Happy path outcome: what state exists when the workflow completes successfully?
> - Error paths: documented as a table (see format below) — one row per failure condition
>
> **Incomplete looks like:**
> - Steps like "system processes the request" — what does the system do? what changes?
> - No preconditions — every workflow has them, even if they're just "user is logged in"
> - A workflow with no error paths — even a two-step workflow can fail
> - Decision points described in prose instead of branched steps — the UI/UX agent can't derive screens from prose branching
>
> **Ask triggers — stop and ask the human before writing if:**
> - A step says "system does X" but how isn't described and it matters for UI or data
> - A decision point exists but what triggers each branch is unclear
> - An error path is obvious (permission denied, record not found, validation failure) but resolution is unspecified
> - The preconditions are unknown
> - A workflow references a feature not listed in Section 4
>
> **Remove this block before delivering the filled doc.**

### Workflow: [Workflow Name]

**Persona:** [Who executes this?]

**Preconditions:**
- [What must be true before this workflow starts?]

**Steps:**
1. [Step — one discrete action or system event]
2. [Step]
3. [Decision point: if [condition] → go to step X; else → go to step Y]
4. [Step]
...

**Happy path outcome:** [What state exists in the system when this completes successfully?]

**Error paths:**
| Condition | Trigger | System response | User resolution |
|-----------|---------|-----------------|-----------------|
| [e.g., Session expired] | [e.g., User submits form after timeout] | [e.g., Redirect to login, preserve form state if possible] | [e.g., Log in again, return to form] |
| [e.g., Required field missing] | [e.g., User submits with blank required field] | [e.g., Inline validation, no submission] | [e.g., Fill required field] |

*(Add additional Workflow blocks as needed)*

---

## 🚦 Gate 3 — Features + Workflows Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] Every feature references only entities defined in Section 3
- [ ] Every feature references only personas defined in Section 2
- [ ] Every feature has at least one failure mode documented
- [ ] Every workflow has preconditions documented
- [ ] Every workflow has at least one error path documented
- [ ] No workflow step says "system does X" without X being specific enough to derive code from
- [ ] All decision points have explicit branch conditions
- [ ] Features and Workflows are consistent with each other — no workflow references a feature not listed, no feature exists with no workflow that uses it

**Sign-off:**
> 🚦 **Gate 3** — Core Features and User Workflows are complete and internally consistent. Ready to define scope, constraints, metrics, and timeline.
>
> **Human sign-off:** ☐ Approved — proceed to Out of Scope

---

## 6. Out of Scope

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Document what this app explicitly does NOT do. This section protects downstream agents from building things that were never intended, and protects the human from scope creep that starts with "well, it's almost there already."
>
> **A complete Out of Scope section covers:**
> - Features or capabilities that a reasonable person might expect this app to have, but it won't
> - Integrations or external systems that won't be connected (at least in this phase)
> - User types that won't be supported
> - Data or content that won't be stored or processed
> - For each exclusion: a brief reason (deferred to later phase, out of product vision, explicitly decided against)
>
> **Incomplete looks like:**
> - An empty section or "TBD" — if nothing is out of scope, that itself is a decision worth stating
> - A list with no rationale — "not building X" without a reason invites challenge later
> - Items that overlap with features in Section 4 — if it's in scope, remove it from here
>
> **Ask triggers — stop and ask the human before writing if:**
> - A capability is obviously adjacent to a listed feature but inclusion/exclusion is unstated
> - It's unclear whether something is deferred (later phase) vs. never (out of product vision)
> - An integration was mentioned in passing but never confirmed as in or out
>
> **Remove this block before delivering the filled doc.**

| Out of scope item | Reason / disposition |
|-------------------|----------------------|
| [Feature or capability] | [Deferred to Phase N / Out of product vision / Explicit decision: why] |

---

## 7. Success Metrics

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define how the human will know this product worked. Metrics without measurement methods are wishes. This section feeds the Analytics & Instrumentation section of the Tech Spec — if a metric can't be measured by the system, flag it now.
>
> **A complete Success Metrics section covers:**
> - The specific metric (what is being measured)
> - The target value or threshold (what "success" looks like — not "improve" but "X% or better")
> - How it will be measured (what event, query, or instrumentation produces this number)
> - The timeframe for measurement (at launch? 30 days post-launch? ongoing?)
>
> **Incomplete looks like:**
> - "Improve user retention" — by how much? measured how? over what period?
> - "Users find value" — unmeasurable as stated
> - A metric with no measurement method — if you can't measure it, you can't verify it
> - A metric that requires data the system doesn't collect — flag this or it will be missed
>
> **Ask triggers — stop and ask the human before writing if:**
> - A metric is described qualitatively with no quantitative threshold
> - How a metric will be measured is unknown
> - A metric depends on data the system may not collect
> - It's unclear what the baseline is (can't measure improvement without a starting point)
>
> **Remove this block before delivering the filled doc.**

| Metric | Target | Measurement method | Timeframe |
|--------|--------|--------------------|-----------|
| [e.g., User activation rate] | [e.g., ≥ 60% of signups complete core action within 7 days] | [e.g., Event: `core_action_completed` logged per user] | [e.g., First 30 days post-launch] |

---

## 8. Technical Constraints / Assumptions

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Document every constraint or assumption that will shape technical decisions downstream. These feed directly into the Tech Spec. A constraint that isn't written here will either be missed or re-derived incorrectly by the Tech Spec agent.
>
> **A complete Technical Constraints section covers:**
> - Platform requirements (web, mobile, desktop, specific OS, browser versions)
> - Performance requirements (load time, response time, concurrent users, data volume)
> - Integration requirements (specific third-party systems that must be connected)
> - Security or compliance requirements (HIPAA, SOC2, GDPR, etc.)
> - Infrastructure constraints (must use X cloud provider, must fit in Y budget, existing systems to work with)
> - Known assumptions (things being treated as true that haven't been verified — flag these explicitly)
>
> **Incomplete looks like:**
> - "Standard security practices" — not specific enough for the Tech Spec agent to work from
> - A compliance requirement named without its implications stated (e.g., "HIPAA" with no note about what that means for data storage and access)
> - Assumptions listed as facts — if it's an assumption, label it as one
> - Missing performance targets — "fast" is not a constraint
>
> **Ask triggers — stop and ask the human before writing if:**
> - A compliance requirement is named but its implications for this app are unclear
> - A performance requirement is described qualitatively ("fast", "scalable") without a number
> - An integration is implied by a feature but not confirmed
> - It's unclear whether something is a hard constraint or a preference
>
> **Remove this block before delivering the filled doc.**

**Constraints:**
| Constraint | Specifics | Hard or soft? |
|------------|-----------|---------------|
| [e.g., Platform] | [e.g., Web only — no native mobile app in Phase 1] | Hard |
| [e.g., Compliance] | [e.g., HIPAA — PHI must be encrypted at rest and in transit; access logging required] | Hard |
| [e.g., Performance] | [e.g., Page load ≤ 2s on 4G; API response ≤ 500ms at p95] | Hard |

**Assumptions:**
| Assumption | Implication if wrong |
|------------|---------------------|
| [e.g., Users have reliable internet access] | [e.g., Offline mode would be required — significant scope increase] |

---

## 9. Timeline / Phases

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Document the delivery plan with enough precision that the Module Breakdown agent can derive build order and phase boundaries. Vague timelines ("Phase 1: MVP, Phase 2: more features") don't give the coding agents anything to work from.
>
> **A complete Timeline section covers:**
> - Phase name and number
> - What ships in this phase — reference features from Section 4 by name
> - What is explicitly deferred to a later phase
> - Target date or duration (even a rough one — "~6 weeks" is better than nothing)
> - The success criterion for the phase (what does "done" mean for this phase?)
>
> **Incomplete looks like:**
> - "Phase 1: MVP" with no feature list — the coding agent doesn't know what MVP means
> - A timeline with no dates or durations — even rough estimates help scope the build
> - Features deferred to "later" without a phase assigned — unanchored deferrals become lost features
> - No success criterion — when does the phase close?
>
> **Ask triggers — stop and ask the human before writing if:**
> - A phase's feature list is undefined
> - It's unclear whether a feature is Phase 1 or deferred
> - No target date or duration is known even roughly
> - The success criterion for a phase is undefined
>
> **Remove this block before delivering the filled doc.**

### Phase [N]: [Phase Name]

- **Target:** [Date or duration]
- **Features shipping:**
  - [Feature name from Section 4]
  - [Feature name]
- **Explicitly deferred:** [Feature or capability, and which phase it moves to]
- **Done when:** [Specific success criterion for this phase]

*(Add additional Phase blocks as needed)*

---

## 10. Open Questions / Decisions Needed

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Surface every unresolved question that could affect design decisions in downstream docs. An open question left undocumented becomes an assumption. An assumption built into the Schema or Tech Spec without the human's knowledge is a rework risk.
>
> **A complete Open Questions section covers:**
> - The question, stated precisely
> - Which section or downstream doc it blocks (if any)
> - The current working assumption, if the team is proceeding on one
> - Who needs to answer it
>
> **Incomplete looks like:**
> - Questions listed without noting what they block — the coding agent doesn't know which ones are urgent
> - "TBD" entries with no working assumption — the downstream agent has nothing to work from and will guess
> - A question that was actually resolved during the session but not removed from here
>
> **Ask triggers — stop and ask the human before writing if:**
> - A question was raised and then answered in the session but not captured — confirm the answer before writing it
> - A question is blocking a Gate checklist item — it must be resolved before sign-off, not deferred here
>
> **Remove this block before delivering the filled doc.**

| # | Question | Blocks | Working assumption | Owner |
|---|----------|--------|--------------------|-------|
| Q-01 | [Precise question] | [Section or downstream doc, or "nothing yet"] | [What we're assuming until answered, or "none"] | [Human / TBD] |

---

## 🚦 Gate 4 — Full PDD Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off. This is the final gate before the Schema, Tech Spec, and UI/UX agents begin.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] All sections are complete (no ⏳ Not Started or 🔄 In Progress in the Status table)
- [ ] Gates 1, 2, and 3 are all signed off
- [ ] Out of Scope entries all have rationale
- [ ] Every Success Metric has a measurement method
- [ ] Every constraint is marked Hard or Soft
- [ ] Every assumption has an implication documented
- [ ] Every Phase has a feature list and a done-when criterion
- [ ] Open Questions are documented with working assumptions — nothing is silently unresolved
- [ ] No entity, feature, persona, or workflow is referenced in one section but missing from its source section
- [ ] No two sections contradict each other

**Sign-off:**
> 🚦 **Gate 4** — PDD is complete, internally consistent, and ready to feed downstream docs. DB Schema, Technical Spec, and UI/UX Design may now begin.
>
> **Human sign-off:** ☐ Approved — PDD complete. Proceed to downstream design docs.
