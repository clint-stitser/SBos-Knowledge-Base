# S-BOS 2.0 — Workspaces (design discussion)

**Status:** Design / not yet locked. Captured 2026-07-07.
**Related:** [[S-BOS_Strategic_Framework]] (lifecycle vs ongoing modes), [[S-BOS_Multi-Entity_Tenancy_Model]], [[S-BOS_Discussion_asset-management-blueprint]], [[S-BOS_Actuals_Loop_and_Time_Cards]].

## Concept

A **Workspace** is the core organizing unit *and* the primary navigation of S-BOS 2.0 — it replaces the "entity"/"company" vernacular. A workspace is a homebase that assembles four things scoped to it: **dashboards, tools, connectors, people**.

Levels (labels, not fixed depths):
- **Portfolio** (e.g. Stitser Built)
- **Division** (operating divisions of a parent — Accounting, 2nd Homes, Asset Management, Development, Brokerage)
- **Company** (LLCs / investment vehicles — e.g. Built Investments Series 3)
- **Project** (e.g. Cal Ave Studios) — *not shown in the org tree; lives on its parent workspace's dashboard*
- **Personal** (a person's Kompass life-OS) — its own separate root; **Family** is the same row one level up (an upsell tier)

## Structure vs contents

- **The Workspaces tree = structure** (portfolio → divisions → companies). This is where you drag-to-reparent. Rides on `entities.parent_entity_id`.
- **The workspace dashboard = contents** (its projects, tasks, people, tools, feed). Projects never clutter the tree — you see them once you *enter* a workspace.

## Nesting (free-form)

Level is just a label; nesting is free-form. A **division can hold companies** (Asset Management → Built Investments S3 → Cal Ave). This is the *operational* org chart — who runs/oversees what — and it doubles as the **billing spine**: Stitser BUILT's divisions provide services, log hours on the time card at an hourly rate, and bill the entity they serve down the tree.

**Keep legal ownership separate.** The ownership/liability chain (Stitser Built *owns* S3 *owns* Cal Ave) is a distinct fact from the operational rollup (Asset Management *manages* S3). The workspace tree = operational; ownership stays as its own pointer/attribute so both remain true. (Open decision: model the operational tree as a layer over the ownership `entities`, not mixed into it.)

## UI

- **Sidebar:** one single-select **"Workspaces"** entry — no nested tree crammed into the rail.
- **Entry/switching happens on a page** — a card-style org tree: divisions side by side as columns, the entities each division runs stacked beneath. Enter a card to make it your active homebase; drag a card to reparent.
- **Visible / Hidden toggle** per workspace: de-activate a messy division while you clean it up — it drops out of the tree *and* out of Kompass's scope, but nothing is deleted. Flip back on when ready. (Distinct from "entered/current," which sets Kompass focus.)

## Kompass scope (multi-activate, inclusive downward)

- Standing in a workspace scopes Kompass to **that workspace and everything beneath it** you're a member of. Stand at the portfolio top → sees everything; stand on one project → sees just that project.
- **Multi-activate:** business portfolio and Personal can be active at the same time for a combined view; scope = union of the active workspaces' subtrees (∩ membership for non-owners).
- Built: migration 073 added `workspace_level` + `workspace_visible` to `entities`; `resolveScope()` in `/api/kompass/chat` walks each subtree and filters projects (by `company_id`) and feed (by `entity_id`). Non-breaking — no scope passed = full portfolio.

## Workspace setup — Project-based vs Routine-based (REQUIREMENT)

**On creating a workspace, the system must prompt the user for the workspace's operating type**, which sets its game model and default dashboard. This is the two-mode distinction from [[S-BOS_Strategic_Framework]]:

- **Project-based** (construction / development type) → **lifecycle mode.** Situations are sequential and gate-advanced (G0–G5), status-driven. Dashboard = project pipeline: stages, gates, posture. Example: a Development workspace.
- **Routine-based** (asset-management type) → **ongoing mode.** Situations are parallel and perpetual, self-scoring via `stat_logs` and recurring cycles / checklists. Dashboard = scorecard: health (GYR), recurring projects, key situations. Example: an Asset Management workspace or an individual asset (Cal Ave).

A workspace may later support both, but setup picks the primary orientation so the homebase, default views, and how Kompass reasons about "what needs attention" match the work. The choice is stored on the workspace and drives which dashboard template loads on entry.

## Data gaps before scope fully lights up

1. **Division layer doesn't exist yet** — no Accounting / 2nd Homes / Asset Management entities.
2. **Projects barely linked** — `department` null on ~622/724; only ~5 projects tie to an entity's company today.
3. **Feed not entity-linked** — `feed_items.entity_id` is 0/116 (pending "entity-link feed" work).

Scope plumbing is proven on the one populated branch (Stitser Built → 5 Cal Ave projects); lighting it up everywhere needs the org-tree build + these linkage passes.
