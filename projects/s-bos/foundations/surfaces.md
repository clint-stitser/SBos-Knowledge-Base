# Surfaces: how people reach their work

*S-BOS Foundations · Article 4 of 5*

---

Records and tools are where the work lives. **Surfaces** are how people reach
it. S-BOS deliberately keeps the number of surface archetypes small — two
main ones, matched to the two axes of [the Grid](the-grid.md) — so that once
you've learned your department's screen, you've learned them all. Every
surface is fed by the same underlying tables; nothing is a special snowflake.

## Archetype 1: the vertical dashboard

A vertical department owns projects dirt → done, so its surface is a
**dashboard of projects laid across key situations**, answering three
questions per project at a glance:

- **Current gate** — the first decision gate not yet passed
- **GYR health** — green / yellow / red, from the health reports
- **Next key date** — from `project_dates`

Above the project grid sit the department's **time-bound goals**
(`goals.department_id` + `target_date`) — so the screen shows not just where
every project stands, but what the department has committed to and by when.

```
  ENTRY-LEVEL HOUSING
  ┌─ Goals ──────────────────────────────────────────────┐
  │ ▸ 40 closings by Q4        ▸ G3 on Cold Creek by 8/1 │
  ├─ Projects ───────────────────────────────────────────┤
  │ Project          Gate   Health   Next key date       │
  │ Wilson Landing   G2     ● G      Grading start 7/21  │
  │ Cold Creek       G1     ● Y      Entitlement 8/04    │
  │ Mesa Ridge       G4     ● G      First closing 9/15  │
  └──────────────────────────────────────────────────────┘
```

**Exemplar: the Entry-Level Housing screen** (`/portal/entry-level-housing`).

[screenshot to be added: Entry-Level Housing dashboard]

## Archetype 2: live-in-your-tool (project-driven horizontals)

An estimator doesn't want a dashboard *about* estimating — they want to *be
estimating*. For project-driven horizontals, **the screen IS the tool**:

- The tool itself fills the screen (the estimate, the pro forma).
- A **project dropdown** at the top is scoped to the department's **gate
  window** — Estimating only sees projects currently in G0–G3, across every
  vertical. When a project passes G3, it drops out of the list. The hand-off
  is built into the screen.
- Alongside sits a **deliverables pipeline**: projects × gate × due date — a
  work queue of what this department owes, to which project, at which gate,
  by when.

```
  ESTIMATING                     project: [ Cold Creek (G1) ▾ ]
  ┌─ the tool (the estimate itself) ─────────────────────┐
  │  cost code   description        qty   unit    total  │
  │  ...                                                 │
  ├─ pipeline ───────────────────────────────────────────┤
  │  Wilson Landing  · G2 estimate refresh  · due 7/18   │
  │  Cold Creek      · G1 concept budget    · due 7/25   │
  │  Mesa Ridge pad  · G0 feasibility rough · due 8/01   │
  └──────────────────────────────────────────────────────┘
```

**Exemplar: the Estimating screen** (`/portal/estimating`).

[screenshot to be added: Estimating screen with project dropdown and
pipeline]

## The third pattern: scorecards for routine-driven horizontals

Routine-driven horizontals (Accounting, asset management) have no hand-off —
their work renews each period. They use the **scorecard archetype**: this
period's deliverables, statuses, and quality scores, reset on the period
boundary. Exemplars: `/portal/asset-scorecard`, `/portal/weekly`.

## Porting rule: SmartSuite → Supabase

The old sb-planning-tools dashboards keyed on **project type**. On Supabase
the key is **department**:

- Dashboards filter on `projects.department_id` — not a type field.
- Current gate is computed from decision-gate checklists — not a status
  column somebody maintains by hand.
- Time math reads `project_dates`.

If you're porting a dashboard and reaching for "project type," stop — the
department axis is the one the Grid guarantees will stay true.

---

*Previous: [The Tool Library](tool-library.md) · Next:
[Kompass builds tools with you](kompass-tool-builder.md)*
