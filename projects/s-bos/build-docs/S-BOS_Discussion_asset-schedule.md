# Discussion: Asset Schedule (Owned Assets, Knowledge Base, Lifecycle & Cost-to-Own)

**Status:** Drafted 2026-07-05 — architecture agreed in conversation; three schema-level sub-decisions still open (marked below). Not yet built. Corresponds to a new **first-class entity** alongside Entity/Category/Project — not a Brain-layer store, not a Master Property variant.

---

## 1. What this is

A generalized register of **owned assets** — anything an Entity owns and cares for over time: a piece of construction equipment, a company vehicle, a tractor, a kid's dirt bike. Same pattern regardless of who owns it or what it is. Three pillars, confirmed in conversation:

1. **Asset Registry** — the thing itself, who owns it, its type, its lifecycle status.
2. **Knowledge Base** — reused infrastructure from the existing Knowledge Library, scoped per-Asset, rolling up to the owning Entity.
3. **Ownership Lifecycle** — maintenance logs/routines, upgrades/replacements, checkout/assignment, and cost-to-own (actual + depreciation/current-value estimate).

## 2. Decisions locked this session

| # | Decision | Notes |
|---|---|---|
| 1 | **Asset is a first-class entity**, not a Master Property variant, not a Brain-layer store. | Explicit — see §3 for the relationship to Master Property. |
| 2 | **Master Property and Asset are separate concepts.** Master Property stays a "market lookup" — historical knowledge against a specific piece of real estate. An Asset *may optionally link* to a Master Property when it sits on/is tied to a specific parcel (e.g., a piece of equipment permanently sited on a property, or an asset that inherits public-record features — acreage, zoning, square footage — from the parcel it's linked to). The link is optional and does not make Asset a sub-type of Property. |
| 3 | **Knowledge Base reuses existing Knowledge Library infrastructure**, scoped per-Asset (`entity_id` + `asset_id`). Rollup to the Entity level is a query filter (`WHERE entity_id = X`), not new rollup logic. |
| 4 | **Ownership/stakeholder links reuse the existing People/Companies tables** — no new contact tables. Same polymorphic pattern already decided for Comments/Tasks. |
| 5 | **A real accounting reference table gets built** — not a stub. Mirrors cost codes / dept codes / account codes from the accounting system of record (Intacct now, QB later) so cost-to-own is queryable inside the platform without asking accounting. Read-only mirror; does not write back to the accounting system. |
| 6 | **Asset is first-class in the Entity tree**, and personal assets (e.g., a kid's dirt bike) belong to that person's **own Entity** — reusing the parent-child Entity tenancy model already built (`S-BOS_Multi-Entity_Tenancy_Model.md`). A minor child's Entity sits as a sub-entity under the family Entity; visibility flows down per the existing Rule 1 (parent sees child assets; siblings don't see each other's). No new entity-relationship concept needed — this was confirmed as already-solved. |
| 7 | **Checkout/assignment is in scope for v1** — an asset can be checked out to a Project (and/or a person) and checked back in, trackable over time. |
| 8 | **Cost-to-own includes both logged actuals AND a depreciation/current-value estimate** — two numbers, not one. |
| 9 | **Start flexible on asset-type structure** (JSONB custom fields via Blueprint), not a rigid per-type schema, for v1. |
| 10 | **A future Skill** (deep-research an asset type → populate an Asset Blueprint + seed Knowledge Library entries) is the intended path for scaling asset-type coverage — explicitly deferred, flagged so it isn't lost, not part of this build. |

## 3. Relationship to Master Property (clarified)

Master Property ≠ Asset. Master Property is a lens over real estate (what do we know about this address/parcel). Asset is a lifecycle register over owned things. They intersect only via an **optional FK** (`assets.master_property_id`, nullable) — set when an asset is sited on, or inherits public-record features from, a specific parcel. No structural merge; no shared table.

## 4. Proposed schema

```
assets
  id, org_id, entity_id            -- required; owning Entity (business or personal/family sub-entity)
  category_id                      -- nullable; groups assets, can drive blueprint default
  master_property_id               -- nullable FK → master_properties; optional site/feature link (Decision #2)
  blueprint_id                     -- nullable FK → asset_blueprints
  name, asset_type, status          -- status: active | retired | sold | disposed
  acquisition_date, acquisition_cost
  -- depreciation / current-value estimate (Decision #8) — OPEN, see §5.1
  depreciation_method              -- e.g. straight_line | declining_balance | none
  useful_life_years                -- nullable
  salvage_value                    -- nullable
  created_at, updated_at

asset_blueprints
  id, org_id, asset_type_label
  default_knowledge_structure       -- JSONB, v1 flexible (Decision #9)
  default_maintenance_routines      -- JSONB
  custom_fields_schema              -- JSONB

asset_stakeholders                  -- reuses People/Companies (Decision #4)
  id, asset_id, party_type (person|company), party_id, role
  -- role: owner | vendor | manufacturer | mechanic | insurer | seller

asset_events                        -- one table for maintenance/upgrade/replacement/disposal/inspection
  id, asset_id, event_type, event_date
  cost, cost_code                  -- FK → accounting_reference_codes
  performed_by_party_type, performed_by_party_id   -- People/Companies
  linked_task_id                   -- nullable; set when generated from a routine
  notes

asset_maintenance_routines
  id, asset_id, interval_unit, interval_value, last_done_at, next_due_at
  -- generates a `tasks` row when due; reuses existing tasks/task_dependencies engine, no new scheduler

asset_checkouts                     -- Decision #7 — OPEN on exact shape, see §5.2
  id, asset_id, checked_out_to_party_type, checked_out_to_party_id
  project_id                        -- nullable; checkout can be to a Project, a person, or both
  checked_out_at, checked_in_at, notes

accounting_reference_codes          -- Decision #5, real mirror table
  id, entity_id, code_type (cost_code|dept_code|account_code)
  code, description, source_system (intacct|qb), active, last_synced_at
```

**Knowledge Base:** no new table — Asset gets a registry row in the existing Knowledge Library pattern, scoped `entity_id + asset_id`.

**Cost-to-own view (conceptual):**
```
actual_cost_to_own = acquisition_cost + SUM(asset_events.cost WHERE event_type IN (maintenance, upgrade, replacement))
estimated_current_value = f(acquisition_cost, depreciation_method, useful_life_years, salvage_value, age)  -- method TBD, §5.1
```

## 5. Open sub-decisions (not yet locked — need Clint before build)

### 5.1 Depreciation method
Confirmed: Clint wants both actual cost-to-own AND a depreciation/current-value estimate. Not yet decided: which method(s) to support.
- **Option A — Straight-line only (v1 default).** `current_value = acquisition_cost - (acquisition_cost - salvage_value) * (age / useful_life_years)`. Simple, one field set, matches most small-equipment/vehicle use cases.
- **Option B — Straight-line + declining-balance,** selectable per asset via `depreciation_method`. More accurate for equipment that loses value faster early (vehicles, machinery) but doubles the fields/logic to build and test.
- **Option C — External market-value lookup** (e.g., NADA/KBB-style) for asset types where that exists (vehicles), falling back to straight-line elsewhere. Highest accuracy, but a real integration, not a formula — likely a Phase 2 enhancement layered onto whichever of A/B ships first.
- **Recommendation:** ship A now (schema already supports swapping in B/C later since `depreciation_method` is already a column, not a hardcoded formula), revisit B/C once real assets are logged and Clint can see whether straight-line is good enough.

### 5.2 Checkout shape
Confirmed: checkout to a Project is in scope. Not yet decided: can an asset be checked out to a **person only** (no project), and can two things have it checked out at once (unlikely, but worth stating explicitly as "no" if that's the intent), and does checkout enforce a hard "can't check out what's already checked out" rule or just log the history.
- **Recommendation:** `project_id` nullable so checkout can be to a person, a project, or both (e.g., "Mike has the truck for the Cold Creek job"); enforce single-active-checkout per asset (one open `checked_in_at IS NULL` row at a time) as a simple integrity rule rather than leaving it soft — flag if you want it looser.

### 5.3 Blueprint research Skill (deferred, not blocking)
The "deep-research an asset type and populate a Blueprint" Skill (Decision #10) is a real build item for later — needs its own design pass (what sources it searches, how much it pre-fills vs. flags for review, review-gate per the standard Skill workflow). Not scoping it now; noting it so it survives to the next relevant session.

## 6. What this does NOT touch (scope boundary)
- Does not change Master Property's existing behavior or schema.
- Does not add a new Entity-relationship concept — reuses the existing parent-child tenancy model as-is.
- Does not build a new task scheduler — maintenance routines ride the existing `tasks`/`task_dependencies` engine.
- Does not integrate with Intacct/QB for write-back — `accounting_reference_codes` is read-mirror only.

## 7. Next step
Clint decides §5.1 and §5.2 (or defers both to build-time defaults per the recommendations above), then this becomes a Core Entity addition to the PDD (§3) and a Layer-0-style schema build item for the Claude Code session — same pattern as prior build docs in this repo.
