# S-BOS Roadmap Sync Procedure

> **Purpose:** Keep the SmartSuite **roadmap App Items** (and their checklists, tasks, notes, tags, status) in lockstep with the build docs — from **either** surface (claude.ai or Claude Code).
> **How "sync to both" works:** This procedure lives in GitHub, so both surfaces read it. The *writes* execute through the **SmartSuite / Kompass MCP**, which both surfaces share. GitHub = the shared instructions; Kompass MCP = the shared write path.
> **Prerequisite:** the Kompass/SmartSuite MCP must be connected on the surface running it. Claude Code has it. **Confirm the claude.ai S-BOS Project has the Kompass connector enabled** — if not, do roadmap writes from Claude Code (or note the intended change here for the other surface to apply).

---

## When to run

- **At session end** — if any build-doc status changed this session, sync the affected App Items before writing `restart.md`.
- **On a status/scope change** — an item moves design→build→live, or its Kind/Stage changes.
- **On demand** — "sync the roadmap" / "update the roadmap."
- **Always read current state first** (search/get the record) before updating — never blind-overwrite, never duplicate. Deletes are **admin-only** (per the access model).

---

## SmartSuite reference (IDs, fields, codes)

### Projects table = App Items — app `68216a706900e8eaf75a05a7`
| Field | Slug | Notes |
|---|---|---|
| Name | `s937f1d342` | string (do NOT send `title` — auto-generated) |
| Status | `status` | code string — see column map below |
| Department | `s49e345573` | must include `6858d8c9355da45e14c28547` (IT/Systems) to appear on the roadmap |
| Kind of App Item | `sf7ec194d1` | `qrt21`=Feature · `WkstT`=Workflow · `EOw0c`=Data Layer · `n89Yl`=Integration · `Nn6M0`=Skill/Instruction · `JGMYu`=Training/Education |
| Applicable Stages | `sfdac4b613` | `10ilB`=Biz Dev · `XWpjX`=Pipeline · `YHm9w`=WIP · `dxvgP`=Closeout · `qWFP5`=Asset Mgmt |
| Inclusions | `s1dbcd776a` | `KjXTw`=Design Doc · `Yxrq4`=Claude Skill · `AI1Q0`=Workflow Diagram · `izl9N`=Built in Supabase |

**Status → roadmap column:** `backlog`=Active in Pipeline → **Pipeline** · `zOlNR`=Active in WIP → **Active Dev** · `Swowl`=Active in Closeout & Warranty → **Closeout** · `Dio3d`=Closed Job → **Live**.

### Check Lists — app `6a060dadc513b3329b7d4485`
Name `title` · Linked Project `s2c0eafab5` (=[project id]) · Tasks link `sfea6d742a` (auto from task side).

### Check List Tasks — app `68a8e17251dc814e8c529f3f`
Name `title` · **Status** `status`: `backlog`=Backlog · `in_progress`=In Process · `ready_for_review`=Ready for Review · `complete`=**Complete (done)** · `rz4lv`=N/A · Link to Project `se5f41aa17` · Link to Check Lists `sz08dosv`.

### Notes & Comments (hub) — app `6894e64f621641b3ef90fa99`
Title `title` · Body `description` · Linked Project `s4cc420502` · Linked Contact `s6b7bc01e8` · Linked Company `s7a3922f54` · Type `s00c42b87e` · Follow Up Date `s197b95d47` · Who Needs to Follow Up `sa4d425b5a` · Follow Up Completed `s2621072fb`.

### MCP tools (Claude Code names; claude.ai uses the Kompass connector equivalents)
`smartsuite_search_records` (find), `smartsuite_get_record` (read), `smartsuite_create_record` (one at a time — **bulk endpoint 500s**), `smartsuite_update_record` (PATCH fields).

---

## Trigger → action mapping

| Event in the build docs | Roadmap update |
|---|---|
| A shared doc now **covers an App Item** (e.g. PDD Feature+Workflow section written for a Feature item) | Find that item's checklist task for that doc (e.g. "PDD — Feature + Workflow entry") and set task `status` = `complete` |
| All of an item's Build-Docs tasks are `complete` | Item is **doc-complete** → ready for Coding Kickoff; note it and/or advance `status` |
| Item moves **design → build** | Set App Item `status` = `zOlNR` (Active Dev) |
| Item **shipped** | Set App Item `status` = `Dio3d` (Live) |
| Item **blocked / paused** | File a Note (see below) describing the blocker; leave status, flag in the note |
| Item's **Kind/Stage/Inclusions** change | Update `sf7ec194d1` / `sfdac4b613` / `s1dbcd776a` |
| A **decision, risk, or context** worth recording | File a Note/Comment against the App Item (Notes & Comments): set `title`, `description`, `s4cc420502`=[project id]; assign follow-up via `sa4d425b5a` + `s197b95d47` if action is needed |
| A **new build module** is identified | Create a new App Item (recipe below) + its Build-Docs checklist from `S-BOS_App_Item_Doc_Requirements.md` |

---

## Recipes

**Mark a doc-task complete**
1. `search_records` Check List Tasks where Link to Project = the App Item id (or filter by title).
2. `update_record` that task: `{ "status": "complete" }`.

**Advance an App Item's status**
1. `update_record` the Project: `{ "status": "<code>" }` (see column map).

**File a note against an App Item**
1. `create_record` in Notes & Comments: `{ "title": "...", "description": "...", "s4cc420502": ["<project id>"] }` (+ follow-up fields if needed).

**Add a new App Item + checklist**
1. Create Project (`s937f1d342`, `status`, `s49e345573`:[IT dept], `sf7ec194d1`, `sfdac4b613`, `s1dbcd776a`).
2. Create Check List (`title`="<Name> — Build Docs", `s2c0eafab5`:[project id]).
3. Create one Task per required doc for that Kind (from `S-BOS_App_Item_Doc_Requirements.md`): `title`, `se5f41aa17`:[project id], `sz08dosv`:[checklist id].

---

## Current App Items (as of 2026-06-17)

| App Item | Project ID | Checklist ID | Kind | Status |
|---|---|---|---|---|
| Biz Dev CRM Module | `6a330b9de2c37a7fc4b090c0` | `6a330c2d694668b3778683a9` | Feature | Active Dev (zOlNR) |
| Automation Tracker | `6a330b7a31192c7d94121c93` | `6a330c264d8bd87db82c8b50` | Feature | Live (Dio3d) |
| Migration Menu | `6a330bb1dbe0dd3a400c0456` | `6a330c33f1a10b4cf3e70b04` | Feature | Live (Dio3d) |
| Supabase Auth | `6a330c0d8bd286a4d0deb347` | `6a330c391cab54c1dac2089b` | Integration | Pipeline (backlog) |
| Recovery: Soft-Delete + Audit Log | `6a330c17932f792581c02d05` | `6a330c3edf2f5e7a366fc318` | Data Layer | Pipeline (backlog) |
| Remote CRUD MCP | `6a330c1e57aea2976b121c81` | `6a330c45614c8395e49d7703` | Integration | Pipeline (backlog) |

> Keep this table current when items are added. The live roadmap renders from these records: `sb-planning-tools-production.up.railway.app/roadmap/`.
