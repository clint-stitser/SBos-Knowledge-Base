# The Grid: Vertical × Horizontal × Gates

*S-BOS Foundations · Article 1 of 5 · Source: the department-review sessions
(canonical sketch: [department-review-illustration.pdf](department-review-illustration.pdf))*

---

Every part of Stitser Built's operating system hangs on one picture: a grid.
Vertical departments run projects top to bottom. Horizontal departments cut
across all of them. And gates — the decision checkpoints G0 through GX — form
the spine that tells everyone where a project actually is.

If you understand this grid, every screen, tool, and report in S-BOS will make
sense. The original whiteboard drawing of it is preserved as the canonical
illustration: **[department-review-illustration.pdf](department-review-illustration.pdf)**.

```
                Entry-Level   Retail   Multi-Family   Brokerage  ...
                    │            │          │             │
   G0 ─────────────┼────────────┼──────────┼─────────────┼────────
        Estimating ═══════════════════════════════════════   ┐
   G1 ─────────────┼────────────┼──────────┼─────────────┼── │ gate
        Finance    ═══════════════════════════════════════   │ window
   G3 ─────────────┼────────────┼──────────┼─────────────┼── ┘ (G0–G3)
        Accounting ═══════════════════════════════════════  (every period)
   ...              │            │          │             │
   GX ─────────────┴────────────┴──────────┴─────────────┴────────
                 dirt → done, one full blueprint per vertical
```

## Vertical departments: dirt → done

A **vertical department** is a product line. It owns a project for its entire
life — from raw dirt to a finished, disposed, or occupied asset — and it
carries a **full blueprint**: all the knowledge, tools, checklists, budgets,
and schedules that kind of project needs.

The verticals:

- **Entry-Level Housing**
- **Retail**
- **Multi-Family**
- **General Brokerage**
- **Asset Disposition**
- **Cool Shit** (one-off ventures that don't fit a standard line)
- **3rd-Party Dev & GC Services**

When a project is created, it lives in exactly one vertical. That department's
blueprint decides what the project gets out of the box — its default tools,
its gate checklist templates, its budget template lines.

## Horizontal departments: one skill, every vertical

A **horizontal department** applies a single skill set across all verticals.
Estimating estimates everything — an entry-level subdivision, a retail pad, a
brokerage-adjacent venture. Accounting closes the books on all of them.

The horizontals:

- Estimating
- Finance
- Accounting
- HR
- Marketing
- Insurance / Compliance
- Warranty
- Customer Service
- Executive

Horizontals come in two flavors, and the distinction drives how their screens
and schedules work:

### Project-driven horizontals: serve a gate window, then hand off

Estimating and Finance don't follow a project forever. They engage during a
**gate window** — G0 through G3 — do their work, and hand off. In the data
model this is `departments.gate_min` / `gate_max`. Their surfaces (see the
[Surfaces](surfaces.md) article) show only the projects currently inside
their window.

### Routine-driven horizontals: renew each period

Accounting, HR, and the asset-management pattern don't have a hand-off — their
schedule, budget, and task list **renew every period** (`operating_type =
'routine'`). A monthly close happens every month; there is no "done," only
this period's scoreboard.

## Gates: the spine

**Gates G0 through GX** are decision checkpoints — in S-BOS they are
decision-gate checklists (`kind = 'decision_gate'`) attached to the project.
Each gate has a final decision item; passing it means the organization has
formally committed to the next stretch of work.

The rule that makes the whole grid computable:

> **A project's current gate = the first decision gate whose final decision
> has not yet passed.**

That one rule powers everything downstream — which projects show up on the
Estimating screen (inside G0–G3), what the Entry-Level Housing dashboard shows
as "where this project stands," and which deliverables are due when.

## Why the grid matters

- **A project always knows where it is** — its vertical says what it is, its
  current gate says how far along it is.
- **A person always knows what's theirs** — verticals watch their projects
  across all gates; project-driven horizontals watch all projects inside
  their gate window; routine horizontals watch the period.
- **Tools attach along both axes** — the vertical blueprint decides the
  default toolbelt; the horizontal's work shows up as gate deliverables. See
  [The Tool Library](tool-library.md).

[screenshot to be added: Entry-Level Housing dashboard showing projects ×
current gate]

---

*Next: [From Ideas to Goals — the bottom-up ontology](task-ontology.md)*
