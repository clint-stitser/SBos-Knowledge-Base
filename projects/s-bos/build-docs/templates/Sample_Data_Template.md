# Sample Data: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized (light pass)
> **Fill order:** After DB Schema has all core entities defined; before any tests or demos reference fixtures. Filled in parallel with UI Strings (both feed off completed design docs).
>
> **Source docs:**
> - `[AppName]_DB_Schema.md` — every field in every sample row must trace to a real Schema field. Constraints (NOT NULL, UNIQUE, enum values) must hold across the sample set.
> - `[AppName]_Product_Design_Doc.md` — Personas and User Workflows. Sample Data must have at least one row per persona, and must be rich enough to exercise every PDD workflow end-to-end.
>
> **Downstream docs that consume this one:**
> - `[AppName]_Cross_Doc_Validation_Checklist.md` — the Schema ↔ Sample Data pair-check verifies every entity has sample rows, every field is type-compatible, and persona/workflow coverage is complete.
> - `[AppName]_Testing_Strategy.md` — Test Data & Fixtures section references sample rows by stable ID.
> - **Demo / screenshot / design discussion** consumers — when someone says "use a real example," they reach for this file.
>
> **Agent role:** Translate Schema entities into a small, realistic, internally consistent set of sample rows. The agent does NOT invent fields not in Schema, does NOT violate Schema constraints, and does NOT exceed the row-count rule (~5-10 per major entity) because the file is reference data, not a fixture dump.
>
> **The three rules while filling this doc:**
> 1. **Every field traces to Schema.** No invented fields. Field types match Schema types. Constraints hold.
> 2. **Realistic, not Lorem Ipsum.** Real-looking names, dates within the past year, real-looking text in description fields. "Test User 1" / "Test User 2" is wrong; "Sarah Chen" / "Marcus Holloway" is right.
> 3. **Persona and workflow coverage are required.** Every PDD persona has at least one user. Every PDD workflow can run end-to-end on this data (a workflow requiring a Project with Tasks needs both in the sample set; an Order workflow with `pending`/`shipped`/`delivered` needs an example of each).
>
> **Common failures:**
> - A sample row with a field not in Schema (typo or stale data)
> - A row missing a NOT NULL field (Schema constraint violated)
> - FK that doesn't resolve to any existing row in the sample set (broken reference)
> - All users are the same persona (PDD has Provider + Patient personas; Sample Data only has Providers)
> - Workflow can't run end-to-end on the data (workflow needs `delivered` Order but no `delivered` rows exist)
>
> **Ask triggers:**
> - PDD has 3+ personas and the sample set would balloon past 10 rows per entity to cover them all — ask Ryan whether to relax the row-count rule or whether some personas can share rows
> - A Schema enum has 10+ values and exhaustive coverage would dominate the sample set — ask which subset to prioritize
> - A Schema entity is referenced by every workflow (e.g., User) but the sample User count is small — ask whether to add more or whether the workflow exercises only the same User
>
> **When this doc is fully filled and approved:** Remove every `🤖 AGENT INSTRUCTIONS` block. Keep all sample rows; this doc is read continuously through build and demos.

**Purpose:** A small set of canonical, realistic data rows that demos, tests, screenshots, and design discussions all share. When someone says "use a real example," they reach for this file.

**When to fill:** After the DB Schema Doc has all core entities defined. Before any tests are written.

**Update:** When entities change shape (new required field, removed field, renamed). Sample data should always be a valid instance of the current schema.

**Rule:** 5–10 rows per major entity. Realistic enough that nothing feels like Lorem Ipsum. Not so much that the file becomes a database dump.

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Entity Tables | ⏳ Not Started | — | One table per major entity |
| Cross-Entity Scenarios | ⏳ Not Started | — | Optional — narrative connecting the rows |
| Test/Demo Usage Notes | ⏳ Not Started | — | Where this data shows up |

---

## How to Fill This In

1. **Pick the entities that matter for demos and tests.** Not every entity needs sample data — focus on the ones that show up in screens, reports, or test fixtures. Lookup tables (categories, statuses) are usually exhaustive elsewhere; skip them here.
2. **5–10 rows per entity.** Enough to show variety. Stop before it becomes a fixture file.
3. **Use realistic values.** Real-looking names, real-looking dates within the past year, real-looking text in description fields. Not "Test User 1" / "Test User 2."
4. **Reference the DB Schema for field names and types.** Every field in a sample row should exist in the Schema. Every required field must be present.
5. **Use stable IDs.** Pick IDs (e.g., `usr_001` to `usr_010`) and reuse them across entities so foreign-key relationships are easy to read. Don't randomize.
6. **Note relationships explicitly.** If `task_004` belongs to `project_002`, show that in the data. The point is for someone to read across tables and understand how things connect.

---

## Entity: [Entity Name 1]

> Reference: `[AppName]_DB_Schema.md` § [Section]

| ID | [Field 1] | [Field 2] | [Field 3] | [Field 4] | [Field 5] |
|----|-----------|-----------|-----------|-----------|-----------|
| [id_001] | [value] | [value] | [value] | [value] | [value] |
| [id_002] | [value] | [value] | [value] | [value] | [value] |
| [id_003] | [value] | [value] | [value] | [value] | [value] |
| [id_004] | [value] | [value] | [value] | [value] | [value] |
| [id_005] | [value] | [value] | [value] | [value] | [value] |

> Notes on this set: [Anything to call out — e.g., "id_003 is intentionally missing the email field to test optional handling."]

---

## Entity: [Entity Name 2]

> Reference: `[AppName]_DB_Schema.md` § [Section]

| ID | [FK Field] | [Field 2] | [Field 3] | [Field 4] |
|----|------------|-----------|-----------|-----------|
| [id_001] | [FK to Entity 1] | [value] | [value] | [value] |
| [id_002] | [FK to Entity 1] | [value] | [value] | [value] |
| [id_003] | [FK to Entity 1] | [value] | [value] | [value] |

> Notes on this set: [—]

---

*(Repeat for each major entity. Remove this template comment when filled in.)*

---

## Cross-Entity Scenarios (optional)

> Short narratives that walk through the data showing how entities relate. Useful when a single row of one table doesn't tell the whole story. Skip this section for simple data models.

### Scenario 1: [Scenario Name]

[2–4 sentences walking through several rows. Example: "User `usr_002` (Sarah) created Project `proj_001` (Q1 Launch) on Jan 5. She added three tasks: `task_001`, `task_002`, `task_003`. Task `task_002` is overdue, which the dashboard should surface."]

### Scenario 2: [Scenario Name]

[Walk-through.]

---

## Test/Demo Usage Notes

> Where does this data show up, so it's easy to keep things consistent.

- **Unit tests:** [Reference path or fixture file location, if seeded into test DB]
- **Integration tests:** [—]
- **Demo seed script:** [If a script loads this into a dev DB on first run, name it]
- **Screenshots:** [If this data is what shows up in marketing/docs screenshots, note that]
- **Design mockups:** [If UI/UX mockups use these names, note that]

---

## What This File Is NOT

- Not a complete fixture file — fixture files might have hundreds of rows; this has a handful
- Not test data for edge cases — those live in test files near the tests that use them
- Not production seed data — production seeding follows its own rules in the Database Migration Checklist
- Not a substitute for the DB Schema — schema is the source of truth for field names, types, and constraints
