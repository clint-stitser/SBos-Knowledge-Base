# S-BOS 2.0 — Custom Rollups (Owner-Relevant Multi-Entity Views)

**Doc class:** design · **Captured:** 2026-07-04 · **Status:** built (migration 044)

## Problem
In a multi-entity business, different owners/members have different ownership across companies (e.g. one owns Realm 100%, another owns Stitser Properties 100%). Each needs a dashboard scoped to *their* slice — and this must be **universal/config-driven** so any licensee's user can define their own view without code changes.

## Decision
A **saved rollup** = a named selection of **companies and/or departments**, owned by a person, scoped as a **union** ("pull Stitser Properties **and** the Disposition department"). Zero Stitser-specific logic — seed data only.

## Data model
- `rollup_views` (name, description, owner_person_id)
- `rollup_view_companies`, `rollup_view_departments` (the selection)
- `v_rollup_projects` — resolver: a project is in scope if its `company_id` is selected **or** its `department_id` is selected
- `v_rollup_summary` — per-rollup project count, brokerage pipeline commission, SP profit

## UI
`/portal/rollups`: list saved rollups with live totals; a builder (searchable company picker + department picker + owner); a scoped dashboard (KPIs, stage breakdown, project list with brokerage economics), plus edit-scope and delete.

## Notes
- Brokerage projects were tagged to Stitser Properties so they roll up under it.
- Coverage grows as projects get their company/department set (today ~96/145 have a company).
- Foundation for licensing: settings must let any user define entities, departments, and their own rollups without SQL.
