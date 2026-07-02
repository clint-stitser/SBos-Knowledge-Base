# Plan: Entry-Level Housing Launch Day — 2026-07-02

> **Status:** Active — today's working plan for Clint + Claude Code.
> **Organizing principle:** the Entity → Category → Project → facets model is *universal* — anything built generically today (a schema, a catalog, the Dispatcher) serves every future category/vertical, not just this one. So the fastest path isn't "build Entry-Level Housing" directly — it's **build the generic layer once, then populate/activate it for this pilot.** That's the order below.
> **Relationship to other docs:** executes the `S-BOS_Discussion_category-scoped-cutover.md` build checklist (§5) as part of Layer 1, and lays the schema groundwork the `S-BOS_Discussion_kompass-dispatch-architecture.md` Dispatcher needs. Doesn't reorder §4 Core Features — that's paused for today.
> **Two open inputs needed from Clint (flagged inline below, not blocking the rest):** the Track 2 scheduling question that wasn't stated, and the empty "AI Integration #3" item.

---

## Track A — Q2 Loan Reconciliation (fully parallel, not sequenced below)

Not a Supabase build task — Credit Desk/Loans is Phase 2, not yet built. This runs entirely on existing tools (SmartSuite Credit Desk + invoicing/document + payment) in parallel with everything else today, bounded only by Clint's own time. **Open:** which tool actually generates the invoices (QuickBooks? Direct in SmartSuite? Word/Docs?) — if it's QuickBooks or Gmail, this chat can draft/send directly, live, alongside the Claude Code session.

---

## Track B — Entry-Level Housing Setup + AI Integration (sequenced, shared foundation)

### Layer 0 — Generic foundation (build once, benefits every future category/vertical)

Do these first, in roughly this order, because everything in Layer 1 depends on at least one of them:

1. **Category record + cutover flag** — Entry-Level Housing Category exists, flagged `cutover_mode: supabase_native` (per the cutover doc). *(Prereq for everything else.)*
2. **Blueprint/Template Catalog schema** — the generic table structure for activatable bundles (tasks, budget lines, schedule durations, roles, tool refs, skill refs), Category-anchored, RLS-scoped. *(Schema only here — populating it with real content is Layer 1, item 1.)*
3. **Knowledge Library schema** — scoped Postgres store (RLS + pgvector), scoped by entity/category/skill/vertical. *(Schema only — population is Layer 1.)*
4. **Project Schedule facet (dependencies, durations, start/end dates)** — this is Track 2 item 3. **⚠️ Needs Clint's unstated question answered before/while building** — flag it now (e.g., is this data-only, or does it need a Gantt-style view; what dependency types — finish-to-start only, or also start-to-start/finish-to-finish; does it need to support the multi-discipline parent-child schedule from §2.5/§3?). This facet is foundational because **both** Project activation (Layer 1) **and** the one-way SmartSuite push (Layer 1) need real schedule data to work against.
5. **Skill/Routine/Plugin Package catalog tables + the Dispatcher invocation path** — the generic `org_id`-scoped schema from the Dispatch Architecture doc. Build the schema and the single invocation function now; the actual Anthropic wiring (code execution, Files API, Skills API) plugs into it in Layer 1 — don't wire live API calls before the schema exists, or there's nothing durable to point them at.
6. **"Select Entities" settings screen (Surface blend)** — the toggle that lets a user show/hide personal vs. business Categories. This is fairly self-contained (routing + a settings UI over the existing Entity/Category model) — good candidate to run **in parallel** with items 2–5 if Claude Code can split attention, since it doesn't block or get blocked by the catalog/schedule/dispatcher work.
7. **AI Integration item "3."** — ⚠️ **unknown, need Clint's input** — slot it into Layer 0 or Layer 1 once named, depending on what it turns out to be.

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

- [ ] **Track 2 item 3's unstated question** — needed before/during Layer 0 item 4 (Schedule facet).
- [ ] **AI Integration item "3."** — needed to slot into Layer 0 or Layer 1.
- [ ] **The 4 chosen project names** — needed before Layer 1 item 4 (activation) and item 5 (cutover push).
- [ ] **Which tool generates Q2 loan invoices** — needed only if Clint wants live help in this chat on Track A.
