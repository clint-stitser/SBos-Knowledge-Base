# S-BOS Self-Hosted Build Loop & Governance

> **Status:** Built and in use (2026-07-06). Describes how S-BOS 2.0 builds itself — the dispatch loop plus the guardrails that keep dispatched Claude Code sessions on-rails.
> **Why this matters:** S-BOS 2.0 is dogfooded as a project inside S-BOS (IT/Systems). Build work is captured, dispatched, and reviewed *through the platform itself* — and every dispatched session inherits the same rules a human builder follows. This is also foundational to the licensing story: a licensee's developer gets a safe, guardrailed way to extend their own instance without a person babysitting a chat window.
> **Related but distinct:** the **skill/workflow dispatcher** (`invoke(skill_id,…)` → `dispatch_runs`, `scripts/dispatch.mjs`; see `S-BOS_Discussion_kompass-dispatch-architecture.md`) runs *skills at runtime*. The **code-dispatch loop** documented here dispatches *platform-build tasks* to Claude Code to write app code and open PRs. Same verb, different job.

---

## 1. The problem

Build work on S-BOS was happening inside long Claude Code chat sessions. Two failure modes:
- **Straying** — a session in the weeds rebuilds something that already exists (a real example: a duplicate `captures` bucket + `attachments` table when `document_library` already attached files to any record — see Build Decisions Log BD-04). A cloud/dispatched session has none of the chat's memory, so this is worse there.
- **No system-of-record for build tasks** — tasks, decisions, and status lived in a transient thread, not in the platform.

## 2. The loop

```
capture / compose a task
        │
        ▼
/portal/dev-dispatch  ──POST──▶  /api/kompass/dispatch  ──▶  code_dispatches (status='queued')
   (system_admin only)              (builds guardrailed prompt)
        │                                                          │
        │ "Launch in Claude Code" (deep-link)          "Queue it" (worker path)
        ▼                                                          ▼
  claude.ai/code cloud session                       scripts/dispatch-worker.mjs
  (any device; auto-loads CLAUDE.md)                 (always-on host / cron)
                                                                   │
                                                     isolated git worktree off origin/main
                                                     → claude CLI headless → commit
                                                     → git push + gh pr create
                                                                   │
                                                                   ▼
                                            PR opened  ·  status='pr_open' + pr_url written back
                                                                   │
                                                                   ▼
                                                    HUMAN REVIEWS + MERGES THE PR
```

### 2.1 In-app control surface — `/portal/dev-dispatch`
- Gated to `system_admin` / `executive` via `resolveActor()` (everyone else sees a "developer console only" notice).
- **Compose a task** (free text) or **pick an existing S-BOS 2.0 task**.
- Per dispatch: a **Launch in Claude Code** deep-link and, once the worker runs, a **PR** link + status pill.

### 2.2 The queue — `code_dispatches` + `/api/kompass/dispatch`
- `POST { taskId }` **or** `{ title }` (ad-hoc) → builds a fully guardrailed prompt and inserts a `code_dispatches` row (`status='queued'`).
- The prompt injects the last 20 `build_log` rows as **"RECENTLY BUILT — reuse, do not rebuild,"** plus orders the session to read `AGENTS.md` + `docs/ARCHITECTURE.md`, reuse existing infra, and open a PR.
- `PATCH { id, status, pr_url, branch, log }` is the worker callback.

### 2.3 Two ways to run
| Mode | How | When to use |
|---|---|---|
| **Launch** | Deep-link opens a `claude.ai/code` cloud session prefilled with task + repo + rules; auto-loads `CLAUDE.md`. Works from any device incl. the Claude mobile app. | You want to drive the session yourself, from anywhere. |
| **Queue → worker** | `scripts/dispatch-worker.mjs` on the always-on host drains the queue and opens PRs hands-off. | Fire-and-forget build tasks; batch overnight. |

### 2.4 The worker — `scripts/dispatch-worker.mjs`
Per queued dispatch it: (1) marks `running`; (2) creates an **isolated git worktree** off fresh `origin/main` (branch `dispatch/<id>-<slug>`); (3) runs the `claude` CLI **headless** in that worktree (agent reads the rules, makes the change, commits); (4) **pushes + opens a PR via `gh`**; (5) writes `status='pr_open'` + `pr_url` + `branch` back.

Run it:
```
npm run dispatch:work      # drain the queue once
npm run dispatch:watch     # poll continuously
node scripts/dispatch-worker.mjs --id <id>   # one specific dispatch
node scripts/dispatch-worker.mjs --dry       # plan only, no claude/PR
```
Requires `SBOS_ANTHROPIC_KEY` + `GITHUB_TOKEN` in gitignored `.env.local` (plus `gh` auth).

## 3. Guardrails (why it doesn't stray or go rogue)

1. **Rules live in the repo, not this chat.** `AGENTS.md` (operating rules; Rule 1 = *reuse before you build*), `docs/ARCHITECTURE.md` (the live PDD / built-state), and a self-contained `CLAUDE.md` that inlines the core rules and imports `AGENTS.md` (so cloud loaders that don't expand imports still get them). See BD-06.
2. **A "recently built" ledger.** The `build_log` table is injected into every dispatch prompt so the session reuses instead of rebuilding.
3. **PR-only, never merge.** The worker (and the deep-link prompt) always open a PR; a human approves. `.github/pull_request_template.md` is a governance checklist (reused-before-building, matches ARCHITECTURE.md, updated doc + build_log, focused, verified, PR-not-merge).
4. **Isolated worktree.** The worker never mutates the primary checkout.
5. **No unsupervised agent-spawns-agent.** The worker is intentionally left blocked from Claude Code's auto-mode — the classifier denies spawning an autonomous `--dangerously-skip-permissions` agent, so it must be run from an interactive terminal / cron host with a one-time approval.
6. **Convention:** anything substantial → append a `build_log` row + update `docs/ARCHITECTURE.md` in the same PR.

## 4. Credentials & security
- `SBOS_ANTHROPIC_KEY` (preferred over a possibly-stale ambient `ANTHROPIC_API_KEY`) and `GITHUB_TOKEN` live only in gitignored `.env.local` — never committed. `gh` keyring auth also works for PR creation.
- Any credential pasted into a chat should be **rotated** afterward (transcripts persist).

## 5. Open items / future
- **Fully-autonomous execution** could later use the Anthropic Managed Agents API / a GitHub App instead of a local `claude` CLI host — the queue contract (`code_dispatches` + PATCH callback) is already the seam for that swap.
- **Scheduling:** wire the worker into a cron on the always-on host (or the Schedule screen) for overnight drains.
- **Per-tenant isolation** (licensing): the same load-bearing pattern as the skill dispatcher — scope by `org_id` when multi-tenant separation is needed.
