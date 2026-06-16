# Database Migration Checklist: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized
> **Fill order:** Coding-phase doc. Fills in parallel with API Contract, Component/Service Layer Map, and Testing Strategy after Module Breakdown is at ✅ Done. Begin only after Module Breakdown Gate 2 is signed off — Module IDs are referenced in every Registry row and every Detail Block.
>
> **Source docs (every migration traces upstream to these):**
> - `[AppName]_DB_Schema.md` — Entities, Relationships, Constraints, Data Dictionary, Migration Notes. **Primary source.** Every migration creates, modifies, or removes something defined in Schema. If a migration touches a field that isn't in Schema, that's a Schema gap — flag it and go back to the human.
> - `[AppName]_Technical_Spec.md` — Tech Stack (drives migration tooling: Prisma / Flyway / Liquibase / Knex / custom), Deployment & Environments (drives environment promotion rules and zero-downtime requirements), Environment Variables (DB connection vars referenced in pre/post checks).
> - `[AppName]_Module_Breakdown.md` — Module Registry and Detail Blocks. **Every migration row in the Registry references the Module ID(s) that introduce it.** Module Breakdown's Detail Block "Migrations" column closes the loop in the other direction — both sides must match.
>
> **Downstream docs that consume this one (write to feed them):**
> - `[AppName]_Module_Breakdown.md` — Each Module Detail Block's "Migrations" column references DB-XX IDs declared here. Bidirectional link — Gate 1 enforces both sides match.
> - `[AppName]_Deployment_Config.md` — The CI/CD pipeline runs migrations in the order defined in the Registry. Migration ordering here drives pipeline step ordering there.
> - `[AppName]_Build_Decisions_Log.md` — BD entries reference DB-XX IDs when a workaround, deviation, or rollback occurs (e.g., "BD-04: rolled back DB-07 due to FK conflict — re-applied as DB-09").
> - `[AppName]_Pre_Build_Validation_Checklist.md` — Verifies Migration Checklist ↔ Module Breakdown bidirectional link and Migration Checklist ↔ Deployment Config ordering match before coding begins.
>
> **Agent role:** Translate Schema entity definitions into ordered, registered, reversible migration units with explicit pre/post checks, rollback procedures, and environment promotion tracking. The human is the designer; the agent enforces precision and consistency. No invented migrations — every DB-XX traces to a Schema change driven by a Module's needs. No invented dependencies — every "Depends On" relationship traces to a real schema or seed-data prerequisite.
>
> **The three rules while filling this doc:**
> 1. Everything written traces to Schema + Tech Spec + Module Breakdown + confirmed human input. No invented migrations, no invented Module IDs, no invented rollback procedures.
> 2. If a migration's reversibility, environment policy, or dependency is unclear, stop and ask. Do not write a rollback procedure on a guess — wrong rollback SQL is worse than no rollback SQL.
> 3. Output must be specific enough that a coding agent (or DBA) can apply any single migration by reading only its Detail Block + the Conventions section, without re-deriving anything from Schema or Module Breakdown.
>
> **Two failure modes drive most of the design here — both are addressed by gates:**
> - **Reversibility lie.** A migration is marked "reversible" but the rollback SQL doesn't actually restore the pre-migration state (e.g., drops a column whose data was backfilled from elsewhere — the rollback recreates the column but can't recreate the data). Every Detail Block has a forced reversibility check — "non-reversible, restore from backup" is a valid answer, but it must be explicit.
> - **Module ID drift.** Registry says DB-04 belongs to M-07, but Module Breakdown's M-07 Detail Block doesn't list DB-04 in its Migrations column. Or worse — the migration is referenced in two Modules' Detail Blocks and the Registry only points to one. Gate 1 enforces bidirectional consistency.
>
> **When this doc is fully filled and approved:** Remove every `🤖 AGENT INSTRUCTIONS` block, every `❓ AGENT PAUSE` prompt, and the agent-facing instruction prose inside `🚦 GATE` blocks. Keep the gate checklists, sign-off lines, and all filled content. The finished doc reads clean for humans.
>
> **Cleanup verification (before declaring the doc done):**
> - Search the file for `🤖` — zero hits
> - Search the file for `❓ AGENT PAUSE` — zero hits
> - Search the file for "Remove this block" — zero hits
> - Every `🚦 GATE` block contains only its checklist and sign-off line — no agent prose
>
> **Internal fill order (strict — section dependencies):**
> 1. Overview
> 2. Migration Type Reference (review only — adjust if project needs new types)
> 3. Zero-Downtime Migration Patterns (review only — read before writing any migration touching an existing production table)
> 4. **Conventions & Tooling** — versioning, naming, transactional rules, migration tool. Must come first; every subsequent section references these.
> 5. **Migration Registry** — must come after Conventions; the canonical inventory. Everything else references DB-XX IDs from here.
> 6. **Dependency Map** — must come after Registry; can't map dependencies that aren't inventoried.
> 7. **🚦 Gate 1 — Conventions, Registry & Dependency Map Locked** (foundation lock; human sign-off before Detail Blocks)
> 8. **Migration Detail Blocks** — must come after Gate 1; one block per migration in Registry order.
> 9. Rollback Procedures (global rules) — concurrent with Detail Blocks; reinforces the per-block SQL (Down).
> 10. Environment Status — populated as migrations are applied.
> 11. Pre-Migration / Post-Migration Checklists — review only; standard operational gates.
> 12. Open Questions — populate as they surface.
> 13. **🚦 Gate 2 — Full Migration Checklist Sign-Off** (final gate before coding begins).

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Overview | 🔲 Not Started | — | — |
| Migration Type Reference | 🔲 Not Started | — | Review and adjust per project |
| Zero-Downtime Migration Patterns | 🔲 Not Started | — | Read before writing any migration touching an existing production table |
| Conventions & Tooling | 🔲 Not Started | — | Strict ordering — fill before Registry |
| Migration Registry | 🔲 Not Started | — | Strict ordering — fill after Conventions |
| Dependency Map | 🔲 Not Started | — | Strict ordering — fill after Registry |
| 🚦 Gate 1 — Conventions, Registry & Dependency Map Locked | 🔲 Not Started | — | Foundation lock — human sign-off required before Detail Blocks |
| Migration Detail Blocks | 🔲 Not Started | — | One block per migration — strict ordering applies |
| Rollback Procedures | 🔲 Not Started | — | Global rules — populate concurrently with Detail Blocks |
| Environment Status | 🔲 Not Started | — | Populated as migrations are applied |
| Pre-Migration / Post-Migration Checklists | 🔲 Not Started | — | Operational — review only |
| Open Questions | 🔲 Not Started | — | Populate as they surface |
| 🚦 Gate 2 — Full Migration Checklist Sign-Off | 🔲 Not Started | — | Final gate before coding begins |

**Migration Status values:** 🔲 Not Started / 🔄 In Progress / ✅ Applied / 🚫 Failed / ↩️ Rolled Back

> ⚠️ **Status scheme note.** This template uses the Migration-Specific scheme defined in `Design_Document_Template_Context.md`. `✅ Applied` replaces `✅ Done` for individual migrations (semantically identical — applied = done for migrations). `🚫 Failed` and `↩️ Rolled Back` are migration-specific. For non-migration sections (e.g., "Overview", "Conventions"), use standard Coding Status: 🔲 / 🔄 / 👀 / ✅ / 🚫.

---

## Overview

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Anchor this doc to its sources. Anyone reading this should immediately know which Schema and which Module Breakdown the migrations trace to, what DB engine is in play, and the total scope.
>
> **A complete Overview covers:**
> - App name and DB engine (pulled from Tech Spec → Tech Stack)
> - Migration tool name and version (pulled from Tech Spec → Tech Stack)
> - Total migration count (matches Registry row count)
> - Explicit references to Schema, Tech Spec, and Module Breakdown as source-of-truth docs
> - One-sentence summary of the migration story (e.g., "Phase 1 stands up 4 core tables; Phase 2 adds 3 index migrations; Phase 3 adds reporting tables")
>
> **Incomplete looks like:**
> - "PostgreSQL" without the version
> - "Flyway" without specifying which migration tool conventions are in use
> - A migration count that doesn't match the Registry below
>
> **Ask triggers:**
> - Tech Spec → Tech Stack is silent on migration tool → ask the human before writing one here
> - Tech Spec → Deployment & Environments doesn't define dev/staging/prod → ask before assuming three environments
>
> **Remove this block before delivering the filled doc.**

- **App:** [App Name]
- **DB engine:** [PostgreSQL 15 / MySQL 8.0 / SQLite 3 / etc. — from Tech Spec → Tech Stack]
- **Migration tool:** [Prisma Migrate / Flyway / Liquibase / Knex / custom / etc. — from Tech Spec → Tech Stack]
- **Total migrations:** [#] (must match Registry row count)
- **Environments:** [Dev / Staging / Prod — from Tech Spec → Deployment & Environments]
- **Source-of-truth docs:**
  - Schema: `[AppName]_DB_Schema.md`
  - Tech Spec: `[AppName]_Technical_Spec.md`
  - Module Breakdown: `[AppName]_Module_Breakdown.md`
- **Migration story (one sentence):** [e.g., "Phase 1 stands up 4 core tables; Phase 2 adds 3 indexes and a seed migration; Phase 3 adds 2 reporting tables and 1 backfill."]

---

## Migration Type Reference

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Confirm the standard migration types cover this project's needs. Add types only if the project genuinely needs one not listed (rare). Do not delete rows — keep the full reference even if some types are unused this project.
>
> **A complete review:**
> - All 11 standard types are present and unmodified
> - Project-specific types (if any) are appended with the same column structure
> - Reversibility column is accurate per type (do not soften "No — data loss" to make a destructive migration feel safer)
>
> **Ask triggers:**
> - Project introduces a migration pattern not covered by the 11 standard types (e.g., partitioning, materialized view refresh, custom extension install) → ask the human whether to add a new row
>
> **Remove this block before delivering the filled doc.**

| Type | Description | Examples | Reversible? |
|------|-------------|----------|-------------|
| **Create** | Add a new table | `create_users_table` | Yes — drop table |
| **Add Column** | Add column to existing table | `add_status_to_projects` | Yes — drop column |
| **Add Index** | Add index to existing table | `add_index_user_id_on_tasks` | Yes — drop index |
| **Add FK** | Add foreign key constraint | `add_fk_project_id_to_tasks` | Yes — drop constraint |
| **Add Constraint** | Add unique, check, or not-null constraint | `add_unique_email_to_users` | Yes — drop constraint |
| **Rename** | Rename table or column | `rename_username_to_name_on_users` | Yes — rename back (but requires multi-deploy — see Pattern 3) |
| **Change Type** | Alter column type | `change_amount_to_decimal_on_orders` | Risky — data loss possible if narrowing |
| **Backfill** | Populate new column with data | `backfill_status_on_projects` | Manual — data-dependent |
| **Drop Column** | Remove column from table | `drop_legacy_token_from_users` | **No** — data loss; must restore from backup |
| **Drop Table** | Remove entire table | `drop_legacy_logs_table` | **No** — data loss; must restore from backup |
| **Seed** | Insert required baseline data | `seed_default_roles` | Partial — delete rows (only safe if rows haven't been referenced) |

> ⚠️ **Destructive migrations** (Drop Column, Drop Table) require backup confirmation and a deploy gap (see Zero-Downtime Pattern 4). Every destructive migration's Detail Block must explicitly state "Reversible: No — restore from backup".

---

## Zero-Downtime Migration Patterns

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** This section is a permanent reference, not something you write per project. Read it before writing any migration that touches an existing production table. When writing a Detail Block, if the migration matches one of the patterns below, the Detail Block's "Zero-Downtime Pattern Used" field must name which pattern.
>
> **A complete review:**
> - All 5 patterns are present and unmodified
> - The Zero-Downtime Checklist at the end is included in every Detail Block for migrations touching existing tables
>
> **Do not edit the patterns.** They are battle-tested. If a project genuinely needs a new pattern (rare), add it as Pattern 6 with the same structure — never replace an existing pattern.
>
> **Remove this block before delivering the filled doc.**

> **Read this before writing any migration that touches an existing table in production.**
>
> A migration that works perfectly in dev can take a production table offline for minutes when that table has millions of rows and live traffic. The patterns below prevent that. Use them whenever the app must stay up during a deploy.
>
> **Rule of thumb:** If a migration acquires a full table lock AND the table has production traffic, it needs the safe pattern. When in doubt, use the safe pattern.

---

### Why Naive Migrations Cause Downtime

Most migrations that modify existing tables acquire an `ACCESS EXCLUSIVE` lock — the strongest lock in Postgres. While that lock is held:
- All reads and writes to that table are blocked
- Queries queue up; if the migration is slow, the queue grows, connections exhaust, and the app goes down

The operations most likely to cause this: adding a `NOT NULL` column without a default, adding a constraint, building an index without `CONCURRENTLY`, renaming a column, and changing a column type.

---

### Pattern 1: Adding a Column Safely

**Naive (dangerous on large tables):**
```sql
ALTER TABLE orders ADD COLUMN status VARCHAR(20) NOT NULL DEFAULT 'pending';
```
This rewrites every row to add the default value. On a large table this holds a full lock for minutes.

**Safe (3-step expand/backfill/constrain):**

**Step 1 — Add nullable column, no default (fast, minimal lock):**
```sql
-- DB-XX: add_status_to_orders
ALTER TABLE orders ADD COLUMN status VARCHAR(20);
```

**Step 2 — Backfill in batches (no lock, run in background):**
```sql
-- DB-XX+1: backfill_status_on_orders
-- Run in batches to avoid long-running transaction
UPDATE orders SET status = 'pending' WHERE status IS NULL AND id IN (
  SELECT id FROM orders WHERE status IS NULL LIMIT 10000
);
-- Repeat until all rows are populated. Script this — don't run manually.
```

**Step 3 — Add NOT NULL constraint (fast once backfill is complete):**
```sql
-- DB-XX+2: constrain_status_on_orders
ALTER TABLE orders ALTER COLUMN status SET NOT NULL;
```

> ⚠️ **Never combine steps 1 and 3 into one migration.** Deploy step 1, verify backfill is complete, then deploy step 3. The deployed app code must handle NULL status between steps 1 and 3.

---

### Pattern 2: Adding an Index Safely

**Naive (dangerous — full table lock for duration of index build):**
```sql
CREATE INDEX idx_orders_status ON orders (status);
```

**Safe (`CONCURRENTLY` — no lock, reads/writes continue during build):**
```sql
-- Must run outside a transaction block — cannot be in BEGIN/COMMIT
CREATE INDEX CONCURRENTLY idx_orders_status ON orders (status);
```

> ⚠️ `CONCURRENTLY` takes longer and cannot run inside a transaction. Most migration tools wrap everything in a transaction — check yours and disable it for this migration if needed.
> If the build fails partway, it leaves an `INVALID` index. Check with: `SELECT indexname, pg_index.indisvalid FROM pg_indexes JOIN pg_class ON pg_class.relname = pg_indexes.indexname JOIN pg_index ON pg_index.indexrelid = pg_class.oid WHERE tablename = 'orders';`
> Drop the invalid index and retry: `DROP INDEX CONCURRENTLY idx_orders_status;`

---

### Pattern 3: Renaming a Column Safely

Renaming a column is a breaking change — deployed code referencing the old name breaks the moment the migration runs.

**Safe (4-step):**

**Step 1 — Add new column:**
```sql
ALTER TABLE users ADD COLUMN display_name VARCHAR(255);
```

**Step 2 — Dual-write:** Deploy app code that writes to BOTH `username` (old) and `display_name` (new).

**Step 3 — Backfill new column:**
```sql
UPDATE users SET display_name = username WHERE display_name IS NULL;
```

**Step 4 — Switch reads to new column:** Deploy app code that reads from `display_name` only.

**Step 5 — Drop old column** (after confirming no code references it):
```sql
ALTER TABLE users DROP COLUMN username;
```

> This is a 3-deploy process. Plan accordingly — it cannot be done in a single release.

---

### Pattern 4: Dropping a Column Safely

**Never drop a column in the same deploy as the code change that removes its usage.**

**Safe (2-step):**

**Step 1 — Remove all code references to the column.** Deploy and verify in production. Wait at least one full deploy cycle.

**Step 2 — Drop the column:**
```sql
ALTER TABLE users DROP COLUMN legacy_token;
```

> The gap between steps exists because if the deploy fails and you roll back the code, you need the column to still be there. Dropping first means a code rollback hits a missing column.

---

### Pattern 5: Changing a Column Type Safely

Type changes that require a full table rewrite (e.g., `VARCHAR` → `TEXT`, `INT` → `BIGINT`) are high-risk on large tables.

**Safe:** Use the Add Column → Backfill → Constrain pattern (Pattern 1) with the new type, then drop the old column (Pattern 4). Never use `ALTER COLUMN TYPE` directly on a large production table.

---

### Zero-Downtime Checklist (add to Migration Detail Block for any migration touching existing tables)

- [ ] Does this migration acquire a full table lock? (ALTER TABLE on large table, non-CONCURRENT index)
- [ ] If yes: is the safe pattern being used instead?
- [ ] Is there a gap between adding a nullable column and adding the NOT NULL constraint?
- [ ] Is any index being built with `CONCURRENTLY`?
- [ ] Is there deployed app code that handles the intermediate state (e.g., nullable column before backfill)?
- [ ] For renames/drops: has code been deployed and verified before the schema change?

---

## Conventions & Tooling

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** This section is filled BEFORE the Migration Registry. Every Registry row references the convention rules defined here (filename pattern, status values, transactional policy). If a human asks you to start filling the Registry before Conventions is complete, push back:
>
> > "Conventions & Tooling must be locked before the Registry. Every Registry row references the filename pattern, transactional policy, and tooling rules from this section. Filling Registry first means rewriting it once Conventions changes. Let's finish Conventions first — should take 10 minutes."
>
> **Your job:** Pin down the rules that govern every migration in this project. Filename format. Migration tool. Whether migrations run in a transaction. How rollback files are named. Once locked, these are not negotiable per-migration.
>
> **A complete Conventions section covers:**
> - Filename pattern (literal — show the format with placeholders, then give 2–3 real examples)
> - Migration tool name + version + relevant config (e.g., "Flyway 9, configured to skip migrations in `pending` folder")
> - Transactional policy (per-migration or all-or-nothing; what to do for `CONCURRENTLY` operations that can't be in a transaction)
> - Rollback file convention (does the tool support a `Down` file? If yes, what's the naming? If no, where does rollback SQL live? — for Flyway 9, undo migrations are paid-only, so rollback SQL typically lives inline in the Detail Block)
> - Migration ordering rule (version numbers strictly increasing; no reusing a version even after a rollback — use a new version)
> - Environment promotion rule (Dev → Staging → Prod; no skipping; verification required between)
> - Who can apply migrations in each environment (Dev: any dev; Staging: lead dev; Prod: lead dev + ops sign-off)
>
> **Incomplete looks like:**
> - "Filename format: V001__name.sql" without showing real examples
> - "We use Flyway" without specifying version and transactional policy
> - Silence on what to do for `CONCURRENTLY` operations
> - No rule for what happens after a rollback (reuse the version? use a new one?)
>
> **Ask triggers:**
> - Tech Spec → Tech Stack lists a migration tool that doesn't natively support rollback files → ask the human where rollback SQL lives
> - Tech Spec → Deployment & Environments is silent on who applies migrations in prod → ask before writing an authorization rule
>
> **Cross-reference checklist:**
> - Migration tool matches Tech Spec → Tech Stack
> - Environment list matches Tech Spec → Deployment & Environments
> - Status values match the Migration-Specific scheme in `Design_Document_Template_Context.md` (🔲 / 🔄 / ✅ Applied / 🚫 Failed / ↩️ Rolled Back)
>
> **Remove this block before delivering the filled doc.**

### Filename Convention

```
V[###]__[verb]_[description].sql
```

- `V` prefix is literal
- `###` is a zero-padded 3-digit version number, strictly increasing
- `__` is a double underscore (separator required by most tools)
- `[verb]_[description]` is snake_case, action-first

**Examples:**
- `V001__create_users_table.sql`
- `V002__add_status_to_projects.sql`
- `V015__backfill_status_on_orders.sql`

**Rule:** Version numbers are never reused. If V007 fails and is rolled back, the fix is V008 — not V007 retried.

### Migration Tool

- **Tool:** [Prisma Migrate / Flyway / Liquibase / Knex / custom — from Tech Spec → Tech Stack]
- **Version:** [Tool version]
- **Config notes:** [e.g., "Flyway configured with `validateOnMigrate=true`; `outOfOrder=false`"]
- **Rollback support:** [Native (Liquibase, paid Flyway) / Manual inline SQL (free Flyway, Prisma, custom) — affects where Down SQL lives in Detail Blocks]

### Transactional Policy

- **Default:** [All migrations run in a single transaction — failure rolls back the whole migration / Each migration is its own transaction / etc.]
- **Exception:** Migrations using `CREATE INDEX CONCURRENTLY` or other operations that cannot run in a transaction must be marked `transactional: false` in the migration tool config and applied with no transaction wrapper. Detail Block must note this explicitly.

### Rollback Convention

- **Where rollback SQL lives:** [Inline in Detail Block "SQL (Down / Rollback)" / Separate `U[###]__[name].sql` undo file / N/A — destructive migrations restore from backup]
- **Reversibility marking:** Every Detail Block must explicitly state one of: "Reversible — rollback SQL runs cleanly", "Reversible with caveats — see Notes/Risks", "Non-reversible — restore from backup". No silent omissions.
- **Post-rollback rule:** A rolled-back migration's version number is retired. The fix gets a new version number.

### Environment Promotion Rule

```
Dev ✅ → Staging ✅ → Production ✅
```

- Every migration must be applied and verified in Dev before Staging.
- Every migration must be applied and verified in Staging before Production.
- No skipping environments. No applying directly to Production without Staging verification.

### Authorization

| Environment | Who can apply |
|-------------|---------------|
| Dev | Any developer |
| Staging | [Lead dev / DBA / role] |
| Production | [Lead dev + ops sign-off / DBA + change-control approval / etc.] |

---

## Migration Registry

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** This section is filled AFTER Conventions & Tooling and BEFORE Dependency Map. If a human asks you to start the Dependency Map before the Registry is complete, push back:
>
> > "Dependency Map needs the full Registry as input — every dependency edge points from one DB-XX to another DB-XX. Filling Dependency Map on a partial Registry means redoing it. Let's finish the Registry first."
>
> **Your job:** Produce the canonical inventory of every migration in the project. One row per migration. Ordered by version number. Every row has a Module ID linking it back to the feature that requires it.
>
> **A complete Registry covers:**
> - One row per migration — no gaps in version numbers (V001, V002, V003... not V001, V003, V005)
> - Every migration has a Module ID from `[AppName]_Module_Breakdown.md` Module Registry
> - Every migration's Type matches one of the rows in Migration Type Reference
> - Depends On column lists DB-XX IDs only (no migration names, no Module IDs) — empty for the first migration
> - Environment columns (Dev / Staging / Prod) start at 🔲 for new migrations
>
> **Incomplete looks like:**
> - A row with no Module ID (every migration must trace to a Module)
> - A row with a Type not in the Type Reference
> - Depends On listing a migration that comes later in version order (impossible — dependencies must precede)
> - A migration that creates a table whose columns aren't defined in Schema (Schema gap — flag it)
>
> **Ask triggers:**
> - A Module in Module Breakdown lists a DB Entity in its Detail Block but no corresponding migration exists in this Registry → ask the human which migration creates it
> - A Schema entity has no migration creating it → ask before assuming the human forgot
> - Two migrations seem to do the same thing → ask before assuming one is redundant
>
> **Cross-reference checklist (verify before declaring section done):**
> - Every Module ID in this Registry exists in Module Breakdown's Module Registry
> - Every Schema entity has at least one migration creating it
> - Every migration's Type is one of the rows in Migration Type Reference (no invented types)
> - Version numbers are continuous (V001, V002, V003...) — no gaps
>
> **Remove this block before delivering the filled doc.**

> One row per migration. Ordered by version number. Search by DB-XX ID to jump to the Detail Block.

| ID | Version | Migration Name | Type | Module | Depends On | Reversible? | Dev | Staging | Prod |
|----|---------|---------------|------|--------|------------|-------------|-----|---------|------|
| DB-01 | V001 | `create_users_table` | Create | M-01 | — | Yes — drop table | 🔲 | 🔲 | 🔲 |
| DB-02 | V002 | `create_projects_table` | Create | M-03 | DB-01 | Yes — drop table | 🔲 | 🔲 | 🔲 |
| DB-03 | V003 | `create_tasks_table` | Create | M-04 | DB-02 | Yes — drop table | 🔲 | 🔲 | 🔲 |
| DB-04 | V004 | `add_index_user_id_on_projects` | Add Index | M-03 | DB-02 | Yes — drop index | 🔲 | 🔲 | 🔲 |
| DB-05 | V005 | `seed_default_roles` | Seed | M-02 | DB-01 | Partial — delete rows | 🔲 | 🔲 | 🔲 |

> **Module column rule:** Every row's Module ID must exist in `[AppName]_Module_Breakdown.md` Module Registry. Bidirectional link — Module Breakdown's Detail Block for that Module must list this DB-XX in its Migrations column. Gate 1 enforces both sides.
>
> **Reversible column rule:** Pulled from Migration Type Reference. For destructive migrations (Drop Column, Drop Table), this column says "No — restore from backup". For backfills, "Manual — data-dependent". No silent omissions.

---

## Dependency Map

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** This section is filled AFTER the Registry is locked. Every node in this map is a DB-XX from the Registry; every edge is a "Depends On" relationship from the Registry. This section visualizes and validates what the Registry's Depends On column declares.
>
> **Your job:** Validate that the migration dependency graph is acyclic, complete, and matches the Registry. Surface ordering bugs before Gate 1.
>
> **A complete Dependency Map covers:**
> - A text-based tree or graph showing every DB-XX from the Registry
> - A flat list mapping each DB-XX to its Depends On migrations with a "Why" column (one phrase explaining the dependency)
> - Explicit confirmation that no cycles exist
> - Explicit confirmation that no orphan references exist (every Depends On entry points to a real DB-XX in the Registry)
>
> **Incomplete looks like:**
> - A migration in the Registry not appearing in this map
> - A "Depends On" relationship listed in the Registry but missing here
> - A cycle (DB-03 depends on DB-04 which depends on DB-03) — flag immediately as a fatal error
> - A "Why" column saying "needed" without specifying what the dependency provides
>
> **DAG validation rules (the agent must check before passing this section to Gate 1):**
> - No cycles — trace every dependency chain to confirm it terminates
> - No self-references — a migration cannot depend on itself
> - No orphan references — every DB-XX in a Depends On column exists in the Registry
> - Completeness — every Registry row appears in the dependency tree (as a node or root)
> - Order — every Depends On target has a lower version number than the dependent
>
> **Ask triggers:**
> - A migration depends on a migration that hasn't been added to the Registry yet → ask whether the missing migration is needed or whether the dependency is wrong
> - The dependency tree has more than one root and the human expected a single root → ask before assuming the multi-root structure is intentional
>
> **Remove this block before delivering the filled doc.**

> Shows which migrations must run before others. Respect this order in every environment.

### Dependency Tree

```
[DB-01: create_users_table]
├── [DB-02: create_projects_table]
│   ├── [DB-03: create_tasks_table]
│   └── [DB-04: add_index_user_id_on_projects]
└── [DB-05: seed_default_roles]
```

### Dependency List

| Migration | Depends On | Why |
|-----------|------------|-----|
| DB-01 | — | No dependencies — starting point |
| DB-02 | DB-01 | FK → users.id requires users table to exist |
| DB-03 | DB-02 | FK → projects.id requires projects table to exist |
| DB-04 | DB-02 | Index on projects.user_id — table must exist |
| DB-05 | DB-01 | Seeds users.role data — table must exist |

### DAG Validation

- [ ] No cycles confirmed (every chain terminates at a root)
- [ ] No self-references confirmed
- [ ] No orphan references confirmed (every Depends On target exists in Registry)
- [ ] Every Registry row appears in the tree (as root or descendant)
- [ ] Every Depends On target has a lower version number than the dependent

---

## 🚦 Gate 1 — Conventions, Registry & Dependency Map Locked

> 🤖 **AGENT INSTRUCTIONS**
>
> **Why this gate matters:** Conventions, Registry, and Dependency Map are the foundation every Detail Block references. Changing any of them after Detail Blocks are written means rewriting every affected Detail Block. This gate exists to catch foundation problems before that cost is incurred.
>
> **Human sign-off is required before Detail Blocks begin.** Do not start writing Detail Blocks until this gate is checked.
>
> **Gate procedure:**
> 1. Walk every checklist item below. For each, open the relevant section and verify — do not check from memory.
> 2. If any check fails, do NOT silently fix it. Stop, flag the gap to the human, and resolve before continuing.
> 3. Pay special attention to the bidirectional Module ID link — open Module Breakdown for every Module ID listed here and confirm that Module's Detail Block references the corresponding DB-XX in its Migrations column.
>
> **Remove this instruction block before delivering. Keep the checklist and sign-off line.**

### Conventions Checks

- [ ] Filename pattern matches the literal format declared (V###__verb_description.sql)
- [ ] Migration tool name + version match Tech Spec → Tech Stack
- [ ] Transactional policy is declared and includes an explicit rule for `CONCURRENTLY` operations
- [ ] Rollback convention is declared (inline / undo files / restore from backup)
- [ ] Environment promotion rule (Dev → Staging → Prod) is declared
- [ ] Authorization rule is filled for all three environments

### Registry Checks

- [ ] Every row has a DB-XX ID (no blanks)
- [ ] Every row has a Module ID that exists in `[AppName]_Module_Breakdown.md` Module Registry
- [ ] Every row's Type is one of the rows in Migration Type Reference
- [ ] Every row's Depends On lists only DB-XX IDs that exist in this Registry
- [ ] Every row's Reversible value matches the Type Reference column
- [ ] Version numbers are continuous (V001, V002, V003...) — no gaps
- [ ] Total row count matches the "Total migrations" number in Overview

### Bidirectional Module Link Checks

- [ ] **For every Module ID referenced in this Registry**, opened `[AppName]_Module_Breakdown.md` and confirmed that Module's Detail Block lists the corresponding DB-XX in its Migrations column.
- [ ] **For every DB-XX referenced in any Module Breakdown Detail Block's Migrations column**, confirmed that DB-XX exists in this Registry with the matching Module ID.
- [ ] Any mismatch is flagged in the doc (do not silently fix — Module Breakdown owns Module IDs; this doc owns DB-XX IDs).

### Dependency Map Checks

- [ ] Every Registry row appears in the dependency tree
- [ ] Every Depends On relationship in the Registry is reflected in the tree
- [ ] No cycles (every chain terminates at a root)
- [ ] No self-references
- [ ] No orphan references (every Depends On target exists in Registry)
- [ ] Every Depends On target has a lower version number than the dependent
- [ ] DAG Validation checklist in Dependency Map section is fully checked

### Schema Cross-Reference Checks

- [ ] Every Schema entity has at least one migration creating it
- [ ] Every migration that creates a table references a Schema entity (no orphan tables — if a migration creates a table not in Schema, that's a Schema gap)
- [ ] Every migration that adds a column references a Schema field (no orphan columns)
- [ ] FK constraints in migrations match Relationships section in Schema

### Sign-Off

- [ ] **Human sign-off** — Conventions, Registry, and Dependency Map approved. Detail Blocks may begin.

Signed: _____________________ Date: ___________

---

## Migration Detail Blocks

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Detail Blocks are filled AFTER Gate 1 is signed off. Do not start Detail Blocks if any of the following are true:
> - Conventions & Tooling has any open `❓ AGENT PAUSE` markers
> - Registry has any rows missing Module IDs or Types
> - Dependency Map has any failed DAG validation checks
> - Gate 1 has not been signed off
>
> If the human asks you to start a Detail Block before Gate 1 is signed off, push back:
>
> > "Detail Blocks reference the Registry, Dependency Map, and Conventions. If any of those change after Detail Blocks are written, every affected block needs rewriting. Let's finish Gate 1 first — should take 10 minutes if Conventions and Registry are solid."
>
> **Your job:** Produce one full specification per migration. A coding agent (or DBA) must be able to apply this single migration by reading only the Detail Block + the Conventions section, without re-deriving anything from Schema or Module Breakdown.
>
> **A complete Detail Block covers ALL of the following 11 sub-sections — no skipping any one:**
> 1. **Header** — DB-XX · filename · Type — exactly matches Registry row
> 2. **Status / Module / Reversible** — three single-line facts
> 3. **Purpose** — one paragraph: what this migration accomplishes and which feature needs it
> 4. **Depends On / Required By** — two small tables; mirrors Dependency Map
> 5. **Schema Changes** — table of operations (CREATE / ALTER / DROP) on Schema objects
> 6. **SQL (Up)** — the migration SQL itself, ready to run
> 7. **SQL (Down / Rollback)** — the rollback SQL, OR an explicit "Non-reversible — restore from backup" with rationale
> 8. **Zero-Downtime Pattern Used** — names the pattern (1–5) if applicable, or "N/A — new table" / "N/A — dev-only"
> 9. **Pre-Apply Checks** — checklist of preconditions before running
> 10. **Post-Apply Checks** — checklist of verifications after running
> 11. **Rollback Procedure + Notes / Risks** — step-by-step rollback (even if "restore from backup"); known risks
>
> **Incomplete looks like:**
> - "Rollback: drop the table" without the actual SQL
> - "Pre-apply: confirm previous migration applied" without specifying which migration
> - "Post-apply: verify it worked" without specifying what verifies it
> - A destructive migration with a Down section claiming reversibility (impossible — flag it)
> - SQL referencing a table or column not in Schema (Schema gap — flag it)
>
> **Reversibility check (mandatory for every Detail Block):**
> - If Type is Drop Column / Drop Table: SQL (Down) must say "Non-reversible — restore from backup". No exceptions. The Down section explains WHY (data loss) and points to the backup procedure.
> - If Type is Backfill: SQL (Down) must explain the manual reversal procedure or state "Data-dependent — see Rollback Procedure".
> - If Type is anything else: SQL (Down) is real, runnable SQL that restores the pre-migration schema state.
>
> **Cross-reference checklist (verify before declaring each block done):**
> - Header DB-XX matches Registry row
> - Filename matches Registry Version + Migration Name
> - Type matches Registry Type
> - Module matches Registry Module
> - Depends On matches Registry Depends On
> - Reversible matches Registry Reversible
> - Schema objects referenced (tables, columns, indexes, constraints) all exist in `[AppName]_DB_Schema.md`
> - If migration touches an existing table, Zero-Downtime Pattern Used is filled (not blank)
>
> **Ask triggers:**
> - SQL would require an extension not declared in Tech Spec → Tech Stack → ask before assuming the extension is available
> - Rollback is theoretically possible but risky (e.g., changing column type back loses precision) → ask before deciding the migration is "Reversible"
> - A migration creates an index on a column not in Schema → ask before assuming Schema is wrong
>
> **Remove this block before delivering the filled doc.**

> One block per migration. Full specification for applying and reversing it. Find any migration by searching its DB-XX ID.

---

### DB-01 · `V001__create_users_table` · Create

**Status:** 🔲 Not Started
**Module:** M-01
**Reversible:** Yes — drop table (only safe before production data exists)

---

**Purpose**

Creates the core `users` table. All other tables with a user relationship depend on this existing first. Required by Module M-01 (Foundation) — every authenticated workflow in the app reads from this table.

---

**Depends On**

| Migration ID | Migration Name | Why |
|--------------|---------------|-----|
| — | — | No dependencies |

**Required By**

| Migration ID | Migration Name | What they need |
|--------------|---------------|---------------|
| DB-02 | `create_projects_table` | FK → users.id |
| DB-05 | `seed_default_roles` | Needs users table to exist |

---

**Schema Changes**

> Reference `[AppName]_DB_Schema.md` § Entities → users for full field definitions.

| Operation | Object | Details |
|-----------|--------|---------|
| CREATE TABLE | `users` | id, email, name, password_hash, status, created_at, updated_at, deleted_at |
| ADD CONSTRAINT | `users.email` | UNIQUE |
| ADD INDEX | `users.created_at` | Range queries |

---

**SQL (Up)**

```sql
CREATE TABLE users (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email         VARCHAR(255) NOT NULL UNIQUE,
  name          VARCHAR(255) NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  status        VARCHAR(20) NOT NULL DEFAULT 'active',
  created_at    TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at    TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  deleted_at    TIMESTAMPTZ
);

CREATE INDEX idx_users_created_at ON users (created_at);
```

**SQL (Down / Rollback)**

```sql
DROP TABLE IF EXISTS users;
```

> ⚠️ Rolling back this migration drops all user data. Only safe before production data exists. After production launch, this migration becomes effectively non-reversible — see Rollback Procedure below.

---

**Zero-Downtime Pattern Used**

N/A — new table. No existing data, no existing traffic. Standard `CREATE TABLE` is safe.

---

**Pre-Apply Checks**

- [ ] Table `users` does not already exist (`SELECT to_regclass('users');` returns NULL)
- [ ] No existing migrations conflict with this schema
- [ ] `gen_random_uuid()` extension is available (`CREATE EXTENSION IF NOT EXISTS pgcrypto;` has been run)

**Post-Apply Checks**

- [ ] `users` table exists with correct columns (`\d users` shows all 8 columns)
- [ ] UNIQUE constraint on `email` is active (`SELECT conname FROM pg_constraint WHERE conname LIKE '%email%';`)
- [ ] Index on `created_at` is present (`\di idx_users_created_at`)
- [ ] Can insert a test row and retrieve it
- [ ] Soft delete field `deleted_at` defaults to NULL

---

**Rollback Procedure**

**Pre-Production (no user data exists):**
1. Confirm no dependent migrations have been applied (DB-02, DB-05 are 🔲 Not Started)
2. Run SQL (Down) above
3. Verify table no longer exists (`SELECT to_regclass('users');` returns NULL)
4. Mark status ↩️ Rolled Back in Environment Status table

**Post-Production (user data exists):**
1. Do NOT run SQL (Down) — data loss is unrecoverable from rollback SQL alone
2. Restore from most recent verified backup
3. Replay subsequent migrations from the backup point forward
4. Mark this migration ↩️ Rolled Back; record the restore point in Notes

**Notes / Risks**

- `gen_random_uuid()` requires `pgcrypto` extension. Enable it before running if not already active: `CREATE EXTENSION IF NOT EXISTS pgcrypto;`
- Rollback after production launch requires backup restore. There is no way to roll back this migration without that.

---

### DB-02 · `V002__create_projects_table` · Create

**Status:** 🔲 Not Started
**Module:** M-03
**Reversible:** Yes — drop table (only safe before production data exists)

---

**Purpose**

Creates the `projects` table. Belongs to a user via FK. Tasks and other project-scoped entities depend on this existing first. Required by Module M-03 (Projects) — the central feature of the app.

---

**Depends On**

| Migration ID | Migration Name | Why |
|--------------|---------------|-----|
| DB-01 | `create_users_table` | FK → users.id |

**Required By**

| Migration ID | Migration Name | What they need |
|--------------|---------------|---------------|
| DB-03 | `create_tasks_table` | FK → projects.id |
| DB-04 | `add_index_user_id_on_projects` | Table must exist |

---

**Schema Changes**

> Reference `[AppName]_DB_Schema.md` § Entities → projects for full field definitions.

| Operation | Object | Details |
|-----------|--------|---------|
| CREATE TABLE | `projects` | id, user_id, name, description, status, created_at, updated_at, deleted_at |
| ADD CONSTRAINT | `projects.user_id` | FK → users.id, ON DELETE RESTRICT |
| ADD INDEX | `projects.user_id` | Queries by user |
| ADD INDEX | `projects.status` | Filtering |

---

**SQL (Up)**

```sql
CREATE TABLE projects (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id     UUID NOT NULL REFERENCES users(id) ON DELETE RESTRICT,
  name        VARCHAR(255) NOT NULL,
  description TEXT,
  status      VARCHAR(20) NOT NULL DEFAULT 'active',
  created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  deleted_at  TIMESTAMPTZ
);

CREATE INDEX idx_projects_user_id ON projects (user_id);
CREATE INDEX idx_projects_status ON projects (status);
```

**SQL (Down / Rollback)**

```sql
DROP TABLE IF EXISTS projects;
```

---

**Zero-Downtime Pattern Used**

N/A — new table. No existing data, no existing traffic.

---

**Pre-Apply Checks**

- [ ] DB-01 has been applied successfully in this environment
- [ ] Table `projects` does not already exist (`SELECT to_regclass('projects');` returns NULL)

**Post-Apply Checks**

- [ ] `projects` table exists with correct columns
- [ ] FK constraint on `user_id` is active (`SELECT conname FROM pg_constraint WHERE conrelid = 'projects'::regclass;`)
- [ ] Indexes on `user_id` and `status` are present
- [ ] Cannot insert a project with a non-existent `user_id` (FK enforced — test with bogus UUID, should fail)

---

**Rollback Procedure**

**Pre-Production:**
1. Confirm DB-03 and DB-04 have not been applied
2. Run SQL (Down) above
3. Verify table no longer exists
4. Mark status ↩️ Rolled Back

**Post-Production:**
1. Restore from backup (see DB-01 Rollback Procedure → Post-Production)

**Notes / Risks**

- ON DELETE RESTRICT means deleting a user who has projects will fail at the DB layer. Intended — application code must clean up projects before deleting the user, or use soft-delete via `deleted_at`.

---

*(Continue pattern for DB-03, DB-04, DB-05, etc. Every Detail Block must include all 11 sub-sections from the agent instructions above.)*

---

## Rollback Procedures

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define the global rollback rules that apply across all migrations. Per-migration rollback SQL lives in each Detail Block — this section is the decision framework that tells someone what to do when a migration goes wrong.
>
> **A complete Rollback Procedures section covers:**
> - When to roll back (decision table by scenario)
> - Rollback decision tree (the algorithm a DBA or coding agent follows under pressure)
> - Rollback order rule (reverse dependency order — always)
> - Backup requirements for destructive migrations (what must be confirmed before running)
>
> **Do not edit the four sub-sections below per project unless the project has a genuinely different rollback policy.** They are battle-tested defaults. Project-specific overrides go in Notes/Risks of the individual Detail Block, not here.
>
> **Remove this block before delivering the filled doc.**

> Global rollback rules and decision tree. Per-migration rollback SQL lives in each Detail Block above.

### When to Roll Back

| Scenario | Action |
|----------|--------|
| Migration failed mid-run | Roll back to last clean state. Fix migration as a NEW version. Re-apply. |
| Migration succeeded but app is broken | Roll back migration. Root cause the issue before re-applying. |
| Destructive migration applied incorrectly | Restore from backup. Rollback SQL cannot recover lost data. |
| Bad data backfill | Manual correction or restore from backup. Backfill rollback is data-dependent. |

### Rollback Decision Tree

```
Migration failed?
├── Yes → Did it partially apply?
│         ├── Yes → Manual cleanup required. Assess row-by-row. Document in Build Decisions Log.
│         └── No  → Run SQL (Down) from Detail Block. Write fix as NEW version (never reuse).
└── No  → App broken after migration?
          ├── Yes → Is it reversible? (See Reversible column in Registry)
          │         ├── Yes → Run SQL (Down) from Detail Block. Fix root cause. Re-apply as new version.
          │         └── No  → Restore from backup. No other option.
          └── No  → All clear. Update Environment Status with timestamp and applier.
```

### Rollback Order Rule

> **Always roll back in reverse dependency order.** If DB-03 depends on DB-02, roll back DB-03 before DB-02.

This rule is non-negotiable. Rolling back DB-02 before DB-03 leaves dangling FK references and a corrupted DB state.

### Backups Before Destructive Migrations

Before any **Drop Column**, **Drop Table**, or **Change Type** (narrowing) migration in staging or production:

- [ ] Backup confirmed and tested (can restore from it — not just confirmed it exists)
- [ ] Backup timestamp recorded in Detail Block Notes
- [ ] Rollback window defined (how long before we call the change permanent)
- [ ] Point-of-no-return confirmed with team before applying
- [ ] Backup retention policy verified (backup must outlast the rollback window)

---

## Environment Status

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Track which migrations have been applied in which environment. This table is the source of truth for "what's the state of Staging right now?"
>
> **A complete Environment Status section covers:**
> - One sub-table per environment declared in Conventions → Environment Promotion Rule
> - One row per migration in each table, ordered by version number
> - Status, Applied At, Applied By, and Notes columns
> - Initial state: all migrations 🔲 Not Started in all environments
>
> **Update rules during build:**
> - When a migration is applied in an environment, update its row in that environment's table with the timestamp and applier
> - When a migration fails, mark 🚫 Failed and note the error in Notes
> - When a migration is rolled back, mark ↩️ Rolled Back; do NOT delete the row — the audit trail matters
>
> **Remove this block before delivering the filled doc.**

> Track applied migrations per environment. Update after each run.

### Development

| Migration ID | Version | Status | Applied At | Applied By | Notes |
|--------------|---------|--------|-----------|------------|-------|
| DB-01 | V001 | 🔲 | — | — | — |
| DB-02 | V002 | 🔲 | — | — | — |
| DB-03 | V003 | 🔲 | — | — | — |
| DB-04 | V004 | 🔲 | — | — | — |
| DB-05 | V005 | 🔲 | — | — | — |

### Staging

| Migration ID | Version | Status | Applied At | Applied By | Notes |
|--------------|---------|--------|-----------|------------|-------|
| DB-01 | V001 | 🔲 | — | — | — |
| DB-02 | V002 | 🔲 | — | — | — |
| DB-03 | V003 | 🔲 | — | — | — |
| DB-04 | V004 | 🔲 | — | — | — |
| DB-05 | V005 | 🔲 | — | — | — |

### Production

| Migration ID | Version | Status | Applied At | Applied By | Notes |
|--------------|---------|--------|-----------|------------|-------|
| DB-01 | V001 | 🔲 | — | — | — |
| DB-02 | V002 | 🔲 | — | — | — |
| DB-03 | V003 | 🔲 | — | — | — |
| DB-04 | V004 | 🔲 | — | — | — |
| DB-05 | V005 | 🔲 | — | — | — |

---

## Pre-Migration Checklist (Run Before Any Migration)

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** This is an operational checklist run by whoever applies migrations. Do not modify the checklist per migration — it applies universally. Project-specific additions can go at the end, but the standard items remain.
>
> **Remove this block before delivering the filled doc.**

> Run this before applying migrations in any environment. Staging and Production require every box checked.

### Always

- [ ] Source doc (`[AppName]_DB_Schema.md`) reviewed — migration matches schema intent
- [ ] Dependency order confirmed — all upstream migrations are ✅ Applied in this environment
- [ ] SQL (Up) reviewed by at least one person other than the author (staging/prod)
- [ ] SQL (Down) exists and has been tested in dev (or marked "Non-reversible — restore from backup" with backup confirmed)
- [ ] No active long-running transactions or locks on affected tables

### For Staging / Production

- [ ] Maintenance window scheduled if migration requires table lock
- [ ] Estimated run time assessed — long migrations on large tables need downtime planning
- [ ] Zero-Downtime Pattern Used field in Detail Block is filled (not blank) if migration touches an existing table
- [ ] Rollback plan confirmed (who does it, how long it takes, what the success criteria are)
- [ ] Team notified before applying

### For Destructive Migrations (Drop Column / Drop Table / Narrowing Change Type)

- [ ] Backup confirmed and tested (verified it can be restored, not just verified it exists)
- [ ] Application code no longer references the column/table being dropped
- [ ] Deployed code change has been live for at least [X] hours with no errors
- [ ] Business sign-off recorded in Build Decisions Log

---

## Post-Migration Checklist (Run After Each Migration)

- [ ] Migration tool confirms success (no error output)
- [ ] Post-Apply Checks from Detail Block completed
- [ ] Application tested against this environment (smoke test passes)
- [ ] No new errors in logs for [X] minutes after apply
- [ ] Environment Status table updated with timestamp and applier
- [ ] If migration introduced any deviation from Detail Block (slower than expected, additional manual step, etc.), entry logged in Build Decisions Log

---

## Open Questions

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Track any open question that blocks a migration from being marked ready to apply. One row per question. Resolve before Gate 2.
>
> **A complete Open Questions table:**
> - Has zero rows when this doc reaches Gate 2 (or all rows are resolved and the question/resolution is preserved for record)
> - Each row names the specific migration(s) affected
> - Each row has an owner (Ryan / Claude / TBD)
> - Each row has a "Needed By" — typically a phase or specific date
>
> **Remove this block before delivering the filled doc.**

| Question | Affects Migration(s) | Priority | Owner | Needed By | Resolution |
|----------|---------------------|----------|-------|-----------|------------|
| [Question] | [DB-XX] | High / Med / Low | [Ryan / Claude / TBD] | [Phase or date] | [Empty until resolved] |

---

## 🚦 Gate 2 — Full Migration Checklist Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Why this gate matters:** This is the last gate before this doc is consumed by downstream coding-phase docs (Deployment Config consumes the Registry order for CI pipeline; Module Breakdown closes the bidirectional link; Build Decisions Log will reference DB-XX IDs during build). Errors here cascade into every downstream doc.
>
> **Human sign-off is required.** Do not declare this doc Done without explicit human approval.
>
> **Gate procedure:**
> 1. Walk every checklist item. For each, open the relevant section and verify — do not check from memory.
> 2. Re-verify the bidirectional Module ID link by opening Module Breakdown. Drift since Gate 1 is common.
> 3. Confirm every Detail Block contains all 11 sub-sections. Skipped sub-sections are a fatal gap.
> 4. If any check fails, flag to the human and resolve before declaring Done.
>
> **Remove this instruction block before delivering. Keep the checklist and sign-off line.**

### Completeness

- [ ] Overview is filled with app name, DB engine, migration tool, total count, source docs, and migration story
- [ ] Migration Type Reference is reviewed (project-specific additions appended if needed)
- [ ] Zero-Downtime Migration Patterns is unmodified (or has documented Pattern 6+ additions)
- [ ] Conventions & Tooling is fully filled — no `❓ AGENT PAUSE` markers remain
- [ ] Migration Registry has one row per migration, no gaps in version numbers
- [ ] Dependency Map covers every Registry row, with no cycles, self-references, or orphans
- [ ] Gate 1 was signed off before Detail Blocks began
- [ ] Every migration in Registry has a corresponding Detail Block
- [ ] Every Detail Block contains all 11 sub-sections
- [ ] Rollback Procedures section is unmodified (or has documented project-specific overrides)
- [ ] Environment Status table has one row per migration in each environment
- [ ] Pre-Migration / Post-Migration Checklists are present and unmodified
- [ ] Open Questions table has zero blocking entries (or all are resolved)

### Cross-Doc Consistency

- [ ] **Module ↔ Migration bidirectional link** verified by opening Module Breakdown for every Module ID in this Registry and confirming the Module's Detail Block lists the corresponding DB-XX in its Migrations column. Confirmed in reverse: every DB-XX in any Module Detail Block's Migrations column exists in this Registry with the matching Module ID.
- [ ] **Schema cross-reference** verified — every Schema entity has at least one Create migration; every Schema field has at least one Add Column or Create migration; every Schema FK has at least one Add FK or Create migration with REFERENCES.
- [ ] **Tech Spec cross-reference** verified — migration tool matches Tech Spec → Tech Stack; environment list matches Tech Spec → Deployment & Environments; required extensions (e.g., `pgcrypto`) are declared in Tech Spec.

### Reversibility Discipline

- [ ] Every Detail Block has a Reversible field with one of three values: "Yes — [how]", "Reversible with caveats — see Notes/Risks", "Non-reversible — restore from backup"
- [ ] Every destructive migration (Drop Column, Drop Table) is marked "Non-reversible — restore from backup" with rationale in the Down section
- [ ] Every backfill migration has an explicit manual reversal procedure or "Data-dependent — see Rollback Procedure"

### Zero-Downtime Discipline

- [ ] Every migration touching an existing table has a "Zero-Downtime Pattern Used" field filled (named pattern OR explicit "N/A — dev-only" with rationale)
- [ ] Every migration adding a NOT NULL column has been split into the expand/backfill/constrain pattern (Pattern 1)
- [ ] Every migration adding an index on a populated table uses `CREATE INDEX CONCURRENTLY` (Pattern 2)
- [ ] Every column rename follows the multi-deploy pattern (Pattern 3)
- [ ] Every column drop follows the deploy-gap pattern (Pattern 4)

### Cleanup Verification

- [ ] Searched the file for `🤖` — zero hits
- [ ] Searched the file for `❓ AGENT PAUSE` — zero hits
- [ ] Searched the file for "Remove this block" — zero hits
- [ ] Every `🚦 GATE` block contains only its checklist and sign-off line — no agent prose

### Sign-Off

- [ ] **Human sign-off** — Migration Checklist is complete, internally consistent, cross-doc consistent, and ready to drive build-phase migration work and downstream Deployment Config CI pipeline.

Signed: _____________________ Date: ___________
