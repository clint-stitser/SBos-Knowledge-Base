# Master Task Table — Scheduling & Cross-Project Linkage Design Decisions

**Status:** Decided in conversation, not yet built
**Context:** Design discussion for the Supabase master task table that powers both checklists and Gantt-style scheduling views across S-BOS.

---

## 1. One table for tasks, checklist items, and schedule items

A checklist item is a task with scheduling fields left null. Do not split into
separate `tasks` and `schedule_tasks` tables — that creates a sync/promotion
problem every time a checklist item needs to become a scheduled task.

Single `tasks` table with nullable scheduling columns:
- `duration`
- `start_date`, `end_date`
- `scheduling_mode` (`manual` | `auto`)
- `constraint_type` (e.g. ASAP, must-start-on) — optional, for future CPM-style auto-scheduling
- `constraint_date` — optional

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

## 4. Implementation notes for v1

- Trigger logic can be simple: on `tasks.end_date` update, find any tasks
  where this task is their `predecessor_task_id` via `task_dependencies`,
  and if that task is `scheduling_mode = auto`, recalculate its date. No
  queueing system needed for v1.
- Visually distinguish cross-project milestone tasks in the Gantt UI so
  users immediately understand "this bar reflects work happening elsewhere."
