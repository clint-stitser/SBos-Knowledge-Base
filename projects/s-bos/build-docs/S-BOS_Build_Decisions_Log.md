# S-BOS Build Decisions Log

> **Purpose:** Typed record of decisions made during the build phase — Deviation / Workaround / Reconciliation / Concern / Gap / Dependency / Scope creep. Logged immediately when they happen (per the Operating Agreement).
> **Format:** BD-NN · date · type · what · why · impact.

---

## BD-01 · 2026-07-02 · Reconciliation
**What:** In Layer 0 item 4, renamed the live `check_list_tasks` table → `tasks`, and folded its existing `source_type`/`source_id` columns (loan/project provenance added during the Credit Desk work) **into** the new polymorphic `parent_type`/`parent_id` link. Added `'loan'` to the `parent_types` registry.
**Why:** Realizes the "one universal Task table" (PDD §3). `source_type`/`source_id` and `parent_type`/`parent_id` are the same polymorphic-parent concept, so keeping both would duplicate meaning. `'loan'` isn't in today's nominal registry scope (Projects/People/Companies/Budget items/Schedule-Tasks) but the 117 Credit Desk reconciliation tasks were already loan-parented, so the registry row preserves them rather than orphaning.
**Impact:** 1 app read (`sb-crm-projects-list-details/page.tsx`) and 1 script (`build-q2-reconciliation.mjs`) updated to `tasks` + `parent_type`/`parent_id`. Migrations 001/002/012–015 left as historical (a fresh replay renames at 017). Verified: 117 loan-parented tasks folded, **0 orphans**, tsc clean. Migration `017_tasks_and_scheduling.sql`.

---

## BD-02 · 2026-07-02 · Gap / Dependency
**What:** Layer 0 item 7 built a NEW canonical `comments` table (polymorphic `parent_type`/`parent_id` via the shared registry) rather than evolving the existing live `notes_comments` hub.
**Why:** `notes_comments` (Biz Dev CRM) is many-to-many via junction tables (one note → many projects/people/companies) and holds live data; the universal decision is polymorphic single-parent. Reconciling them mid-session risked live-CRM data.
**Impact / open:** Two comment stores coexist temporarily. Pending decisions (Clint): (a) migrate `notes_comments` → `comments`, and (b) whether multi-attach is preserved (a junction) or becomes multiple comment rows. Not blocking today's Layer 0.

---

## BD-03 · 2026-07-02 · Reconciliation
**What:** Layer 0 item 2 added `is_template` + `blueprint_id` to tasks, check_lists, task_dependencies, project_budget_items, stakeholder_bridge, and relaxed NOT NULL on `project_budget_items.project_id` and `stakeholder_bridge.project_id`/`person_id`.
**Why:** A Blueprint's template rows live in the same entity tables (blueprint_id set, project_id null) and copy onto a project at activation. Template budget items + roles have no project/person yet, so those columns had to be nullable.
**Impact:** Backward-compatible (existing rows unaffected). Migration `019_blueprints.sql`. Activation copy-onto-project function deferred to Layer 1.

---

## BD-04 · 2026-07-06 · Reconciliation
**What:** Dropped a redundant file-storage system — a `captures` Storage bucket + `attachments` table created earlier in the session — and consolidated all capture attachments onto the existing `documents` bucket + `document_library` table (which already attaches files to any record via `parent_type`/`parent_id`).
**Why:** `document_library` already solved polymorphic file attachment; the parallel system duplicated meaning and split file storage in two. Caught when Clint asked "Did you create an additional file storage system?"
**Impact:** `/api/capture/upload` now writes to `documents` (service role, mirroring `app/portal/documents/actions.ts`); the filer writes `document_library` rows (`doc_type='attachment'`). `attachments` + `captures` removed in migration `065`. Standing rule reinforced (now Rule 1 in AGENTS.md): search for existing infra before adding storage.

---

## BD-05 · 2026-07-06 · Gap
**What:** Project- and document-level versioning + an `audit_log` backstop had never actually been built — only `kb_entries`, `blueprints`, and `skills` carried the paired `*_versions` pattern.
**Why:** The described design (paired `*_versions` + `current_version_id` pointer, never-overwrite, `audit_log`/`fn_audit` as passive backstop) was assumed present but wasn't wired for `document_library`/projects/tasks/etc. Surfaced when Clint asked to confirm the versioning design was real.
**Impact:** Migration `066` adds `document_library_versions` + `current_version_id` (trigger `trg_doclib_version`: INSERT→v1, content/storage change→new version, never overwrite) and `audit_log` + `fn_audit` applied to document_library/projects/tasks/notes_comments/decisions/journals/kb_entries. Verified insert→v1, edit→v2. **Open (punted):** retention/cleanup policy — deferred until real usage data exists.

---

## BD-06 · 2026-07-06 · Decision
**What:** Moved the build rules + "what already exists" to live IN the `sb-crm-poc` repo — `AGENTS.md` (operating rules; Rule 1 = reuse-before-build), `docs/ARCHITECTURE.md` (the live PDD / built-state source of truth), a `build_log` table (queryable "recently built" ledger), and `.github/pull_request_template.md` (governance checklist). `CLAUDE.md` made self-contained (inlines core rules + imports `AGENTS.md`) so web/cloud loaders that don't expand imports still get the guardrails.
**Why:** A dispatched or cloud Claude Code session has none of this chat's memory. Without the rules + built-state in the repo, sessions "stray into never-never land" and rebuild things that exist (see BD-04). The `/api/kompass/dispatch` prompt injects the last 20 `build_log` rows as "RECENTLY BUILT — reuse, do not rebuild."
**Impact:** Every dispatched/cloud session now reads binding rules from the repo. Convention: append a `build_log` row + update `docs/ARCHITECTURE.md` in the same PR for anything substantial (the PR template enforces it).

---

## BD-07 · 2026-07-06 · Decision
**What:** Built the code-dispatch worker (`scripts/dispatch-worker.mjs`) — the external half of the "S-BOS builds S-BOS" loop. It drains `code_dispatches` where `status='queued'`, creates an isolated git worktree off `origin/main`, runs the `claude` CLI headless there, then pushes + opens a PR via `gh` and writes status/pr_url back. Design constraints adopted: (a) isolated worktree so the main checkout is never touched; (b) **PR-only, never merge** — every change lands as a PR for human review; (c) the worker is intentionally left **blocked from Claude Code auto-mode** — the classifier denies spawning an autonomous `--dangerously-skip-permissions` agent, so it runs from an interactive terminal / cron host with one-time approval.
**Why:** The coding step needs credentials + a repo checkout, so it can't live inside the app. The guardrails keep an autonomous agent from merging unreviewed code, or from being launched unsupervised from within another agent session.
**Impact:** Two run modes now exist — **Launch** (per-dispatch deep-link to a cloud claude.ai/code session; initiate-from-app, any device) and **Queue → worker** (hands-off; opens a PR). Console at `/portal/dev-dispatch` (system_admin-gated). Full write-up: `S-BOS_Self_Hosted_Build_Loop.md`. Creds in gitignored `.env.local` (`SBOS_ANTHROPIC_KEY`, `GITHUB_TOKEN`).
