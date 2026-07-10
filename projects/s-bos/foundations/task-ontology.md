# From Ideas to Goals: the bottom-up ontology

*S-BOS Foundations · Article 2 of 5*

---

S-BOS isn't a pile of tables that happened to accumulate. Every table serves a
rung on one ladder that runs from a raw idea at the bottom to a measured goal
at the top. This article walks that ladder and maps each rung to the actual
S-BOS tables, so when you look at the schema you see a philosophy, not a
junk drawer.

## The bottom: ideas become skills

It starts with **ideas** — thoughts, paradigms, things noticed on a job site
or in a spreadsheet. Ideas accumulate into **knowledge**.

Knowledge alone doesn't do work. Work takes a skill, and a skill has a
recipe:

```
ideas (thoughts, paradigms)  →  accumulate into KNOWLEDGE

KNOWLEDGE
  + a TOOL       (screw gun, spreadsheet, formula, code surface)
  + a TECHNIQUE  (knowing how to swing it)
  ────────────────────────────────────────
  = a SKILL
```

A tool without a technique isn't a skill yet — that's why every tool in the
[Tool Library](tool-library.md) is expected to have its technique documented
in the knowledge base.

## The middle: tasks and what they consume

A **task** is the atomic unit of execution. Every task needs three answers:

- **Who** is doing it
- **When** it happens
- **How** it gets done — i.e., which skill is applied

And applying a skill to a task consumes two things: **cost** and **time**.
That is the entire resource model. Everything financial in S-BOS is
ultimately a roll-up of cost consumed by tasks; everything schedule-related
is a roll-up of time.

## The roll-up: tasks to goals

```
tasks ──→ checklists · schedules · budgets
              │
              ▼
          projects
              │
              ▼
     priorities & phases
              │
              ▼
           goals
              │
   execution throws off STATISTICS;
   goals are measured by their progress
```

- Tasks group into **checklists** (what), **schedules** (when), and
  **budgets** (cost).
- Those bundles belong to **projects** — the unit a vertical department
  carries dirt → done.
- Projects serve **priorities and phases** — the strategic layer.
- Priorities ladder up to **goals**.
- And execution at every level throws off **statistics** — the numbers that
  tell you whether the goals are actually being reached.

## The table map

Every rung has a home in the S-BOS schema:

| Rung | S-BOS tables |
|---|---|
| Knowledge | `kb_entries` (plus this knowledge base) |
| Task — who / when / what | `tasks`, `check_lists` |
| Cost consumed | `project_budget_items` (and the `uw_*` underwriting tables) |
| Time consumed | `project_dates`, `time_cards` |
| Projects | `projects` |
| Priorities & phases | `priorities` |
| Goals | `goals` |
| Statistics | `stat_logs`, `project_health_reports` |

When a new feature is proposed, the first question is: *which rung does it
serve?* If it doesn't map to a rung, it probably doesn't belong.

## Why bottom-up matters

Most systems are designed top-down: someone declares the reports they want,
then forces data entry to feed them. S-BOS goes the other way. Get the atomic
unit right — a task with a who, a when, and a how, consuming cost and time —
and the checklists, budgets, schedules, project health, and goal progress all
fall out as roll-ups instead of separate data-entry chores.

That's also why statistics live at the top of the ladder rather than the
side: they aren't a reporting afterthought, they're the exhaust of execution,
and goals are measured by them.

---

*Previous: [The Grid](the-grid.md) · Next: [The Tool Library](tool-library.md)*
