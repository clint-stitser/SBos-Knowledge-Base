# S-BOS Build Decisions Log

> **Purpose:** Typed record of decisions made during the build phase — Deviation / Workaround / Reconciliation / Concern / Gap / Dependency / Scope creep. Logged immediately when they happen (per the Operating Agreement).
> **Format:** BD-NN · date · type · what · why · impact.

---

## BD-01 · 2026-07-02 · Reconciliation
**What:** In Layer 0 item 4, renamed the live `check_list_tasks` table → `tasks`, and folded its existing `source_type`/`source_id` columns (loan/project provenance added during the Credit Desk work) **into** the new polymorphic `parent_type`/`parent_id` link. Added `'loan'` to the `parent_types` registry.
**Why:** Realizes the "one universal Task table" (PDD §3). `source_type`/`source_id` and `parent_type`/`parent_id` are the same polymorphic-parent concept, so keeping both would duplicate meaning. `'loan'` isn't in today's nominal registry scope (Projects/People/Companies/Budget items/Schedule-Tasks) but the 117 Credit Desk reconciliation tasks were already loan-parented, so the registry row preserves them rather than orphaning.
**Impact:** 1 app read (`sb-crm-projects-list-details/page.tsx`) and 1 script (`build-q2-reconciliation.mjs`) updated to `tasks` + `parent_type`/`parent_id`. Migrations 001/002/012–015 left as historical (a fresh replay renames at 017). Verified: 117 loan-parented tasks folded, **0 orphans**, tsc clean. Migration `017_tasks_and_scheduling.sql`.
