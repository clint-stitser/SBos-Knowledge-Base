# MS Project-Style Scheduling Engine Scope

**Status:** Decided in conversation, not yet built
**Context:** Design discussion for the Supabase master task table that powers checklists, Gantt-style scheduling views, and cross-project linkage across S-BOS — extended to scope a full MS Project-style scheduling engine, then extended again to scope budget enrichment on the same table.

---

## 1. One table for tasks, checklist items, and schedule items

A checklist item is a task with scheduling fields left null. Do not split into
separate `tasks` and `schedule_tasks` tables — that creates a sync/promotion
problem every time a checklist item needs to become a scheduled task.

Single `tasks` table with nullable scheduling columns:
- `duration`
- `start_date`, `end_date`
- `scheduling_mode` (`manual` | `auto`)
- `constraint_type` (e.g. ASAP, must-start-on) — see Section 5 for full scope
- `constraint_date`

A plain checklist is just the degenerate case where these are empty. Gantt
views filter/render accordingly.

## 2. Predecessors/successors are a relationship table, not columns

Do not put `predecessor_id` on the task row — a task can have multiple
predecessors, each with its own type and lag.

```
task_dependencies (
  id,
  predecessor_task_id,
  successor_task_id,
  dependency_type,   -- FS / SS / FF / SF
  lag_days           -- negative = lead time
)
```

This table is also the mechanism used for cross-project linkage (see below) —
no separate structure needed.

## 3. Cross-project task visibility: parent/child (e.g. subdivision) projects

**Scenario:** A task at the child-project level needs to be visible on the
parent project's schedule, especially when different teams work each level.

Two options were considered:

### Option A — Shared row via junction table (rejected as default)
```
task_projects (
  task_id, project_id,
  role,   -- "primary" | "reference"
  display_assignee_id
)
```
One task, same row, referenced by both projects. Requires permission gating
(`role`) to prevent the parent-project team from editing a task the child
team owns. Reserved only for the rare case where it's genuinely the *same
accountable item* with two owners tracking it identically.

### Option B — Linked milestone via task_dependencies (chosen approach)
- Child project keeps its own detailed task, owned and edited only there.
- Parent project gets its own milestone-type task (e.g. "Subdivision grading
  complete"), visually distinct (different color/icon) on its Gantt.
- The two are connected via the existing `task_dependencies` table
  (`predecessor_task_id` / `successor_task_id`), same mechanism as any other
  dependency — just crossing project boundaries.
- The parent milestone is `scheduling_mode = auto`: a Postgres trigger
  recalculates its date when the child task's date changes. This is a nudge/
  sync of a linked row, not a live mirror — the parent milestone is its own
  row with its own data.
- Read-only from the parent side; no permission gating needed since ownership
  never crosses project lines.

**Decision: Option B is the default pattern.** Simpler, no dual-ownership
ambiguity, no new data model — reuses `task_dependencies` as-is. Use only for
the subset of child tasks the parent actually needs as a milestone; most
child-project tasks stay invisible to the parent and the parent Gantt stays
clean.

Option A stays available but should be the exception, not the default, given
the added complexity of shared ownership and permissions.

## 4. Implementation notes for v1 (basic Gantt viewer)

- Trigger logic can be simple: on `tasks.end_date` update, find any tasks
  where this task is their `predecessor_task_id` via `task_dependencies`,
  and if that task is `scheduling_mode = auto`, recalculate its date. No
  queueing system needed for v1.
- Visually distinguish cross-project milestone tasks in the Gantt UI so
  users immediately understand "this bar reflects work happening elsewhere."

---

## 5. Scope for a full MS Project-style scheduling engine

**Decision: still the same `tasks` table.** An MS Project-style engine
doesn't need a different set of tasks — it needs more metadata per task and
more logic sitting on top of the same rows. Splitting it into a second table
would duplicate task identity (name, status, assignee, project link) and
require syncing two records for one real-world thing — the same problem
avoided in Section 1.

### 5.1 Additional columns on `tasks` (nullable, ignored by plain checklist items)

- `constraint_type` — ASAP, ALAP, Must Start On, Start No Earlier Than, etc.
- `constraint_date`
- `is_critical` — boolean flag, computed/cached from the critical path calculation
- `is_milestone` — zero-duration marker
- `baseline_start`, `baseline_end` — snapshot for variance tracking against the live schedule
- `percent_complete` — tracked distinctly from status, as in MS Project

### 5.2 New supporting tables (separate from `tasks`, not replacing it)

- `task_resource_assignments` — links tasks to people/resources with
  allocation % or units, enabling overallocation detection. This is genuinely
  new; nothing in the current model captures how much of a person's time a
  task consumes.
- `schedule_baselines` — if more than one saved baseline over time is needed
  (MS Project supports up to 11), store snapshots here rather than adding
  multiple baseline-date-pairs directly to the task row.

### 5.3 Compute layer (logic, not storage)

- **Critical path calculation** — forward/backward pass over
  `task_dependencies` to identify which chain of tasks drives the project
  end date. Runs as a function/service, reads `tasks` + `task_dependencies`,
  writes back `is_critical` flags.
- **Cascading recalculation** — when a date shifts, walk the entire
  dependent chain (not just one downstream task), handling cycles/conflicts
  gracefully.
- **Interactive Gantt behavior** — drag-to-reschedule, drag-to-resize
  duration, and defined behavior for dependents when a user does this
  manually (auto-shift everything downstream vs. warn and ask). Primarily a
  frontend concern, but it drives backend requirements.

### 5.4 Net scope statement

Same `tasks` table, more columns on it, one or two new supporting tables for
resources/baselines, plus a scheduling engine (critical path + cascading
recalculation) that reads/writes those tables. No fork of the task model
itself. This is a meaningfully larger build than a basic Gantt viewer —
closer to a small scheduling engine — and should be scoped as its own
build-doc phase rather than an incremental add to the master task table work.

---

## 6. Budget enrichment on the same `tasks` table

**Premise:** In underwriting, a project is made up of phases, and phases are
made up of items/tasks. Every task therefore has both a duration (schedule)
and a cost (budget). Since duration/dates were successfully added as
enrichment columns on `tasks` (Sections 1 and 5), the same principle was
tested against budget.

**The complication:** Unlike duration, budget line items and schedule tasks
don't map cleanly 1:1. A single budget line item (e.g. "Framing," $50,000)
is often executed across multiple tasks that happen at different times
("Frame walls" in week 3, "Frame roof" in week 4, "Framing punch list" in
week 6). Less commonly, one task pulls from multiple line items happening
at the same time (a "rough-in" task touching electrical, plumbing, and HVAC
budget simultaneously). This is the same many-to-many mismatch already
solved for predecessors in Section 2 — a relationship table, not a column,
is the correct shape.

### 6.1 Two options considered

**Option 1 — Track actual dollars (spent/committed) at the budget-line-item
level only.** Tasks link to line items to show *when* portions of the
budget are expected to land on the calendar (a time-phased forecast), but
real invoices and commitments are recorded against the line item as a whole,
not split per task.

**Option 2 — Track actual dollars per task.** Every invoice or committed
cost would need to be broken down and attributed to the specific task it
paid for (e.g., how much of the Framing invoice was for the roof
specifically). This gives sharper per-task cost accuracy but requires cost
data precise enough to support that split.

### 6.2 Decision: Option 1

**Subcontractor estimates and invoices don't come in split by task** — a
sub bills for "Framing," not for "the roof portion of Framing." Requiring
per-task actuals would force a level of cost-data precision that doesn't
exist at the source, so Option 1 is the correct fit today.

- **Budgeted amounts** can still be allocated across tasks (for time-phased
  forecasting — how much budget is expected to be consumed in which week).
- **Actual and committed amounts** are tracked and recorded at the budget
  line item level only.

**Revisit trigger:** If the estimating team builds out quantity take-off
capacity and chooses to group/split line items at that level of detail,
this decision should be revisited — that would be the point where cost
data becomes precise enough to justify per-task actuals (Option 2).

### 6.3 Data shape

```
budget_line_items (
  id, project_id, name, cost_code,
  budgeted_amount, committed_amount, actual_amount
)

task_budget_allocations (
  id, task_id, budget_line_item_id,
  allocated_amount   -- portion of the line item's BUDGETED amount tied to this task
)
```

- Simple case (task = line item): one allocation row, full budgeted amount.
- Split case (one line item, three tasks over time): three allocation rows,
  same `budget_line_item_id`, `allocated_amount`s summing to the line item's
  budgeted total.
- Committed/actual dollars are recorded directly on `budget_line_items`, not
  on the allocation rows — per the Section 6.2 decision.

### 6.4 Downstream benefit (noted, not scoped for v1)

Because tasks already carry `start_date`/`end_date`, summing
`allocated_amount` grouped by task schedule dates produces a time-phased
spend curve (budgeted cash flow by week/month) without any additional date
fields on the budget side. This could eventually feed the GP Cash Flow
Schedule as a computed source rather than a hand-curated one — flagged as a
future integration point, not a v1 requirement.
