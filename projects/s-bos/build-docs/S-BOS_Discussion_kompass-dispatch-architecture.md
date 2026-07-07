# Discussion: Kompass Dispatch Architecture

> **Status:** Design exploration — not yet a locked PDD section. Captured per the Operating Agreement's "As needed" discussion-doc convention.
> **Why this matters:** This is the mechanism by which team members (and, eventually, licensees) build their own workflows/skills/routines inside their own Kompass instance from Day 1, while every workflow they create is automatically captured as versioned, cataloged **company property** in Compass's own database — not trapped in a personal Claude Project or chat thread. This is foundational to the internal team launch and to the "Realtor Kompass" licensing model.
> **Relationship to other docs:** This is a parallel track to PDD §4 (Core Features back-fill). It does not replace or reorder `restart.md` → Next Steps unless Clint says so. This thread corresponds to **Pillar C (the Kompass assistant + Feed)** and **Pillar D (Execution Tools)** in the locked Product Vision (§2.5) — this discussion works out *how Pillar C actually dispatches*, ahead of when it was originally sequenced.

---

## 1. The Core Problem Being Solved

Today, skill/project instructions live inside Claude's own surfaces (Projects, Skills panel, Cowork). This causes:
- **Org-brain leakage** — a workflow refined in one person's chat thread doesn't make it back into a catalog the whole org benefits from.
- **Data bleed** — outputs get manually copy-pasted from a chat surface back into the system of record, introducing transcription errors.
- **The always-on-machine problem** — routines built to run inside a persistent chat/Cowork session require a machine to stay on per person running them. That doesn't scale to a team, let alone a licensee's team.
- **No ownership boundary** — if a team member builds a great workflow inside their own personal Claude account, it's not clearly captured as company property.

## 2. The Target Architecture

**Compass (Supabase) becomes the sole owner of all instruction/workflow content. Claude becomes a stateless compute call Compass makes — not a surface people go work inside.**

Core components:

- **Skill/Instruction Catalog** — a Supabase table per `org_id`: name, version, instruction body (source of truth), scope (entity/category/project/user), declared tool bindings, expected input/output schema, model requirement. This is the **canonical, editable copy** — the thing a user actually edits inside their Kompass instance.
- **Routine table** — trigger type (manual, cron/schedule, DB event/webhook), which skill(s) it chains, status, last/next run. Routines never touch Anthropic directly — they're pure Compass scheduling logic.
- **Plugin Package** — a bundle of skills + routines + tool refs, activatable onto an instance. Same pattern as the existing Blueprint/Template Catalog (Category-anchored, activation = instantiation) — reused one layer up, for AI capability instead of project structure.
- **Dispatcher** — a single internal invocation path (`invoke(skill_id, context_refs, structured_input)`) that every trigger source calls: manual button, scheduled routine, or a mini-app "ping." One path = one Run Log, one permission gate, one place to enforce structured-output discipline, regardless of what triggered it.
- **Mini-apps / Execution Tools (Pillar D)** — deterministic, purpose-built software (budget builder, pay-app runner). They are clients of the same Dispatcher, not separately integrated with Claude. A mini-app never knows or cares which model answers.

## 3. Document-Generation Workflows (template + data + output)

Split into two jobs that should **not** both go to the LLM:

1. **Data gathering — Compass does this itself.** The dispatcher queries Compass's own DB and assembles a small, clean structured payload. The model is never asked to "go find the data."
2. **Authoring/assembly — the actual skill call**, using Anthropic's current API primitives:
   - **Code execution tool** — runs Python/bash in Anthropic's sandboxed container; no local/always-on machine needed.
   - **Files API** — uploads a template/data file ahead of time, referenced by ID in the call; downloads the generated output afterward. **Generated files expire from Anthropic's Files API after 24 hours** — the dispatcher must pull and store the result immediately into Supabase Storage.
   - **Skills API (`/v1/skills`)** — lets Compass upload its own custom Skills (org-specific templates + field-mapping logic) directly, versioned, callable by `skill_id`. Anthropic's pre-built docx/pptx/xlsx/pdf Skills can also be referenced directly (up to 8 Skills per request).
   - **No human-in-the-loop is required on Anthropic's side to create/update a Skill** — it's a plain authenticated API call. Self-service on Compass's side is fully possible.

Compass's catalog row for a document-generation skill stores a **pointer**: `skill_id` + version + which template it pairs with — not the docx-generation logic itself.

## 4. Multi-Tenant / Licensing Model — "Realtor Kompass" Case Study

Scenario: a third-party realtor licenses the system as "Realtor Kompass," with their own documents/workflows/routines that get built out over time as their team uses it.

### Decision: Shared workspace now, dividers built for future separation
- **Option A (adopted for now):** One Anthropic workspace/API key, with Compass's own catalog table (`org_id` → `skill_id`) enforcing which org can see/invoke which skill. Lower overhead; appropriate with a small number of licensees.
- **Option B (design for, don't build yet):** A separate Anthropic workspace/API key per licensee — true platform-level isolation, no shared "room," cleaner security story, per-tenant usage billing for free. Becomes worth the overhead once there are enough licensees that per-client billing/security separation matters, or a licensee explicitly requires it.
- **Build now so the switch is a config change, not a rebuild:** the `org_id`-scoped catalog table is the load-bearing piece either way.

### BYOK (Bring Your Own Key) — a third path, licensee-initiated
- A licensee can open their **own** Anthropic commercial account and hand Compass their own API key (stored securely, like any credential). From then on, Compass dispatches on their behalf using *their* key.
- Effects: Anthropic bills them directly (not Clint); their skills/data live under an account Clint doesn't own — true separation as a side effect; Compass gains a new support burden — detecting if their account runs low/hits a limit, since that would silently break their routines.
- **This is an optional path, not a requirement.** Anthropic's terms squarely support a standard commercial API key powering a product a company sells to its own customers (Clint's default model) — the restrictions that have made news are about people piggybacking a personal Claude subscription (Free/Pro/Max/OAuth) to serve outside paying users, which is a different scenario entirely. Recommend Clint (or counsel) confirm current Commercial Terms of Service language directly before finalizing licensing paperwork, since terms in this space have shifted more than once recently — but architecturally, the default plan is on solid ground either way.

## 5. Skill Review-Gate Workflow (adopted)

For content a licensee's/team's own staff build or edit:

1. **Draft** — user edits their workflow/template inside their own Kompass instance (no code — fields/text, like editing a document).
2. **Test** — Compass runs a private trial with sample data; nothing live uses it yet.
3. **Confirm** — the user/team confirms the draft is ready.
4. **Joint stress test** — Clint meets with them and stress-tests it together.
5. **Publish** — only after that joint session does the new version go live and become what routines actually execute against.
6. **Safety net** — versioned publishing means a bad edit never breaks what's already live; rollback to the last good version is always available.

*(Open question, not yet decided: whether/when a licensee's own admin gets delegated authority to approve their own future edits without Clint in the loop — parked for after the first few rounds prove out.)*

## 6. Data Residency — where things actually live

- **Supabase is the canonical, source-of-truth store** for every `.md` instruction file, every document template, and every routine/plugin definition, scoped by `org_id`, inside the org's own app instance. This is what the user edits, what makes the workflow "company property," and what survives independent of any AI vendor relationship.
- **Anthropic's side (via the Skills API) holds a secondary, execution-ready copy** of anything actually *published* — required because the code-execution sandbox needs the Skill materialized there to run it. This copy is not covered by Zero Data Retention and follows Anthropic's standard retention policy. It is a **runtime mirror**, not the vault — if the Anthropic relationship ever changed, the authoritative content is intact in Supabase and could be republished elsewhere.
- Generated **outputs** (a produced docx/pptx from a run) follow the same pattern: transient at Anthropic (24-hour expiry on the Files API), pulled down and stored durably in Supabase Storage immediately by the dispatcher.
- **A stricter alternative exists** if zero secondary residency at Anthropic is ever required: skip pre-registering named Skills via the Skills API, and instead pass instructions/templates inline with each call via the Files API without persisting a Skill definition. Trade-off: loses the versioning/catalog convenience the Skills API gives natively; would need Compass to replicate that bookkeeping itself. Not needed today — noted for the record in case data-residency requirements tighten later (e.g., a licensee's compliance requirements).

## 7. Open Items From This Discussion

- [ ] Sketch the actual Dispatcher invocation contract (request/response shape) — deferred pending §4 sequencing decision.
- [ ] Sketch the Skill / Routine / Plugin Package table schema — deferred, same reason.
- [ ] Decide when (not if) to move from Option A to Option B per-tenant workspace separation — trigger conditions: licensee count, licensee compliance requirement, or per-tenant billing need.
- [ ] Confirm current Anthropic Commercial Terms of Service language directly (or via counsel) before finalizing Realtor Kompass licensing paperwork.
- [ ] Decide, for Realtor Kompass and future licensees, when (if ever) their own admin gets delegated publish-approval authority.
- [ ] Build-vs-buy call on document-generation execution: lean on Anthropic's Skills API/Code Execution (recommended) vs. self-hosting a sandboxed runner.

---

## 8. Related but distinct: the code-dispatch loop (built 2026-07-06)

The **Dispatcher** described above is the *skill/workflow runtime* — `invoke(skill_id, context_refs, structured_input)` → a stateless Anthropic Messages API call, logged to `dispatch_runs` (see `scripts/dispatch.mjs`). Do **not** conflate it with the **code-dispatch loop** built on 2026-07-06, which is a separate system that dispatches *platform-build tasks* to **Claude Code** to write app code and open PRs: the `code_dispatches` queue + the `/portal/dev-dispatch` console + `scripts/dispatch-worker.mjs` (isolated worktree → headless `claude` → PR, never merges). Same verb ("dispatch"), different job — one runs skills at runtime, the other builds the platform. Full write-up: **`S-BOS_Self_Hosted_Build_Loop.md`** (and Build Decisions Log BD-06/BD-07).
