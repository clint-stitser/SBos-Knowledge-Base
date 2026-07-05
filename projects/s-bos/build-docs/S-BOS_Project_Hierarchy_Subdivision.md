# S-BOS 2.0 — Project Hierarchy & Subdivision

**Doc class:** design · **Captured:** 2026-07-04 · **Status:** built (migration 039)

## Decision
Projects support arbitrary-depth parent → child → grandchild hierarchy via a self-reference, so a subdivided parcel becomes a parent project and each resulting lot/phase is a child with its **own** gates, budget, schedule, and ladder. Children inherit nothing automatically — each carries its own data.

## Data model
- `projects.parent_project_id` → `projects(id)` (self-ref, `on delete set null`).
- `v_project_tree` — recursive CTE giving `depth`, `root_id`, and a materialized `path` (e.g. *Big Parcel › Phase 1 › Lot 4*).

## UI
- **Subdivide** action on the Project Ladder: creates N child lots, each inheriting company/department and getting a fresh copy of the parent's decision-gate ladder. Named `<prefix> — Lot N`; numbering continues past existing children.
- **Projects list grouping**: children nest under their parent with a +/− toggle and a child-count badge; collapsed by default so a large subdivision doesn't flood the list; recursive; search force-expands and keeps a parent visible if any descendant matches.

## Reuse
The same hierarchy carries the **brokerage** file type: an Agency Contract is a parent project, each Property Contract a child (see Brokerage Economics article).
