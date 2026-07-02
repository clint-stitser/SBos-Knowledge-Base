# Plan: Entry-Level Housing Launch Day — 2026-07-02

> **Status:** Active — today's working plan for Clint + Claude Code.
> **Organizing principle:** the Entity → Category → Project → facets model is *universal* — anything built generically today (a schema, a catalog, the Dispatcher) serves every future category/vertical, not just this one. So the fastest path isn't "build Entry-Level Housing" directly — it's **build the generic layer once, then populate/activate it for this pilot.** That's the order below.
> **Relationship to other docs:** executes the `S-BOS_Discussion_category-scoped-cutover.md` build checklist (§5) as part of Layer 1, and lays the schema groundwork the `S-BOS_Discussion_kompass-dispatch-architecture.md` Dispatcher needs. Doesn't reorder §4 Core Features — that's paused for today.
> **Scheduling scope resolved:** see `MS_Project_Style_Scheduling_Engine_Scope.md` — today builds v1 only (§1–4 of that doc); the full engine (§5: critical path, resource assignments, baselines, interactive drag-cascade) is its own future phase, not today's scope. "AI Integration #3" was a stray autonumber — ignore, no third item existed.

---

## NEW DECISION — 2026-07-02: System-Wide Document Versioning

**Applies to every document-bearing table built today and going forward:** Knowledge Library entries (global/entity/department/project levels), project documents (legal docs, design docs, invoices, etc.), the Document/Template Library (templates linked to project types/companies/departments), and AI-interface documents (skills, routines, prompts).

- **Pattern: per-type, append-only version tables** — not one shared polymorphic table. Each parent table (`kb_entries`, `project_documents`, `templates`, `skills`, etc.) gets its own sibling `_versions` table of identical shape: `id, parent_id, version_number, created_by, created_at, commit_message` + either `content` (text/markdown docs — fully diffable) or `storage_path` (file docs like invoices/legal PDFs — supersede-and-keep, not diffable). Which column applies is determined by the parent document's file type.
- Parent table carries `current_version_id` pointing at the live version; rollback = repoint, never delete history.
- **`supa_audit` runs underneath as a passive backstop** on these tables — automatic trigger-based row change logging, layered under the intentional versions tables (not a replacement for them; no commit messages/rollback semantics on its own).
- **Storage cost/retention policy for file versions is explicitly deferred** — revisit once the system is running and usage patterns are understood. Not a blocker for today's build.
- **Build impact on today's plan:** any schema built in Layer 0 that stores documents/content (Knowledge Library, Blueprint/Template Catalog, Skill/Routine catalog) should include its `_versions` table from the start, not retrofit later. Flagged inline in Layer 0 below.

---

## Track A — Q2 Loan Reconciliation (fully parallel, not sequenced below)

Not a Supabase build task — Credit Desk/Loans is Phase 2, not yet built. This runs entirely on existing tools (SmartSuite Credit Desk + invoicing/document + payment) in parallel with everything else today, bounded only by Clint's own time. **Open:** which tool actually generates the invoices (QuickBooks? Direct in SmartSuite? Word/Docs?) — if it's QuickBooks or Gmail, this chat can draft/send directly, live, alongside the Claude Code session.

---

## Track B — Entry-Level Housing Setup + AI Integration (sequenced, shared foundation)

### Layer 0 — Generic foundation (build once, benefits every future category/vertical)

Do these first, in roughly this order, because everything in Layer 1 depends on at least one of them:

1. **Category record + cutover flag** — Entry-Level Housing Category exists, flagged `cutover_mode: supabase_native` (per the cutover doc). *(Prereq for everything else.)*
2. **Blueprint/Template Catalog schema** — the generic table structure for activatable bundles (tasks, budget lines, schedule durations, roles, tool refs, skill refs), Category-anchored, RLS-scoped. *(Schema only here — populating it with real content is Layer 1, item 1.)* **Include `template_versions` per the versioning decision above.**
3. **Knowledge Library schema** — scoped Postgres store (RLS + pgvector), scoped by entity/category/skill/vertical. *(Schema only — population is Layer 1.)* **Include `kb_entry_versions` per the versioning decision above.**
4. **Project Schedule facet — v1 scope only** (Track 2 item 3, now fully specced in `MS_Project_Style_Scheduling_Engine_Scope.md`). Build today: a single `tasks` table where checklist items and schedule items are the same row (scheduling columns just nullable) — `duration`, `start_date`, `end_date`, `scheduling_mode` (manual/auto), `constraint_type`, `constraint_date`; a `task_dependencies` table (`predecessor_task_id`/`successor_task_id`, dependency_type FS/SS/FF/SF, `lag_days`); and the parent-child cross-project pattern (that doc's §3, Option B — a linked milestone task on the parent project, connected via `task_dependencies`, `scheduling_mode=auto`, recalculated by a simple Postgres trigger when the child task's date changes). **Do NOT build that doc's §5 today** (critical path calculation, resource assignments, baselines, constraint-type enforcement, interactive drag-to-reschedule cascade) — flagged in the doc itself as a meaningfully larger build deserving its own future phase, not an add-on to today. This facet is foundational because **both** Project activation (Layer 1) **and** the one-way SmartSuite push (Layer 1) need real schedule data to work against.
5. **Skill/Routine/Plugin Package catalog tables + the Dispatcher invocation path** — the generic `org_id`-scoped schema from the Dispatch Architecture doc. Build the schema and the single invocation function now; the actual Anthropic wiring (code execution, Files API, Skills API) plugs into it in Layer 1 — don't wire live API calls before the schema exists, or there's nothing durable to point them at. **Include `skill_versions` per the versioning decision above.**
6. **"Select Entities" settings screen (Surface blend)** — the toggle that lets a user show/hide personal vs. business Categories. This is fairly self-contained (routing + a settings UI over the existing Entity/Category model) — good candidate to run **in parallel** with items 2–5 if Claude Code can split attention, since it doesn't block or get blocked by the catalog/schedule/dispatcher work.

> **Note:** Project documents (legal docs, design docs, invoices) don't have a dedicated Layer 0 schema item in this plan yet — they fall under the Project Drive facet, not yet scheduled today. When that facet is built, `project_document_versions` applies per the versioning decision above.

### Layer 1 — Populate and activate (apply the foundation to this pilot)

Only makes sense once the relevant Layer 0 piece exists:

1. **Populate the Blueprint Catalog** with the actual content from the research session (Track 2 item 1) — this is the CrossMod-style stage-gate bundle for the Entry-Level Housing product line.
2. **Populate the Knowledge Library** at the Category/department level (Track 2 item 2, category-level) — the product-line knowledge captured in the research session.
3. **Wire the real Anthropic API calls into the Dispatcher** (Track 3 item 1) — code execution tool, Files API, Skills API, and Managed Agents/scheduled deployments, each hooked to the invocation path built in Layer 0 item 5.
4. **Activate the 4 chosen projects** from the Blueprint (Track 2 item 5) — stamps tasks/schedule/roles onto real Projects, and this is also when each project's own Knowledge Library store gets created and seeded (Track 2 item 2, project-level). **⚠️ Still need the actual 4 project names from Clint** — flagged since the cutover doc's earlier session.
5. **Run the Category-Scoped One-Way Cutover push + lockout** (from `S-BOS_Discussion_category-scoped-cutover.md` §5) on those same 4 projects — ID-pairing table, one-way push of Details/Schedule/Tasks/Team to SmartSuite, Day-1 banner/view lockout. Natural to do this right after activation, on the same projects, in the same sitting.

### Layer 2 — Surface (needs real data from Layer 1 to be meaningful)

1. **Product-line dashboard** (Track 2 item 4) — build this last on purpose; it's the piece most dependent on the 4 activated projects actually having real schedule/task/knowledge data to display. Building it earlier means building against fake data and re-touching it later.
2. **End-to-end pass on the Surface blend** — confirm the TSW+S-BOS merged shell actually surfaces the new Entry-Level Housing Category + dashboard correctly through the entity toggle built in Layer 0.

---

## If time runs short today

Protect in this order: **Layer 0 (all of it) → Layer 1 items 1, 2, 4, 5 → Layer 1 item 3 (AI wiring) → Layer 2.** The foundation and the activated pilot projects are the load-bearing wins; the dashboard and the live AI wiring are the most defensible things to carry into tomorrow if the day runs long, since neither blocks the other work and neither is irreversible if it slips.

---

## Open Items Blocking Specific Steps (not the whole plan)

- [x] ~~Track 2 item 3's unstated question~~ — resolved, see `MS_Project_Style_Scheduling_Engine_Scope.md`.
- [x] ~~AI Integration item "3."~~ — resolved, stray autonumber, no item existed.
- [ ] **The 4 chosen project names** — needed before Layer 1 item 4 (activation) and item 5 (cutover push).
- [ ] **Which tool generates Q2 loan invoices** — needed only if Clint wants live help in this chat on Track A.

## Future Phase (explicitly NOT today)

- [ ] **Full MS Project-style scheduling engine** — `MS_Project_Style_Scheduling_Engine_Scope.md` §5: additional `tasks` columns (`is_critical`, `is_milestone`, `baseline_start`/`baseline_end`, `percent_complete`), new `task_resource_assignments` + `schedule_baselines` tables, and the compute layer (critical path calculation, cascading recalculation, interactive Gantt drag behavior). Scope as its own build-doc phase once v1 is live and proven.
- [ ] **Storage cost/retention policy for file-based document versions** (invoices, legal docs, etc.) — deferred until the system is running and usage patterns are understood.
