# S-BOS 2.0 — Workspaces (design discussion)

**Status:** Design / not yet locked. Captured 2026-07-07.
**Related:** [[S-BOS_Strategic_Framework]] (lifecycle vs ongoing modes), [[S-BOS_Multi-Entity_Tenancy_Model]], [[S-BOS_Discussion_asset-management-blueprint]], [[S-BOS_Actuals_Loop_and_Time_Cards]].

## What Stitser Built is (anchoring org context)

Stitser Built is a **real-estate platform** that owns entities and organizes work around departments. Two orthogonal layers:

**Entities = legal vehicles the platform owns (two kinds):**
- **Licensed service companies** — hold a license to perform a service and bill for it: Built (commercial general contractor), Realm (custom-home contractor). Capabilities the platform deploys onto projects.
- **Asset / project-specific SPEs** — own a specific asset: Mayberry Gardens (office park), PC-1 Developers (subdivision), Built Investments S3 (Cal Ave). The things owned.

**Departments / Divisions = how work is organized (two kinds):**
- **Product-line / multi-functional** — construction + brokerage + development collaborate on a product line: Retail, Multi-family. Revenue-generating, cross-functional.
- **Support functions** — Accounting, Financing, Compliance, IT. Horizontal shared services (why Accounting has many entities "underneath" — it serves the whole platform).

**Operating model:** a product-line division runs a project, pulls in the licensed companies to perform the work (e.g. Built as GC), and an asset SPE owns it. Support divisions serve the whole platform. Hours flow through the **time card → billed to the entity served** (the billing spine).

**Implications for the workspace model:**
- **Entities ≠ Divisions.** Divisions/departments are the operational org (the workspace tree); entities are legal vehicles attached to that org as licensed-performers and asset-owners. This is why scope runs on **department → projects**, not the ownership `entities` tree.
- **Maps onto Project-based vs Routine-based:** product-line divisions are mostly Project-based (lifecycle); support functions are mostly Routine-based (ongoing).

## Concept

A **Workspace** is the core organizing unit *and* the primary navigation of S-BOS 2.0 — it replaces the "entity"/"company" vernacular. A workspace is a homebase that assembles four things scoped to it: **dashboards, tools, connectors, people**.

Levels (labels, not fixed depths):
- **Portfolio** (e.g. Stitser Built)
- **Division** (operating divisions of a parent — Accounting, 2nd Homes, Asset Management, Development, Brokerage). **"Division" and "Department" are interchangeable in S-BOS** — projects already carry a `department`/`department_id`, so a division IS a department.
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
- **Built (migration 074, sb-crm-poc `/portal/workspaces`).** A division is defined by its **member companies** (`department_companies`); scope flows **division → member companies → their people + projects** (filtered by `company_id`). `lib/workspaces/scope.ts` resolves the dept subtree → company ids; Kompass reads the user's active workspace(s) ∩ their `department_members` grants and filters projects + people. Non-breaking (no active workspace ⇒ full portfolio). Per-person access control gates both the Workspaces UI and Kompass scope (admins see all). Membership was bootstrapped from existing `projects.department_id → company_id` tags (e.g. Retail ← Assiduity, Slide Side Junction).
- *(An earlier attempt scoping on the `entities` ownership tree / `projects.department_id` was reverted as premature; the shipped model is company-membership-based.)*

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
