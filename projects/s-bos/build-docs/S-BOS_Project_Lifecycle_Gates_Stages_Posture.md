# S-BOS 2.0 — Project Lifecycle: Decision Gates, Stages, Posture & Portfolio Bands

**Doc class:** design · **Captured:** 2026-07-04 · **Status:** built (migrations 036–038)

## Problem
How to represent "where is a project?" without forcing a single linear status or making the team maintain likelihood-of-close percentages (which they were gun-shy about and hard to keep current).

## Decisions

### Stage auto-derives from decision gates
Gates live in `check_lists` (`kind='decision_gate'`, titled `G0`…`G6`). A gate is *passed* when its `final_decision` reads go/proceed/approve/pass/complete/yes. `v_project_stage` maps the **furthest passed gate** to a stage:

- G0–1 → **Biz Dev** · G2–3 → **Pipeline** · G4 → **WIP** · G5 → **Closeout & Warranty** · G6 → **Complete**

`stage_override` lets senior management pin a project (e.g. **Stalled**) — override wins over the derived stage. Cached to `projects.stage`.

### Closeout & Warranty is its own stage (migration 038)
Added **G6 — Closeout & Warranty** to the CrossMod blueprint and remapped: G5 (homes placed/sold/CO) → Closeout & Warranty; G6 (warranty expired, books closed) → Complete. G6 covers punch list, retention release, warranty-doc handoff, and monitoring the warranty period.

### Forecast posture instead of probability
Rather than a % on each deal, senior management sets a **posture band**: `Committed | Go | Watch | Excluded`. This replaces likelihood weighting entirely.

### Portfolio = conviction bands
`v_portfolio_cash_flow` bands projects by posture. The portfolio view reads **Committed (floor) → +Go (expected) → +Watch (all-in)** so leadership sees a floor and layered upside, not a blended guess. Stalled/estimate deals land in Watch, never the committed floor.

## Line-item lifecycle (context)
Budget lines (`uw_cost_items`) carry estimate (qty×unit_cost) → committed → actual with a status; effective = actual ?? committed ?? selected-bid ?? estimate. Many schedule tasks can roll up to one budget line via `tasks.budget_item_id`. Actuals are fed by the operational docs (see the Actuals Loop article).

## UI
`/portal/project-ladder` renders the 5-rung ladder + a 7-gate stepper; passing/resetting a gate advances the stage live (server actions). `/portal/portfolio` renders the conviction bands.
