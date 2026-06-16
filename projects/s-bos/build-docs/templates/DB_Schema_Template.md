# Database Schema Doc: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized
> **Fill order:** Second design doc. Depends on the PDD. Begin only after PDD Gate 2 (Core Entities) is signed off — the PDD's Core Entities section is the upstream input for this doc.
>
> **Source doc:** `[AppName]_Product_Design_Doc.md` — specifically the Core Entities, Core Features, and User Workflows sections.
>
> **Agent role:** Translate the PDD's Core Entities into a precise, machine-executable schema. The human is the designer; the agent is the scribe and depth-adder. Every field, type, constraint, and relationship must be defendable against either (a) something stated in the PDD, or (b) a question the agent paused to ask the human.
>
> **The three rules while filling this doc:**
> 1. Everything written traces back to either the PDD or a confirmed answer from the human. No invented entities, no invented fields, no guessed types.
> 2. If the PDD is silent on something this doc needs (e.g., field type, max length, nullability, cascade behavior), stop and ask the human — do not assume. The PDD intentionally stays at a higher level; this doc fills the gap.
> 3. Output must be precise enough that the API Contract agent and the Database Migration Checklist agent can work from it without guessing — every field has a type, every relationship has a direction and on-delete behavior, every status enum has its values listed.
>
> **PDD-to-Schema mapping (verify before each section):**
> - Every entity in PDD Core Entities → one entity block here
> - Every PDD Core Entity attribute → at minimum one field here (often more, with explicit types)
> - Every PDD relationship → one row in the Relationships section
> - Every PDD lifecycle / status → status field in the entity + states tied to the Tech Spec State Machines section
> - Every entity referenced in PDD Features or Workflows → must exist here. If it's referenced but not in PDD Core Entities, that's a PDD gap — flag it and go back to the human.
>
> **When this doc is fully filled and approved:** Remove every `🤖 AGENT INSTRUCTIONS` block, every `❓ AGENT PAUSE` prompt, and the agent-facing instruction prose inside `🚦 GATE` blocks. Keep the gate checklists, sign-off lines, and all filled content. The finished doc reads clean for humans.
>
> **Cleanup verification (before declaring the doc done):**
> - Search the file for `🤖` — zero hits
> - Search the file for `❓ AGENT PAUSE` — zero hits
> - Search the file for "Remove this block" — zero hits
> - Every `🚦 GATE` block contains only its checklist and sign-off line — no agent prose

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Overview | ⏳ Not Started | — | — |
| Entity Relationship Diagram | ⏳ Not Started | — | — |
| Entities | ⏳ Not Started | — | — |
| 🚦 Gate 1 — Entities Complete | ⏳ Not Started | — | — |
| Relationships | ⏳ Not Started | — | — |
| 🚦 Gate 2 — Relationships Complete | ⏳ Not Started | — | — |
| Data Dictionary | ⏳ Not Started | — | — |
| Data Types Reference | ⏳ Not Started | — | — |
| Migration Notes | ⏳ Not Started | — | — |
| Query Patterns | ⏳ Not Started | — | — |
| Constraints / Validation | ⏳ Not Started | — | — |
| 🚦 Gate 3 — Full Schema Sign-Off | ⏳ Not Started | — | — |

**Status scheme:** ⏳ Not Started → 🔄 In Progress → ❓ Needs Discussion → ✅ Done

---

## Overview

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Anchor this schema to the PDD. Anyone reading this doc should immediately understand which app this models and where it comes from.
>
> **A complete Overview covers:**
> - What real-world domain the schema models (one sentence — pulled from PDD Problem Statement)
> - The core entities (the list from PDD Core Entities, names only)
> - The 2–3 most important relationships in plain language
> - Explicit reference to the source PDD
>
> **Ask triggers:**
> - PDD doesn't have a clear domain statement → ask the human before writing one here
>
> **Remove this block before delivering the filled doc.**

**What it models:** [What real-world domain this schema represents — pulled from PDD Problem Statement]
**Core entities:** [List the entities from PDD Core Entities, one line each]
**Key relationships:** [How the major entities connect — 2–3 sentences max]
**Source of truth:** Flows from `[AppName]_Product_Design_Doc.md` § Core Entities

---

## Entity Relationship Diagram (ERD)

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Produce a text-based ERD that any reader can scan in 10 seconds to understand entity connections. This is the visual anchor for the rest of the doc.
>
> **A complete ERD covers:**
> - Every entity that has its own block in the Entities section
> - Every relationship from the Relationships section (must match 1:1 — no extras here, no extras there)
> - Junction tables for many-to-many relationships shown explicitly
>
> **Incomplete looks like:**
> - An entity in the Entities section that doesn't appear in the ERD (or vice versa)
> - Relationship direction shown ambiguously (use `<` to point at the "many" side every time)
> - Missing junction tables on M:M relationships
>
> **Ask triggers:**
> - The PDD describes a relationship in prose but doesn't make cardinality explicit → ask the human before guessing 1:M vs M:M
>
> **Remove this block before delivering the filled doc.**

**Notation key:**
```
──    one-to-one
──<   one-to-many  (the < points to the "many" side)
>──<  many-to-many (use a junction table)
```

**Example:**
```
User ──< Project ──< Task

User ──< Comment

Project >──< Tag   (junction: ProjectTag)
```

[Replace example above with the actual ERD for this app.]

---

## Entities

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define every entity from the PDD Core Entities section, plus any entities discovered while filling out the schema that the PDD missed (flag these as gaps).
>
> **A complete Entity block covers:**
> - **Soft delete decision** stated explicitly. Yes (with `deleted_at`) or No (hard delete). Never "TBD."
> - **Mutable fields** named explicitly. Which fields can be changed after creation? This drives the API Contract's PATCH endpoints.
> - **Every field** with: name, type (from Data Types Reference), nullable, default, Set By (System / User / Either), and constraints. If a field is an enum, list every valid value.
> - **Indexes** beyond PK/FK with one-line reason for each. PK index is implicit — don't list it.
> - **Lifecycle notes** if the entity has a status field or any non-trivial creation/update rules.
>
> **Incomplete looks like:**
> - A field with type "TBD" or "string" with no max length
> - A field listed without nullability (Nullable column is empty)
> - An enum field without its values listed in either the field table or the Notes
> - An index without a one-line reason — "improves performance" is not a reason; "lookups by user_id are the most common query" is
> - "Various fields" or "etc." in place of an actual field list
>
> **Ask triggers — stop and ask the human if:**
> - The PDD names a field but doesn't constrain it enough to pick a type (e.g., "name" — VARCHAR(255)? TEXT? what's the actual max length?)
> - The PDD lists a status enum but doesn't list every legal value
> - It's unclear whether the entity should support soft delete or hard delete
> - A field is implied by a feature but not in the PDD (e.g., Feature says "send the user a receipt" but PDD doesn't mention an email field on User)
> - You're tempted to add a field "in case it's needed" — don't. Ask first.
>
> **Pre-fill checklist for each entity (cross-reference the PDD):**
> - [ ] Entity name matches the PDD exactly (singular, PascalCase)
> - [ ] Every PDD attribute for this entity is represented as one or more fields here
> - [ ] If PDD says "X has many Y" or "X belongs to Y", the FK is included in the appropriate entity
> - [ ] If PDD says the entity has lifecycle states, status field is present and enum values listed
>
> **Remove this block before delivering the filled doc.**

### Entity 1: [Name]

> **Soft delete?** [Yes — use deleted_at / No — hard delete]
> **Mutable fields:** [Which fields can change after creation?]
> **Source in PDD:** [Section reference — e.g., "PDD § Core Entities → User"]

| Field | Type | Nullable | Default | Set By | Constraints | Notes |
|-------|------|----------|---------|--------|-------------|-------|
| id | UUID | No | Auto | System | PK | — |
| name | String | No | — | User | MAX 255 | — |
| created_at | Timestamp | No | NOW() | System | — | ISO 8601 |
| updated_at | Timestamp | No | NOW() | System | — | ISO 8601 |
| deleted_at | Timestamp | Yes | NULL | System | — | Soft delete — NULL means active |

**Indexes:**
- `created_at` (range queries)
- `name` (search)

**Notes:** [Any special handling, lifecycle info, or constraints not captured in the table]

### Entity 2: [Name]

> **Soft delete?** [Yes — use deleted_at / No — hard delete]
> **Mutable fields:** [Which fields can change after creation?]
> **Source in PDD:** [Section reference]

| Field | Type | Nullable | Default | Set By | Constraints | Notes |
|-------|------|----------|---------|--------|-------------|-------|
| id | UUID | No | Auto | System | PK | — |
| user_id | UUID | No | — | System | FK → User.id | — |
| status | Enum | No | pending | System | — | Values: pending, active, archived |

**Indexes:**
- `user_id` (queries by user)
- `status` (filtering)

**Notes:** [—]

*(Add additional Entity blocks as needed)*

---

## 🚦 Gate 1 — Entities Complete

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off. This gate matters because Relationships and Data Dictionary both depend on entities being complete and correctly named.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] Every entity in PDD Core Entities has a block here
- [ ] No entities here that aren't in PDD Core Entities (or, if added, flagged in Notes with rationale)
- [ ] Every field has a type, nullable value, default, Set By, and constraint column filled in (or `—`)
- [ ] Every Enum field lists all valid values
- [ ] Every entity declares soft-delete decision explicitly (no TBDs)
- [ ] Every entity declares mutable fields explicitly
- [ ] Every PDD attribute for each entity is represented

**Sign-off:**
> 🚦 **Gate 1** — Entities are complete, every field has a type and constraint, every enum is enumerated. Ready to define relationships.
>
> **Human sign-off:** ☐ Approved — proceed to Relationships

---

## Relationships

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Capture every relationship between entities, with cardinality, FK nullability + reason, on-delete behavior, and soft-delete propagation. A vague relationship here causes mid-coding rework when the migration agent doesn't know what cascade behavior to write.
>
> **A complete relationship block covers:**
> - Cardinality (1:1, 1:M, or M:M) — must match the ERD exactly
> - FK nullable? Yes/No **with a reason**. "Nullable" with no reason is a defect.
> - On delete: Cascade / Restrict / Set NULL — **with a reason** stating why that choice is correct for this business rule
> - Soft delete impact: if entity A is soft-deleted, what happens to B? (orphaned but visible? filtered out by query convention? cascade-soft-delete?)
> - Junction table fields (M:M only) — name and full field list
>
> **Incomplete looks like:**
> - "FK nullable: Yes" with no reason
> - "On delete: Cascade" with no justification — Cascade is almost always wrong; the agent must defend it
> - Soft-delete impact column blank or "TBD"
> - A relationship shown in the ERD but missing here, or vice versa
>
> **Ask triggers — stop and ask the human if:**
> - The PDD says A and B are related but doesn't make cardinality explicit (1:M vs M:M is a real difference)
> - Whether the relationship is required or optional is unclear (FK nullable hinges on this)
> - The business semantics of "what happens to B when A is deleted" aren't obvious from the PDD
>
> **Default heuristic — confirm with the human, don't auto-apply:** Restrict is safer than Cascade. Only cascade when child records are meaningless without the parent (e.g., `OrderLine` belongs to `Order` — line items make no sense without their order). Even then, confirm.
>
> **Remove this block before delivering the filled doc.**

### [Entity A] → [Entity B] (1:M / 1:1 / M:M)

- **Relationship:** [A has many B / A belongs to one B / etc.]
- **FK nullable?** [Yes / No] — **Why:** [Optional relationship (e.g., task may not belong to a project yet) / Required relationship (e.g., comment must belong to a post) — be explicit. "Nullable" with no reason is a design decision waiting to bite you.]
- **On delete:** [Cascade / Restrict / Set NULL] — [why this is the right business behavior]
- **Soft delete impact:** [If A is soft-deleted, what happens to B?]
- **Junction table (M:M only):** [Table name and fields]

### [Entity A] → [Entity B] (1:M / 1:1 / M:M)

- **Relationship:** [—]
- **FK nullable?** [Yes / No] — **Why:** [—]
- **On delete:** [—] — [why]
- **Soft delete impact:** [—]
- **Junction table (M:M only):** [—]

---

## 🚦 Gate 2 — Relationships Complete

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off. This gate matters because the Data Dictionary, Migration Checklist, and downstream API Contract all assume relationships are nailed down.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] Every relationship from the PDD is represented here
- [ ] Every relationship in the ERD has a block here (and vice versa)
- [ ] Every FK has a nullable reason — none left blank
- [ ] Every on-delete behavior has a justification — none left as "Cascade" without defense
- [ ] Every relationship has a soft-delete impact statement
- [ ] Every M:M relationship has its junction table defined

**Sign-off:**
> 🚦 **Gate 2** — Relationships complete, on-delete behavior defensible, soft-delete impacts documented. Ready for Data Dictionary and downstream sections.
>
> **Human sign-off:** ☐ Approved — proceed to Data Dictionary

---

## Data Dictionary

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Produce a single, complete reference of every field across every entity. This is what downstream agents (Migration Checklist, API Contract, Component/Service Map) read when they need to know "what does this field actually mean and how is it constrained?"
>
> **A complete Data Dictionary covers:**
> - One row per field per entity (so the User.email row and the Project.email row, if both existed, are separate rows)
> - **Mutable column** — Yes if the field can be updated via the API after creation, No otherwise. Immutable + Set By = System = never editable.
> - **Valid values** for enums — list every value, not "see Entities section"
> - **Description** — what the field means in business terms, not what its type is
> - **Example** — a realistic value, not "string" or "value here"
>
> **Incomplete looks like:**
> - A field in an Entity block that doesn't appear here
> - A field here that doesn't appear in any Entity block
> - "string" or "value" as an example — write a real example
> - Mutable column blank — every field must declare this
> - An enum field with "see above" instead of the actual values
>
> **Consistency check:** Every field in every Entity block above must have a row here. The dictionary is the authoritative reference — when a downstream agent needs to look something up, they come here, not to the Entity blocks.
>
> **Remove this block before delivering the filled doc.**

**Complete reference of all fields across all entities.**

> **Immutable fields** (Set By = System, Mutable = No) should never be editable via API after creation.

| Field | Entity | Type | Nullable | Default | Set By | Mutable | Constraints | Valid Values | Description | Example |
|-------|--------|------|----------|---------|--------|---------|-------------|--------------|-------------|---------|
| id | User | UUID | No | Auto | System | No | PK | — | Unique identifier. Auto-generated. | `550e8400-e29b-41d4-a716-446655440000` |
| name | User | String | No | — | User | Yes | MAX 255 | Non-empty string | User's full name | `Ryan Falke` |
| email | User | String | No | — | User | Yes | UNIQUE, MAX 255 | Valid email format | User's email address. Must be unique. | `ryan@example.com` |
| password_hash | User | String | No | — | System | Yes | — | — | Bcrypt hash of password. Never plain text. | `$2b$10$...` |
| status | User | Enum | No | active | System | Yes | — | active, inactive, suspended | Account status. Controls login ability. | `active` |
| created_at | User | Timestamp | No | NOW() | System | No | — | ISO 8601 | When record was created | `2025-01-15T10:30:00Z` |
| updated_at | User | Timestamp | No | NOW() | System | Yes | — | ISO 8601 | Last time record was modified | `2025-01-20T14:22:15Z` |
| deleted_at | User | Timestamp | Yes | NULL | System | Yes | — | NULL or timestamp | Soft delete marker. NULL = active. | `2025-01-20T14:22:15Z` |

---

## Data Types Reference

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** This section is mostly fixed — it's the canonical type reference. Add app-specific types only if the project uses something beyond the standard set (e.g., PostGIS geometry types, custom domain types).
>
> **What to do:**
> - Verify the standard rows below match the project's DB engine (e.g., MySQL doesn't have `TIMESTAMPTZ` — use `DATETIME` and note timezone handling)
> - Add a row for any custom or app-specific type
> - Delete rows for types this app doesn't use, only if certain — keep the row if uncertain
>
> **Remove this block before delivering the filled doc.**

| Type | Storage | Notes | Example |
|------|---------|-------|---------|
| UUID | 36 chars | Auto-generated, never user-provided | `550e8400-e29b-41d4-a716-446655440000` |
| String | VARCHAR(n) | Always set MAX length explicitly | `"Ryan Falke"` |
| Integer | INT | Specify range if constrained | `42` |
| Float | DECIMAL(p,s) | Use for money only with explicit precision | `19.99` |
| Boolean | BOOL | Never store as 0/1 integer | `true` |
| Timestamp | TIMESTAMPTZ | Always UTC, convert in app layer | `2025-01-15T10:30:00Z` |
| Enum | VARCHAR | Define all valid values in Data Dictionary | `"active"` |
| JSON/JSONB | JSONB | Use sparingly — hard to query/index | `{"key": "value"}` |

> **Timezone rule:** All timestamps stored as UTC. Display conversion happens in the frontend.
> **Money rule:** Never use Float for currency. Use Integer (cents) or DECIMAL(10,2).

---

## Migration Notes

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Capture the project-level decisions about how schema changes are managed. This feeds directly into the Database Migration Checklist downstream — that doc is the operational counterpart; this section is the policy layer.
>
> **A complete Migration Notes section covers:**
> - The migration tool (must be a specific tool, not "TBD")
> - Initial schema date
> - Rollback strategy in 1–2 sentences
> - Anticipated future changes that the human has mentioned but aren't shipping in v1
> - Seed order — every parent table before its children, junction tables last
>
> **Ask triggers — stop and ask the human if:**
> - The migration tool hasn't been chosen — this can't be left as "TBD"
> - The rollback strategy is unknown — even "restore from backup, no automated rollback" is a valid answer
>
> **Remove this block before delivering the filled doc.**

**Versioning:** [Tool — e.g., Flyway, Liquibase, Prisma Migrate, custom] — migrations named `V001__description.sql`
**Initial schema date:** [Date]

### Change Rules
- **Non-destructive (safe):** Adding tables, adding nullable columns, adding indexes
- **Destructive (requires care):** Renaming columns, dropping columns, changing types
- **Never in production without backup:** Dropping tables, truncating data

### Rollback Strategy
[How do we undo a bad migration? Manual script / tool-managed / point-in-time restore?]

### Anticipated Future Changes
- [Change] — [Why it's coming, when to expect it]

### Seeding

> ⚠️ **Seed order matters.** Parent tables must be seeded before child tables due to FK constraints. List tables in dependency order — if User must exist before Project, seed User first.

**Seed order:**
1. [Table with no FK dependencies — e.g., User, Tag]
2. [Table that depends on #1 — e.g., Project (FK → User)]
3. [Table that depends on #2 — e.g., Task (FK → Project)]
4. [Junction tables last — e.g., ProjectTag]

| Environment | Seed Data | Notes |
|-------------|-----------|-------|
| Development | Full fake dataset | Realistic volumes for testing |
| Staging | Anonymized prod snapshot | Mirror prod structure |
| Production | Minimal required data only | e.g., admin user, default config |

---

## Query Patterns

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Document the most common read and write operations against this schema, so the indexes you defined in the Entities section can be defended. Every index from above should have a query here that uses it.
>
> **A complete Query Patterns section covers:**
> - The 5–10 most common queries the app will execute (not exhaustive — just the ones that matter for performance)
> - Each query annotated with the index it relies on (cross-reference Entities)
> - Performance notes only where they matter — "no optimization needed" for low-volume operations is fine
>
> **Incomplete looks like:**
> - An index in the Entities section that has no query using it (probably an unnecessary index)
> - A high-frequency query that depends on a column not indexed in Entities (probably a missing index)
>
> **Ask triggers:**
> - Workflows in the PDD imply queries this section doesn't capture (e.g., "show all overdue tasks for this user" implies a query on user_id + status + due_date) — ask the human whether this is in scope for v1, then index accordingly
>
> **Remove this block before delivering the filled doc.**

> List the most common operations. Flag anything performance-sensitive.
> Show raw SQL or ORM syntax depending on your stack — be consistent.

### Read Patterns

| Operation | Query / Method | Performance Notes |
|-----------|---------------|-------------------|
| Get all projects for a user | `WHERE user_id = ?` | Index on `user_id` |
| Get active tasks in a project | `WHERE project_id = ? AND status = 'active'` | Compound index on `(project_id, status)` |
| Count tasks by status | `GROUP BY status` | Low volume — no optimization needed |

### Write Patterns

| Operation | Query / Method | Notes |
|-----------|---------------|-------|
| Create user | `INSERT INTO users (name, email, ...) VALUES (...)` | Email uniqueness enforced at DB level |
| Soft delete record | `UPDATE x SET deleted_at = NOW() WHERE id = ?` | Never hard delete |

> ⚠️ Never use `SELECT *` in production queries — always specify columns.

---

## Constraints / Validation Rules

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Make every business rule's enforcement location explicit. "Validation happens" is not a design — *where* validation happens is the design. This section is the bridge to the Tech Spec (which defines API-level validation) and the Component/Service Map (which defines service-level validation).
>
> **A complete Constraints section covers:**
> - Every rule classified by enforcement layer: Database / Application / API
> - Every critical rule enforced at MORE than one layer (defense in depth)
> - Cross-reference: every UNIQUE / FK / CHECK in the Entity blocks above should appear in the Database-Level table here
>
> **Incomplete looks like:**
> - A rule listed without an enforcement layer
> - A critical rule enforced only in one layer (e.g., email uniqueness only at the API level — bypassable by direct DB writes)
> - An empty table — every meaningful rule lives somewhere; if a table is empty, you haven't surfaced enough rules
>
> **Ask triggers — stop and ask the human if:**
> - The PDD Business Rules in a Feature mention something that could be DB-enforced or app-enforced (e.g., "users can have at most 10 active projects") — ask which layer; the answer affects schema (DB check vs app logic)
>
> **Remove this block before delivering the filled doc.**

> Every rule must specify WHERE it's enforced. A rule enforced nowhere is not a rule.

### Database-Level (enforced by DB engine)

| Rule | Table | Implementation |
|------|-------|---------------|
| Email must be unique | users | UNIQUE constraint |
| FK must reference valid record | projects | FOREIGN KEY constraint |

### Application-Level (enforced in service / business logic)

| Rule | Entity | Notes |
|------|--------|-------|
| Task cannot be deleted if status = 'completed' | Task | Check before delete call |
| User cannot have more than 10 active projects | Project | Count check before insert |

### API-Level (enforced at request validation)

| Rule | Endpoint | Notes |
|------|----------|-------|
| Name cannot be empty | POST /projects | Return 400 if missing or blank |
| Email must be valid format | POST /users | Regex or library validation |

> ⚠️ Critical rules should be enforced at DB level AND app level. Never rely on a single layer.

---

## 🚦 Gate 3 — Full Schema Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off. This is the final gate before downstream coding-phase docs (Migration Checklist, API Contract) consume this schema.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] All sections are complete — Status table shows no ⏳ or 🔄
- [ ] Gates 1 and 2 are signed off
- [ ] Every field in every Entity has a corresponding row in the Data Dictionary
- [ ] Every Data Dictionary row has a corresponding field in an Entity
- [ ] Every Enum field has all its valid values listed
- [ ] Every relationship has on-delete behavior justified and soft-delete impact noted
- [ ] Every index in Entity blocks has at least one query in Query Patterns that uses it
- [ ] Every critical business rule is enforced at the DB level AND the app level (defense in depth)
- [ ] Migration tool is chosen — not "TBD"
- [ ] Seed order is documented — parents before children
- [ ] No entity in the PDD Core Entities is missing from this doc
- [ ] No entity here is missing from the PDD Core Entities, unless explicitly flagged as a discovered gap and approved by the human

**Sign-off:**
> 🚦 **Gate 3** — DB Schema complete, internally consistent, traces fully to the PDD. Ready to feed the Tech Spec, Migration Checklist, and API Contract.
>
> **Human sign-off:** ☐ Approved — DB Schema complete. Proceed to Tech Spec (and parallel: UI/UX).
