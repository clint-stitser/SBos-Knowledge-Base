# Cross-Doc Validation Checklist: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized (verification-mode)
> **Fill order:** Heavy-lifter consistency check between design phase and coding phase. Runs after all design docs have at least one complete draft and before the Coding Kickoff Checklist. This is the second-to-last gate before coding begins.
>
> **Pipeline position:**
> ```
> Design docs complete → THIS DOC → Coding Kickoff Checklist → Coding begins
> ```
>
> **Source docs (every check verifies against one or two of these):**
> - `[AppName]_Product_Design_Doc.md` — Core Entities, Core Features, User Workflows, Personas, Out of Scope, Technical Constraints, Success Metrics
> - `[AppName]_DB_Schema.md` — Entities, Fields, Relationships, Data Dictionary, Constraints, Indexes
> - `[AppName]_Technical_Spec.md` — API Endpoints, Auth, State Machines, Events & Side Effects, Error Handling, Performance, Dependencies
> - `[AppName]_UI_UX.md` — Screens, Navigation, Shared Components, Screen → API Map, Interactions, Error States
> - `[AppName]_UI_Strings.md` — Per-Screen strings, Global strings, Error strings, Empty states
> - `[AppName]_Sample_Data.md` — Sample rows per entity, persona coverage, workflow coverage
>
> **Downstream docs that consume this one:**
> - `[AppName]_Coding_Kickoff_Checklist.md` — Final gate before coding. Cannot pass until this checklist is signed off.
> - `[AppName]_Build_Decisions_Log.md` — When validation finds a divergence that's actually a *layering* difference rather than a conflict (per Granularity Check below), the divergence is captured here as a Reconciliation entry, not a validation failure.
> - Future projects — the Back-Feed Loop section turns this project's surprises into next project's check items.
>
> **Agent role (verification mode, not generation mode):**
> Verify cross-doc consistency by opening each source doc, reading the named sections, and walking the actual content against the check items. **The agent does NOT make design decisions here, and does NOT fix conflicts.** When a check fails, the agent reports the gap — naming the exact item, the exact docs, and the exact sections — and stops. The human resolves.
>
> **The three rules while running this validation:**
> 1. **No box checked from memory.** Always open the source doc and read the named section. Memory drifts; the file is the truth.
> 2. **If a check fails, do NOT silently move on. Do NOT attempt to fix the gap here.** Flag the specific gap: which check item, which docs, which sections, what the conflict is. The human decides what to change in which doc.
> 3. **Distinguish conflict from layering.** When two docs disagree at the *same level of detail*, that's a conflict and validation fails. When the more specific doc adds detail the less specific one doesn't have, that's normal layering and validation passes. Use the Granularity Check (below) to make this call consistently.
>
> **The dominant failure mode this doc defends against:**
> Validation that runs by skim, not by read. The agent (or the human) opens the doc once, forms an impression, then checks boxes from that impression for the next twenty items. The result is sign-off on docs that have real conflicts the agent never actually looked at. The hard rule against "from memory" exists because of this exact failure mode.
>
> **When this doc is fully filled and approved:** Remove every `🤖 AGENT INSTRUCTIONS` block and the agent-facing instruction prose inside the `🚦 GATE` block. Keep the gate checklist, the sign-off line, the findings logged during the run, and all filled snapshots. The finished doc reads clean for humans.
>
> **Cleanup verification (before declaring the doc done):**
> - Search the file for `🤖` — zero hits
> - Search the file for "Remove this block" — zero hits
> - The `🚦 GATE` block contains only its checklist and sign-off line — no agent prose
>
> **Internal fill order (loose — sections are independent):**
> 1. Granularity Check — read this first; defines what counts as a conflict
> 2. The seven pair checks (six original + UI/UX ↔ UI Strings + Schema ↔ Sample Data) — order doesn't matter, but cleaner if walked top-to-bottom
> 3. Full Pass Checks — must come after all pair checks are clean
> 4. **🚦 Gate — Validation Sign-Off** (the only gate; verification docs don't need mid-doc gates)
> 5. Sign-Off Scope snapshot — filled at sign-off
> 6. Back-Feed Loop — read-only reference; lives here permanently

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Granularity Check | ⏳ Not Started | — | Read first — defines what counts as a conflict |
| PDD ↔ Database Schema | ⏳ Not Started | — | Pair check |
| PDD ↔ Technical Spec | ⏳ Not Started | — | Pair check |
| PDD ↔ UI/UX | ⏳ Not Started | — | Pair check |
| Database Schema ↔ Technical Spec | ⏳ Not Started | — | Pair check |
| Database Schema ↔ UI/UX | ⏳ Not Started | — | Pair check |
| Technical Spec ↔ UI/UX | ⏳ Not Started | — | Pair check |
| UI/UX ↔ UI Strings | ⏳ Not Started | — | Pair check |
| Database Schema ↔ Sample Data | ⏳ Not Started | — | Pair check (includes PDD persona/workflow coverage) |
| Full Pass Checks | ⏳ Not Started | — | Run after all pair checks are clean |
| 🚦 Gate — Validation Sign-Off | ⏳ Not Started | — | Final gate before Coding Kickoff Checklist |
| Sign-Off Scope (doc state snapshot) | ⏳ Not Started | — | Captured at sign-off; updated on partial re-validation |
| Back-Feed Loop | — | — | Read-only reference, lives here permanently |

**Design Doc Status values:** ⏳ Not Started / 🔄 In Progress / ❓ Needs Discussion / ✅ Done

---

## How to Use

- **Full pass:** Say "Run cross-doc validation" when all six design docs have a complete draft. The agent walks all pair checks plus the full pass checks and reports findings in one pass.
- **Partial pass:** Say "Run cross-doc validation on what's done so far" if you want early feedback before all docs are complete. The agent runs only the pairs where both docs are at least drafted.
- **Re-validation:** If a doc is revised after sign-off, run only the pair checks involving that doc. See Sign-Off Scope rules below.
- The human reviews findings and decides what to change in which doc. The agent does not fix conflicts; it reports them.
- Update Sign-Off status when all conflicts are resolved or explicitly waived.

---

## Granularity Check

> 🤖 **AGENT INSTRUCTIONS**
>
> **Read this section before running any pair check.** This section defines what counts as a "conflict" vs. a "layering difference." Without this rule, the agent will flag normal layering as conflicts and produce a noisy validation pass that gets ignored.
>
> **Your job:**
> 1. Read the default authoritative-doc table below
> 2. Ask the human: "Does this project follow the default authoritative-doc rules, or are there exceptions?" If exceptions, capture them in the table.
> 3. Apply the rule when running every pair check below: only flag a conflict when one doc is *factually wrong relative to the authoritative one at the same level of detail*. Don't flag the more specific doc for having more detail than the less specific one — that's the system working as designed.
>
> **The reconciliation rule (important):**
> When a divergence is found that's actually layering (the more specific doc added detail the less specific one didn't have), it's NOT a validation failure. It IS a candidate for a **Reconciliation entry** in the Build Decisions Log once coding begins — to make sure the layering decision is captured and not lost. Note any such reconciliation candidates in the pair check's findings.
>
> **Remove this block before delivering the filled doc.**

> **Why this matters:** Design docs operate at different levels of detail. The Tech Spec describes service behavior; the API Contract specifies wire format. Both are correct — they describe the same thing at different resolutions. When they diverge during a check, the question isn't "which is wrong" — it's "which is authoritative for this layer." Without an explicit rule, the agent guesses.

**Default rule: the more specific doc wins at its level of detail.** State the rule below; don't assume.

| Doc pair | Authoritative when they conflict | Reasoning |
|----------|----------------------------------|-----------|
| PDD ↔ Tech Spec | Tech Spec | Tech Spec is the technical refinement |
| Tech Spec ↔ Module Breakdown | Module Breakdown | Module Breakdown is the build-level refinement |
| PDD ↔ UI/UX | UI/UX | UI/UX is the screen-level refinement |
| DB Schema ↔ Tech Spec | DB Schema (for data shape); Tech Spec (for behavior) | Different roles — Schema = data, Tech Spec = behavior |
| Tech Spec ↔ API Contract | API Contract | API Contract is the wire-level refinement |
| UI/UX ↔ UI Strings | UI Strings (for actual text); UI/UX (for screen layout) | Different roles — UI Strings = text, UI/UX = structure |
| DB Schema ↔ Sample Data | DB Schema (for field types and constraints); Sample Data is downstream | Schema is the source of truth for data shape |

**Project-specific exceptions (if any):**

> If this project's authoritative-doc rule differs from the defaults above, document the exception here before running validation. Otherwise leave blank — the defaults apply.

| Doc pair | Project-specific authoritative doc | Reasoning |
|----------|-----------------------------------|-----------|
| — | — | — |

**When a divergence is found during validation:**
- If one doc is **factually wrong** relative to the authoritative one at the same level of detail → validation **fails**. Flag the gap, name the docs and sections, stop.
- If the more specific doc **adds detail** the less specific one didn't have → validation **passes**. This is normal layering. Note it as a Reconciliation candidate to capture in Build Decisions Log during build.

---

## PDD ↔ Database Schema

> 🤖 **AGENT INSTRUCTIONS**
>
> **Open these source sections before running the checks:**
> - `[AppName]_Product_Design_Doc.md` → Core Entities, Core Features, User Workflows
> - `[AppName]_DB_Schema.md` → Entities, Relationships, Constraints, Data Dictionary
>
> **Hard rule:** Do NOT check any box without opening the source doc and reading the named section. If a check requires walking a list (e.g., "every Core Entity exists as a table"), walk the actual list — don't summarize from memory.
>
> **If a check fails:**
> - Do NOT attempt to fix the gap. The human decides which doc gets updated.
> - Report the gap with: check item ID (e.g., "Entities check 1"), the exact entity/field/relationship name that failed, the source doc + section where the gap appears, and a one-sentence summary of the conflict.
> - Move to the next check. Do not stop the validation run on a single failure.
>
> **Logging format for findings:**
> Append findings to the Findings table at the bottom of this section. Don't bury them inline. The Findings table is what the human reviews.
>
> **Reconciliation candidates:** If a difference is layering (Schema adds field-level detail not in PDD Core Entities — expected), do not flag as conflict. Note in Findings as `[Reconciliation candidate]` so it's captured for Build Decisions Log later.
>
> **Remove this block before delivering the filled doc.**

### Entities
- [ ] Every Core Entity in the PDD exists as a table in the Schema
- [ ] No tables in the Schema are missing from the PDD Core Entities (or, if present, they're justified — e.g., join tables, audit tables)
- [ ] Entity names are spelled and capitalized consistently across both docs

### Fields
- [ ] Every field referenced in PDD features or workflows exists in the Schema
- [ ] Required fields implied by core workflows (e.g., status, timestamps, ownership FKs) are present in Schema

### Relationships
- [ ] Every relationship described in the PDD (e.g., "a Project has many Tasks") matches a foreign key in the Schema
- [ ] No relationships exist in the Schema that aren't implied or explained by the PDD

### Constraints
- [ ] Every business rule in the PDD (e.g., "each user can only have one active session") is reflected as a constraint, unique index, or schema-level note
- [ ] Every NOT NULL / UNIQUE constraint in Schema is consistent with the PDD's stated rules (no schema constraint contradicts a PDD rule)

### Findings

> Logged during the validation run. One row per gap found. Empty = pair check passed clean.

| Check ID | Item / name | Conflict summary | Reconciliation candidate? | Resolved? |
|----------|-------------|------------------|--------------------------|-----------|
| — | — | — | — | — |

---

## PDD ↔ Technical Spec

> 🤖 **AGENT INSTRUCTIONS**
>
> **Open these source sections before running the checks:**
> - `[AppName]_Product_Design_Doc.md` → Core Features, User Workflows, Personas, Technical Constraints, Success Metrics, Out of Scope
> - `[AppName]_Technical_Spec.md` → API Endpoints, Authentication & Authorization, Performance Considerations, Dependencies & Integrations, Events & Side Effects
>
> **Hard rule:** Open the docs. Walk the lists. No checks from memory.
>
> **Common failure patterns to look for:**
> - A PDD Feature with no endpoint in Tech Spec — almost always a real gap, not layering
> - A Tech Spec endpoint with no corresponding PDD Feature — usually a layering or scope-creep question; flag for human review
> - PDD persona not present in Tech Spec roles table — real gap
> - PDD performance constraint with no Tech Spec implementation — real gap (Tech Spec must show *how* the constraint is met)
> - PDD integration mentioned but no Tech Spec Dependencies row — real gap
>
> **Reconciliation candidates:** Tech Spec listing endpoints PDD didn't explicitly call out (e.g., utility endpoints for the UI's needs) is often layering, not a conflict. Note these for review.
>
> **Remove this block before delivering the filled doc.**

### Features → API Coverage
- [ ] Every Core Feature in the PDD has at least one corresponding API endpoint in the Tech Spec
- [ ] Every Tech Spec endpoint maps to at least one PDD Feature, or is justified as supporting infrastructure (auth, health checks, etc.)

### Users / Personas → Auth Model
- [ ] Every persona / user role in the PDD is accounted for in the Tech Spec's Roles and Permissions table
- [ ] Permission levels in Tech Spec match what the PDD says each role can do — no extra capabilities, no missing capabilities

### Workflows → Endpoints
- [ ] Every user workflow step in the PDD that involves a system action (save, submit, approve, etc.) maps to a specific endpoint
- [ ] Workflow error paths in the PDD are accounted for in Tech Spec endpoint error scenarios

### Non-Functional Requirements
- [ ] Performance targets in the PDD (if any) are reflected in Tech Spec Performance Considerations with a specific approach
- [ ] Scale assumptions in the PDD match Tech Spec infrastructure choices

### Integrations
- [ ] Every third-party service mentioned in the PDD is addressed in Tech Spec Dependencies & Integrations
- [ ] No Tech Spec integration is unexplained by the PDD (or justified as infrastructure)

### Compliance & Security
- [ ] Every compliance requirement in the PDD (HIPAA, GDPR, etc.) has a corresponding row in Tech Spec Security Considerations
- [ ] PII-handling requirements in the PDD are reflected in Tech Spec security and auth

### Findings

| Check ID | Item / name | Conflict summary | Reconciliation candidate? | Resolved? |
|----------|-------------|------------------|--------------------------|-----------|
| — | — | — | — | — |

---

## PDD ↔ UI/UX

> 🤖 **AGENT INSTRUCTIONS**
>
> **Open these source sections before running the checks:**
> - `[AppName]_Product_Design_Doc.md` → Core Features, User Workflows, Personas, Out of Scope
> - `[AppName]_UI_UX.md` → Screens, Navigation / Information Architecture, User Interactions
>
> **Hard rule:** Walk every Core Feature in PDD against the UI/UX Screens list. Walk every Screen in UI/UX against the PDD Features. Do not approximate.
>
> **Common failure patterns:**
> - PDD Feature with no screen in UI/UX (typically a real gap unless feature is fully backend — flag the question)
> - UI/UX Screen with no PDD Feature (often scope creep — surface for human decision)
> - PDD persona that has no navigation entry-point in UI/UX (real gap — the role can't access the app)
> - Terminology drift (PDD says "Project," UI/UX says "Workspace" — must reconcile in one direction)
>
> **Remove this block before delivering the filled doc.**

### Features → Screens
- [ ] Every user-facing feature in the PDD has at least one screen in the UI/UX doc
- [ ] No screens exist in the UI/UX doc that aren't tied to a PDD Feature or supporting workflow (auth, profile, etc. are valid)

### Personas → Navigation
- [ ] Every persona in the PDD has a defined entry point into the app per UI/UX Navigation
- [ ] Role-restricted features are reflected as gated UI elements in UI/UX (not just backend rules)
- [ ] No persona is referenced in UI/UX without appearing in PDD

### Workflows → Flows
- [ ] Every user workflow in the PDD has a corresponding flow or interaction sequence in UI/UX
- [ ] Flow steps match the PDD workflow steps — same sequence, same outcomes
- [ ] Workflow error paths in the PDD are represented as error states / screens in UI/UX

### Terminology
- [ ] Labels, button text, section names in UI/UX use the same terms as the PDD
- [ ] No synonyms used interchangeably (e.g., "Project" vs "Case" vs "Matter")

### Findings

| Check ID | Item / name | Conflict summary | Reconciliation candidate? | Resolved? |
|----------|-------------|------------------|--------------------------|-----------|
| — | — | — | — | — |

---

## Database Schema ↔ Technical Spec

> 🤖 **AGENT INSTRUCTIONS**
>
> **Open these source sections before running the checks:**
> - `[AppName]_DB_Schema.md` → Entities, Relationships, Data Dictionary, Constraints, Indexes
> - `[AppName]_Technical_Spec.md` → API Endpoints (request/response field references), State Machines, Performance Considerations (indexing strategy)
>
> **Hard rule:** For every endpoint with a request body or response field, verify the fields exist in Schema's Data Dictionary. Don't skim — for endpoints with many fields, walk each one.
>
> **Common failure patterns:**
> - Tech Spec endpoint references a field that's not in Schema (real gap — either Schema is missing the field or Tech Spec invented it)
> - Schema has a status enum but Tech Spec has no State Machine for that entity (Tech Spec gap — flag as red flag per Tech Spec's own Gate 2)
> - Tech Spec lists an index requirement but Schema's Indexes section doesn't include it (real gap)
> - State Machine transition references a field that doesn't exist in Schema
>
> **Remove this block before delivering the filled doc.**

### Data Model → API Contracts
- [ ] Every API endpoint that reads or writes data references only fields that exist in Schema
- [ ] No API endpoint creates or returns data that has no corresponding table/field in Schema
- [ ] Field types in Tech Spec request/response shapes are compatible with Schema field types

### State Machines → Schema
- [ ] Every entity with a status enum in Schema has a State Machine in Tech Spec
- [ ] Every state value in Schema's status enum appears in the corresponding Tech Spec State Machine
- [ ] Every State Machine transition references only fields that exist in Schema

### Migrations & Schema Evolution
- [ ] Any schema decisions noted in Tech Spec (soft deletes, audit logs, optimistic locking) are reflected in Schema
- [ ] Migration strategy in Tech Spec is compatible with Schema structure

### Indexes & Performance
- [ ] Fields used as filters/sorts in Tech Spec API queries have indexes in Schema (or are documented as accepting full-table scans)
- [ ] Indexes in Schema are explained by either a Tech Spec performance need or a foreign key relationship

### Findings

| Check ID | Item / name | Conflict summary | Reconciliation candidate? | Resolved? |
|----------|-------------|------------------|--------------------------|-----------|
| — | — | — | — | — |

---

## Database Schema ↔ UI/UX

> 🤖 **AGENT INSTRUCTIONS**
>
> **Open these source sections before running the checks:**
> - `[AppName]_DB_Schema.md` → Entities, Data Dictionary, Constraints
> - `[AppName]_UI_UX.md` → Screens (specifically the field-level details of forms, tables, detail views)
>
> **Hard rule:** For every form in UI/UX, walk every input field against Schema. For every data display (table, detail view), walk every column/value against Schema.
>
> **Common failure patterns:**
> - UI form has a field with no Schema column (real gap — either the field is computed/derived, or Schema is missing the column)
> - Schema has a NOT NULL field that's not in the UI's create form (real gap — user can't fill it)
> - UI uses a date picker for a field Schema defines as TIMESTAMP (compatible but worth confirming the time component is handled)
> - UI displays a field Schema marks as sensitive/PII without a corresponding masking note
>
> **Remove this block before delivering the filled doc.**

### Display Fields
- [ ] Every field displayed in UI/UX (labels, table columns, detail views) exists in Schema (or is documented as computed/derived)
- [ ] No UI element displays data that has no field in Schema

### Form Fields → Schema Fields
- [ ] Every input field in a UI/UX form maps to a Schema field
- [ ] Required form fields match NOT NULL constraints in Schema (no UI optional field for a NOT NULL column without a default)
- [ ] Field input types are compatible with Schema types (date picker → DATE/TIMESTAMP, number input → INTEGER/DECIMAL, etc.)

### Lists & Filters
- [ ] Every filterable column in a UI list has a corresponding indexed field in Schema (cross-check with Tech Spec performance section)
- [ ] Every sortable column in a UI list has a corresponding indexed field or is documented as small-table sort

### Sensitive Data
- [ ] Any Schema field marked as PII / sensitive is either not displayed in UI/UX or has a documented masking/redaction approach

### Findings

| Check ID | Item / name | Conflict summary | Reconciliation candidate? | Resolved? |
|----------|-------------|------------------|--------------------------|-----------|
| — | — | — | — | — |

---

## Technical Spec ↔ UI/UX

> 🤖 **AGENT INSTRUCTIONS**
>
> **Open these source sections before running the checks:**
> - `[AppName]_Technical_Spec.md` → API Endpoints, Authentication & Authorization, Error Handling, Events & Side Effects
> - `[AppName]_UI_UX.md` → Screen → API Map, User Interactions / State Management, Screens (error states, loading states)
>
> **Hard rule:** The UI/UX Screen → API Map is the cross-reference doc — walk every row in it against Tech Spec API Endpoints. Then walk Tech Spec endpoints in reverse — is every endpoint connected to at least one UI screen (or justified as backend-only)?
>
> **Common failure patterns:**
> - UI screen needs data but no Tech Spec endpoint returns it (real gap)
> - Tech Spec endpoint exists but no UI consumes it (often layering — flag for review)
> - UI has a gated element with no backend permission enforcement (security gap)
> - UI shows error state for an error Tech Spec never returns, or doesn't handle an error Tech Spec does return
> - Async UI behavior (loading, optimistic updates) not aligned with Tech Spec response patterns
>
> **Remove this block before delivering the filled doc.**

### API ↔ Screen Data Needs
- [ ] Every screen in UI/UX that loads data has a corresponding API endpoint that returns that data (per Screen → API Map)
- [ ] Every form submission in UI/UX has a corresponding write endpoint (POST/PATCH/PUT) in Tech Spec
- [ ] Every Tech Spec endpoint is consumed by at least one UI/UX screen, or is justified as backend-to-backend / supporting infrastructure

### Auth ↔ UI Access Control
- [ ] Every gated UI element (hidden button, locked screen) corresponds to a permission enforced by Tech Spec auth model
- [ ] No UI access control exists without a backend enforcement counterpart (UI-only gates are security holes)

### Error States
- [ ] Every Tech Spec error code has a corresponding UI error state or message in UI/UX
- [ ] No UI error state exists for an error Tech Spec never returns
- [ ] Validation errors are surfaced field-by-field in UI/UX matching Tech Spec's per-field `details` array

### Loading / Async / Real-Time
- [ ] Every async API call has a corresponding loading state in UI/UX
- [ ] Optimistic updates (if any) are documented in both Tech Spec and UI/UX
- [ ] WebSocket / real-time events from Tech Spec Events & Side Effects have corresponding UI handling

### Findings

| Check ID | Item / name | Conflict summary | Reconciliation candidate? | Resolved? |
|----------|-------------|------------------|--------------------------|-----------|
| — | — | — | — | — |

---

## UI/UX ↔ UI Strings

> 🤖 **AGENT INSTRUCTIONS**
>
> **Open these source sections before running the checks:**
> - `[AppName]_UI_UX.md` → Screens (every screen with its UI elements), Shared Component Inventory
> - `[AppName]_UI_Strings.md` → Per-Screen string sections, Global strings, Error strings, Empty states
>
> **Hard rule:** Every screen in UI/UX should have a corresponding per-screen section in UI Strings. Walk both directions:
> 1. For every screen in UI/UX, verify a Per-Screen section exists in UI Strings.
> 2. For every Per-Screen section in UI Strings, verify the screen exists in UI/UX.
>
> Then walk individual strings: for every text element referenced in UI/UX (button label, heading, helper text, error message, empty state copy), verify the actual string exists in UI Strings.
>
> **Common failure patterns:**
> - UI/UX references a button or label that's not in UI Strings (gap — either UI Strings is incomplete or UI/UX is over-specifying text)
> - UI Strings has strings for a screen that's not in UI/UX (orphan strings — either dead, or UI/UX is missing the screen)
> - UI/UX shows error states but UI Strings has no Error strings section, or vice versa
> - Terminology drift between UI/UX descriptions and UI Strings values (e.g., UI/UX says "Save" button, UI Strings says "Submit")
> - Shared Component in UI/UX has strings that aren't centralized in UI Strings Global strings (component-level strings should be reusable)
>
> **Reconciliation candidates:** UI Strings is the authoritative source for actual text per the Granularity Check. If UI/UX has placeholder text that doesn't match UI Strings' actual text, that's not a conflict — UI Strings wins. Note as Reconciliation if the divergence is meaningful.
>
> **Remove this block before delivering the filled doc.**

### Screen Coverage
- [ ] Every screen in UI/UX has a corresponding Per-Screen section in UI Strings
- [ ] No Per-Screen section in UI Strings refers to a screen that doesn't exist in UI/UX

### Element Coverage
- [ ] Every button, heading, label, helper text, and empty-state copy referenced in UI/UX has an actual string in UI Strings
- [ ] No string exists in UI Strings without a corresponding UI element in UI/UX

### Error & Empty States
- [ ] Every error state referenced in UI/UX has a corresponding error string in UI Strings (or a documented use of a Global error string)
- [ ] Every empty state in UI/UX has a corresponding empty-state string in UI Strings

### Shared Components
- [ ] Strings used by Shared Components in UI/UX are centralized in UI Strings Global section (not duplicated per screen)
- [ ] Component-level strings are consistent across every screen that uses the component

### Terminology Consistency
- [ ] Terminology used in UI/UX descriptions matches the actual text in UI Strings (or the divergence is intentional and documented)
- [ ] No two strings in UI Strings use different terms for the same concept

### Findings

| Check ID | Item / name | Conflict summary | Reconciliation candidate? | Resolved? |
|----------|-------------|------------------|--------------------------|-----------|
| — | — | — | — | — |

---

## Database Schema ↔ Sample Data

> 🤖 **AGENT INSTRUCTIONS**
>
> **Open these source sections before running the checks:**
> - `[AppName]_DB_Schema.md` → Entities, Data Dictionary, Constraints, Relationships
> - `[AppName]_Sample_Data.md` → Sample rows per entity, persona coverage, workflow coverage
> - `[AppName]_Product_Design_Doc.md` → Personas, User Workflows (for the persona/workflow coverage subsection)
>
> **Hard rule:** For every entity in Schema, verify Sample Data has at least one sample row. For every sample row, verify every field is type-compatible with Schema's Data Dictionary and respects every constraint (NOT NULL, UNIQUE, enum values).
>
> **Common failure patterns:**
> - Entity in Schema with no sample rows in Sample Data (gap — coding tests will have nothing to seed with)
> - Sample row references an entity not in Schema (typo or stale Sample Data)
> - Sample row violates a Schema constraint (NOT NULL field empty, enum value not in allowed list, FK pointing to nonexistent row)
> - Sample Data doesn't cover every persona from PDD (gap — tests can't exercise role-specific behavior)
> - Sample Data doesn't exercise the workflows from PDD (gap — demos and tests can't walk realistic paths)
> - Field types mismatch (e.g., Sample Data has `"2025-01-15"` for a TIMESTAMP field that needs a time component)
>
> **Remove this block before delivering the filled doc.**

### Entity Coverage
- [ ] Every entity in Schema has at least one sample row in Sample Data
- [ ] Every sample row's table reference matches an entity in Schema (no orphan sample rows)

### Field & Type Compatibility
- [ ] Every field in every sample row maps to a field in Schema's Data Dictionary
- [ ] Sample values are compatible with Schema types (strings within max length, dates in the correct format, enums in the allowed value list, integers in range)
- [ ] No sample row has a field not in Schema (no invented fields)

### Constraints Respected
- [ ] No sample row violates a NOT NULL constraint
- [ ] No sample row violates a UNIQUE constraint within the sample set
- [ ] Every FK in a sample row points to an existing sample row (no broken references in the sample set)
- [ ] Every enum-valued field uses a value from the allowed list

### Persona Coverage (PDD cross-check)
- [ ] Sample Data includes at least one row per persona from PDD (e.g., if PDD has "Provider" and "Patient" personas, Sample Data has at least one user of each)
- [ ] Persona roles in Sample Data match the role values defined in Schema / Tech Spec auth model

### Workflow Coverage (PDD cross-check)
- [ ] Sample Data is rich enough to exercise the PDD User Workflows end-to-end (e.g., if a workflow requires a Project with Tasks, Sample Data has both)
- [ ] Sample Data includes records in every status / state from Schema enums (e.g., if Order has `pending / shipped / delivered`, Sample Data has examples of each)

### Findings

| Check ID | Item / name | Conflict summary | Reconciliation candidate? | Resolved? |
|----------|-------------|------------------|--------------------------|-----------|
| — | — | — | — | — |

---

## Full Pass Checks

> 🤖 **AGENT INSTRUCTIONS**
>
> **Run only after all seven pair checks above are clean** (every pair check's Findings table is empty, or every finding has been resolved or explicitly waived).
>
> **Open these source sections before running:**
> - All six design docs — the full pass walks across all of them, not pair-by-pair
>
> **Hard rule:** The Terminology Consistency check is the most important and the easiest to half-do. Pick five core entities/concepts from the PDD and trace each one through all six docs. If any of them is referred to by a different name in any doc, flag it.
>
> **Remove this block before delivering the filled doc.**

### Terminology Consistency
- [ ] All six design docs use the same names for the same things (entities, roles, features, actions, statuses)
- [ ] No doc introduces a term not defined in PDD (or defined in another design doc with PDD's blessing)
- [ ] Pick five core terms from PDD; trace each through all six docs — every occurrence uses the same term

### No Orphaned Decisions
- [ ] Every architectural decision in Tech Spec traces back to a requirement in PDD (or is supporting infrastructure)
- [ ] Every UI pattern in UI/UX traces back to a feature or workflow in PDD
- [ ] Every Schema constraint traces back to a business rule in PDD or a technical requirement in Tech Spec
- [ ] Every UI string in UI Strings traces back to a UI element in UI/UX

### Compliance & Regulatory (if applicable)
- [ ] Every regulatory requirement in PDD (HIPAA, GDPR, SOC2, etc.) is addressed in at least one of: Tech Spec Security, Schema constraints, UI/UX (consent flows, masking)
- [ ] No compliance requirement is mentioned in only one doc — it appears wherever it's enforced

### Out-of-Scope Verification
- [ ] No design doc specifies behavior for something the PDD lists as Out of Scope
- [ ] If any doc *does* include something out of scope, it's flagged for PDD scope update

### Open Questions
- [ ] No unresolved ❓ Needs Discussion sections remain in any of the six docs
- [ ] Every Open Questions item in every doc has either been answered or explicitly deferred with rationale

### Findings

| Check ID | Item / name | Conflict summary | Reconciliation candidate? | Resolved? |
|----------|-------------|------------------|--------------------------|-----------|
| — | — | — | — | — |

---

## 🚦 Gate — Validation Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not declare validation complete until the human explicitly signs off. This is the only gate in this doc — verification work is mostly mechanical, so mid-doc gates would be overkill.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off. The Coding Kickoff Checklist (the next and final gate before coding) explicitly checks that this validation has passed — there's no way to start coding without this gate signed off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**

*Pair-check completeness*
- [ ] PDD ↔ Database Schema pair check is complete, Findings table is empty or every finding has been resolved/waived
- [ ] PDD ↔ Technical Spec pair check is complete and clean
- [ ] PDD ↔ UI/UX pair check is complete and clean
- [ ] Database Schema ↔ Technical Spec pair check is complete and clean
- [ ] Database Schema ↔ UI/UX pair check is complete and clean
- [ ] Technical Spec ↔ UI/UX pair check is complete and clean
- [ ] UI/UX ↔ UI Strings pair check is complete and clean
- [ ] Database Schema ↔ Sample Data pair check is complete and clean (including persona and workflow coverage)

*Full pass completeness*
- [ ] Full Pass Checks section is complete, Findings table is empty or every finding has been resolved/waived
- [ ] Terminology Consistency check has been done by tracing five core terms across all six docs (not done by skim)
- [ ] No unresolved ❓ Needs Discussion sections remain in any of the six docs

*Reconciliations captured*
- [ ] Every finding marked as `[Reconciliation candidate]` has been noted for capture in Build Decisions Log during the build phase (the agent does not create BD entries here, but the human or build-phase agent will)

*Sign-off scope*
- [ ] Doc State Snapshot table below is filled in with current state of each of the six docs (date or git commit SHA)

**Sign-off:**
> 🚦 **Validation Sign-Off** — All design docs are internally consistent and cross-validated. Conflicts are resolved. Reconciliation candidates are captured for build. Design phase is locked.
>
> **Human sign-off:** ☐ Approved — proceed to Coding Kickoff Checklist (the final gate before coding begins)

---

## Sign-Off Scope (when validation expires)

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Fill in the Doc State Snapshot table at sign-off time. Use either the file modification date (Filesystem MCP can read it) or the git commit SHA (ask the human). Update on every partial re-validation.
>
> **The agent enforces the partial re-validation rule:** If the human asks for re-validation after a doc has been edited, run only the pair checks involving the edited doc. Do not re-run pair checks on docs that haven't changed since the last sign-off — that's wasted work.
>
> **Material vs. cosmetic — the agent's rule:** Typo fixes, formatting changes, and prose clarifications that don't change content are cosmetic — no re-validation needed. Adding a Feature, Entity, Endpoint, Screen, or Constraint is material — re-validate. Removing a Feature, Entity, Endpoint, Screen, or Constraint is material — re-validate. If unsure, treat as material and re-validate.
>
> **Remove this block before delivering the filled doc.**

> **Sign-off is not permanent.** It's bound to the docs *as they were at sign-off*. If any of the six design docs are revised after sign-off, validation is invalidated **for that doc only** — partial re-validation is required, not a full re-run.

**Rules:**

1. **Record the doc states at sign-off.** When the human signs off, fill in the table below with the file modification date or git commit SHA of each doc. That snapshot is what the sign-off covers.
2. **A revision invalidates validation for that doc only.** If `[AppName]_Technical_Spec.md` is edited after sign-off, only the pair checks involving Tech Spec re-run. The other five docs stay signed off.
3. **Partial re-validation walks only the affected pair-check sections** — mark them re-validated with the new doc state and update the snapshot table.
4. **Material vs. cosmetic edits.** Typo fixes don't invalidate. Adding a new feature, removing an entity, changing a constraint, or moving a workflow does. If unsure, treat as material.

### Doc State Snapshot at Sign-Off

> Fill in when sign-off is granted. Update on every re-validation.

| Doc | Sign-off date | File state (date / SHA) | Last re-validated |
|-----|---------------|-------------------------|-------------------|
| `[AppName]_Product_Design_Doc.md` | — | — | — |
| `[AppName]_DB_Schema.md` | — | — | — |
| `[AppName]_Technical_Spec.md` | — | — | — |
| `[AppName]_UI_UX.md` | — | — | — |
| `[AppName]_UI_Strings.md` | — | — | — |
| `[AppName]_Sample_Data.md` | — | — | — |

---

## Back-Feed Loop (cross-project improvement)

> **This section is permanent reference — no 🤖 instructions, no edits per project. It documents how this template improves itself across projects.**

**This template is a consumer of the back-feed loop, not its home.** The loop *runs* during Project Closeout: at end of project, the Project Closeout doc harvests every **Reconciliation** entry from the Build Decisions Log and asks "does this Cross-Doc Validation template need a new check item to catch this class of conflict next time?"

What that means in practice:

- **During this project:** when validation finds a conflict that the existing checks didn't anticipate, log it as a Reconciliation in the Build Decisions Log during build. Don't try to update this template mid-project.
- **At Project Closeout:** review the Reconciliation entries. Each one is a candidate for a new check item in this template. The Project Closeout doc proposes the deltas; the human approves; the deltas land in the source-of-truth template via a Template Update Worklog.
- **For the next project:** this template starts the new project with the new checks already in it. The first project teaches the second.

This is what makes the validation checklist self-improving across projects without requiring out-of-band retro work.
