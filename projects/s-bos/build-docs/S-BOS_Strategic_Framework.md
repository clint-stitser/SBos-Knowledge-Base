# S-BOS Strategic Framework (v2 — locked)

**Status:** Canonical & locked 2026-07-07. Supersedes the SmartSuite-era "Stitser BUILT Game App" doc (dated data structure). This is the source of truth for the strategic layer (Pillars / Goals / Phases / Tasks, the score, GYR, and commitment rules). Reconcile any other doc or session against this one.

## 1. The thesis (unchanged — this is the point)
Everyone plays harder when they know they're winning. To know, you must be able to answer, at a glance: **Who's playing · What's the score · How much time is left.** Every element of the system exists to keep those three answerable, and to keep the team aligned on: where are we going, why, when, where are we now, how far left, how much time, and who owns what.

## 2. The ladder (milestones absorbed into tasks/checklists — 2026-07-07)
| Tier | Duration | What it is | S-BOS home (REUSE — no new tables) |
|---|---|---|---|
| **Pillar** | Standing / annual | A perpetual operating area (e.g. the 7 asset-mgmt pillars; a company function). Holds no raw number — it's the **roll-up**. | `goals` (`kind='goal'`), entity/dept scoped |
| **Goal / Target** | 6–18 mo, or a standing KPI | A single measurable outcome: one metric, one target, a due date, an owner, players. **One number per goal — the game framework.** | `goals` (`kind='target'\|'threshold'\|'covenant'`), nested under a pillar via `parent_goal_id` |
| **Phase / Sprint** *(was "Priority")* | 3–6 mo | A time-boxed initiative that moves goals; parallel phases run at once and report separately. | `priorities` (already has `window_start`/`window_end`) — **renamed conceptually to Phase/Sprint** |
| **Task** | minutes–days | Handoffs, predecessors, follow-ups. **Milestones fold in here** (a task/checklist item can mark a notable progress point). | `tasks` (+ `check_lists` to group), universal polymorphic parent |

**Nested goals:** a Pillar is a goal; each KPI is a goal **under** it (`parent_goal_id`). The single-number game framework stays pure at the leaf; the multi-element scorecard exists only as a **roll-up** at the Pillar and Entity tier.

## 3. The score — current value comes from Stats (feed-first, routine-capture fallback)
A goal stores the **target**; the **current value ("the score") comes from `stat_logs`** — time-series readings tied to a `stat_type` (metric) linked to the goal (`stat_types.default_goal_id`, `stat_logs.goal_id`/`priority_id`). This layer already exists (body metrics — weight/Oura/Strava — run through it live).
- **Feed-first:** where an automated feed exists, it writes readings on a routine (TSW pattern: Oura, Strava, Siri weight; for assets: operating statements, occupancy reports, bank/loan data, budget-vs-actual). `stat_logs.source` records the feed.
- **Routine capture where no feed exists:** a scheduled prompt/form (REUSE `routines` / `routine_steps` / `scheduled_jobs`) captures the reading on cadence; `source='routine_capture'`.
- **GYR is computed** from the latest reading vs the goal's target/threshold (`operator`/`severity`, migration 069).

## 4. GYR — four levels, cascading but independent & overridable
- **Super Green (ahead)** · **Green (on track)** · **Yellow (at risk)** · **Red (critical).** (Restores the 4th level — "ahead" fuels momentum.)
- Status **cascades** (leaf KPI → Pillar → Entity) as a **default roll-up signal**, but each tier's status is **independent and human-overridable**: a Red leaf flags attention without forcing its Pillar/Entity Red (per the original "a milestone can be RED while the priority & goal stay on track"). Roll-up default = worst-of-children unless overridden with a note.

## 5. Commitment lock (targets & due dates are promises)
Once set, a goal's `target_value` / `target_date` — and any `covenant`-kind threshold — **cannot be changed casually.** A change requires: (a) a written reason, logged as a **Decision** (`decisions`); (b) **elevated permission** (executive/system_admin); (c) the `fn_audit` trail records old→new (migration 066). This preserves the integrity of the score.

## 6. Players, scope & access
- **Company / entity** — required (a goal can span multiple SB companies). **Department** — optional. **Partner/Customer company** — external collaborators get **scoped access to only their associated goals** (the licensing/multi-tenant seam). **Owner** — per Pillar/Goal/Phase/Task.
- Reuse: `entity_id`/`department_id` columns + `scope` jsonb on goals; `task_people`/`stakeholder_bridge` for owners/players.

## 7. Thresholds = the goals extension (already reconciled)
Enforcement teeth (covenants, floors, approval limits) are `goals` with `operator`+`severity`+`scope` (migration 069), evaluated by the GYR engine — **not a parallel `thresholds` table.** See `S-BOS_Discussion_asset-management-blueprint.md` §4 (reconciled).

## 8. Reuse map — the whole framework rides existing infrastructure
| Concept | Table(s) | New? |
|---|---|---|
| Pillar / Goal / KPI / target / threshold / covenant | `goals` (+ `parent_goal_id`) | +1 column only |
| Phase / Sprint | `priorities` | reuse |
| Task / milestone / checklist | `tasks`, `check_lists` | reuse |
| The score (readings) | `stat_types`, `stat_logs` | reuse |
| Routine capture + scheduling | `routines`, `routine_steps`, `scheduled_jobs` | reuse |
| Roll-up / contribution | `priority_contributions` | reuse |
| GYR evaluation + history | GYR health engine, `project_health_reports` | reuse |
| Commitment lock audit | `fn_audit` / `audit_log` | reuse |
| Reference knowledge | `kb_entries` (entity-scoped) | reuse |
| Activatable bundles | `blueprints` (`is_template`/`blueprint_id`) | reuse |

**Net new schema across the entire framework: one nullable column (`goals.parent_goal_id`).**

## 9. Rhythm & celebration (motivational layer)
- **Weekly rhythm:** review the Game (Pillar/Goal) scoreboard → Phase/Sprint scoreboard → open tasks & What/Who/When. (Reuse Weekly Health.)
- **Celebration tiers:** task/milestone win = acknowledgement; Phase win = small celebration; Goal win = bonus. (Optional; can drive notifications.)

## 10. Applied: asset management (the first full use case)
The 7 pillars (Revenue; Customer Service & Retention; Asset Maintenance & Capital Planning; Financial Mgmt/Budget/OpEx; Capital Markets & Debt; Investor Reporting & Relations; Risk/Insurance/Compliance) are **pillar-goals per asset entity**; each pillar's KPIs are nested goals fed by `stat_logs` (operating-statement feed or routine capture); covenants are `covenant`-kind goals with the commitment lock; recurring cycles are Projects contributing to pillars; all bundled as an activatable blueprint. See `S-BOS_Discussion_asset-management-blueprint.md`.

## Open items
- [ ] Add `goals.parent_goal_id` (the one column) + wire 4-level GYR roll-up (leaf → pillar → entity) with override.
- [ ] Build the routine-capture UI for metrics with no automated feed (reusing `routines`/`scheduled_jobs`).
- [ ] Enforce the commitment lock on `target_value`/`target_date`/covenant edits (reason → Decision + elevated perm).
- [ ] Asset operating-statement → `stat_logs` feed (per-entity), so asset KPIs self-populate.
