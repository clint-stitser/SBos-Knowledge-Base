# Deployment Config: [App Name]

---

## 🛠️ Read Before Filling

> **This template is AI-optimized.** It contains agent-facing instruction blocks (🤖), pause markers (❓ AGENT PAUSE), and gates (🚦 GATE) that guide the writing agent. These get removed at the end. The cleanup verification checklist below tells you what to strip.

**Template version:** v1.0 (AI-optimized)
**Fill order in pipeline:** Coding-phase doc #6 of 8. Fills after Module Breakdown, API Contract, Migration Checklist, Component/Service Map, Testing Strategy. Fills before Mid-Build Review.

**Source docs (read these first — do not start writing until they are at ✅ Done):**
- `[AppName]_Technical_Spec.md` — pull from these sections:
  - **Tech Stack** → drives hosting platform choice, language runtime, container strategy
  - **Authentication & Authorization** → drives secret types in Secrets table
  - **Environment Variables** → every variable named there MUST appear in this doc's Environment Variables table (bidirectional)
  - **Deployment & Environments** → drives Environment Summary and per-environment sections
  - **Monitoring & Logging** → drives Monitoring & Alerting section
  - **Dependencies & Integrations** → drives external service env vars and secret entries
- `[AppName]_Module_Breakdown.md` — pull from these sections:
  - **Phase Plan** → drives which Modules deploy in which pipeline runs
  - **Module Registry** → Module IDs (M-XX) referenced in pipeline phase mapping
- `[AppName]_Database_Migration_Checklist.md` — pull from these sections:
  - **Migration Registry** → migration ID order drives CI/CD migration step order (bidirectional)
  - **Conventions** → migration tool / runner referenced in pipeline migration step
  - Each migration's **Reversibility declaration** → drives Rollback Policy and Rollback vs Fix-Forward decision tree
- `[AppName]_Testing_Strategy.md` — pull from these sections:
  - **CI/CD Integration** → pipeline test stages here MUST match Testing Strategy's declarations (bidirectional)
  - **Coverage Plans** → coverage thresholds referenced in PR Check Pipeline pass criteria

**Downstream consumers (write to feed them — every section is read by at least one of these):**
- **Mid-Build Review Template** → drift checks "operational health" against this doc's monitoring thresholds, alert rules, and environment configs
- **Phase Closeout Template** → per-phase deploys reference this doc's runbooks; rollback events get logged here and rolled up there
- **Project Closeout Template** → final operational state validated against this doc
- **CI/CD systems (GitHub Actions / etc.)** → the pipeline-as-code is generated from the CI/CD Pipeline section. The agent generating the YAML reads this doc as the spec.
- **On-call engineer / DevOps / coding agent operating a deploy** → reads Deployment Runbook and Rollback Procedure under time pressure. Clarity here is incident-response infrastructure.

**Agent role:** You are the scribe translating Tech Spec's environment definitions, Module Breakdown's phase plan, and Migration Checklist's reversibility declarations into a complete, executable CI/CD spec with runbooks. You are NOT inventing deployment infrastructure. You are NOT making architectural decisions about hosting platforms, CI tools, or rollback policy — those belong to Ryan. Your job is to capture his decisions at a precision level where a CI agent can generate pipeline YAML from this doc and a human on-call can execute rollback at 2am from this doc.

**The three rules (apply to every section):**
1. **Everything traces back to a source.** Every env var, every secret, every pipeline step, every alert rule traces to Tech Spec, Module Breakdown, Migration Checklist, Testing Strategy, or an explicit decision from Ryan. No invented operational config.
2. **If unclear, stop and ask.** Use `❓ AGENT PAUSE` mid-section. Do not guess hosting providers, secret rotation cadences, alert thresholds, or rollback authorities.
3. **Downstream-precision standard.** Output must be specific enough that:
   - A CI agent can generate `.github/workflows/*.yml` from the Pipeline section alone
   - An on-call engineer at 2am can execute the Rollback Procedure with no prior context
   - The Env Vars table is the single source of truth — `.env.example`, secret store entries, and runtime config all derive from it

**Two failure modes this doc is designed to prevent (referenced explicitly at gates):**
- **Silent environment drift** — dev / staging / prod diverge in config (env var values, dependencies, infra) without documentation. Causes "works on staging, fails in prod" surprises. Addressed by canonical Env Vars table + per-environment sections + Gate 2 bidirectional check against Tech Spec.
- **Deploy/rollback asymmetry** — deploy steps are documented but rollback procedure has gaps. Common patterns: irreversible migration with no fix-forward plan, secret rotation that can't be undone, missing rollback authority, no time-window rule. Addressed by Reversibility declarations on every deploy step + Rollback Authority table + Manual Rollback Steps with time-boxing + Gate 1 foundation lock.

**Internal fill order (sections depend on each other — fill in this order):**
1. Overview
2. Environment Summary (per-environment one-liners — detail comes later)
3. Environment Variables (canonical table — drives `.env.example` and Secrets)
4. Secrets Management (rotation policy + access patterns)
5. Rollback Authority & Policy (lives inside Rollback Procedure, but the **Authority table + Rollback-vs-Fix-Forward rules** lock at Gate 1, ahead of Manual Steps)
6. **🚦 Gate 1 — Foundation Lock**
7. CI/CD Pipeline — Triggers
8. CI/CD Pipeline — PR Check Pipeline
9. CI/CD Pipeline — Staging Deploy Pipeline
10. CI/CD Pipeline — Production Deploy Pipeline
11. Environment Configs — Dev / Staging / Production (full per-environment detail)
12. Deployment Runbook (Pre-Deploy / Deploy Steps / Post-Deploy)
13. Rollback Procedure — Decision Tree
14. Rollback Procedure — Manual Rollback Steps
15. Rollback Procedure — Fix-Forward Steps
16. Rollback Procedure — After a Rollback or Hotfix
17. Monitoring & Alerting
18. Feature Flags (or explicitly "none")
19. Open Questions
20. **🚦 Gate 2 — Full Sign-Off**

**When fully filled and approved — cleanup verification:**
- [ ] No `🤖` blocks remain — search for the string `🤖` and verify zero hits
- [ ] No `❓ AGENT PAUSE` markers remain — search for `❓ AGENT PAUSE` and verify zero hits
- [ ] All `🚦 GATE` blocks have agent-instruction prose removed; only the **checklist + human sign-off line** remain
- [ ] All `> Remove this block before delivering the filled doc.` strings removed
- [ ] All `[bracketed placeholders]` filled or explicitly marked `N/A` with rationale
- [ ] All references to `[AppName]` replaced with the real app name

> Remove this entire 🛠️ Read Before Filling section before delivering the filled doc.

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Overview | 🔲 Not Started | — | — |
| Environment Summary | 🔲 Not Started | — | — |
| Environment Variables | 🔲 Not Started | — | — |
| Secrets Management | 🔲 Not Started | — | — |
| Rollback Authority & Policy | 🔲 Not Started | — | Locks at Gate 1 |
| **🚦 Gate 1 — Foundation Lock** | 🔲 Not Started | Ryan | Sign off before pipeline stages begin |
| CI/CD Pipeline — Triggers | 🔲 Not Started | — | — |
| CI/CD Pipeline — PR Check | 🔲 Not Started | — | — |
| CI/CD Pipeline — Staging Deploy | 🔲 Not Started | — | — |
| CI/CD Pipeline — Prod Deploy | 🔲 Not Started | — | — |
| Environment Configs — Dev | 🔲 Not Started | — | — |
| Environment Configs — Staging | 🔲 Not Started | — | — |
| Environment Configs — Production | 🔲 Not Started | — | — |
| Deployment Runbook | 🔲 Not Started | — | — |
| Rollback — Decision Tree | 🔲 Not Started | — | — |
| Rollback — Manual Steps | 🔲 Not Started | — | — |
| Rollback — Fix-Forward Steps | 🔲 Not Started | — | — |
| Rollback — After | 🔲 Not Started | — | — |
| Monitoring & Alerting | 🔲 Not Started | — | — |
| Feature Flags | 🔲 Not Started | — | If not using flags, explicitly state "none" |
| Open Questions | 🔲 Not Started | — | — |
| **🚦 Gate 2 — Full Sign-Off** | 🔲 Not Started | Ryan | Sign off before Mid-Build Review |

**Status values:** 🔲 Not Started / 🔄 In Progress / 👀 In Review / ✅ Done / 🚫 Blocked

---

## Overview

> 🤖 **AGENT INSTRUCTIONS — Overview**
>
> **Your job:** Capture the deployment context in 6 fields. Each field is a single decision pulled from Tech Spec or Ryan.
>
> **What a complete answer covers:**
> - App name (real, not `[App Name]`)
> - Source-doc filenames (already named — fill in the `[AppName]_` prefix with the real app name)
> - Hosting platform — one specific provider (Render / Railway / Fly.io / AWS / Vercel / etc.)
> - CI/CD tool — one specific tool (GitHub Actions / CircleCI / GitLab CI / etc.)
> - Container strategy — Docker / serverless / native runtime / none
> - Environments — explicit list (every env named here gets a row in Environment Summary and its own per-environment config block)
>
> **What incomplete looks like:**
> - "TBD" or "[—]" left in any field at Gate 1
> - "Multiple options" in any field — pick one, move others to Open Questions
> - Environment list says "dev / prod" but no staging — confirm with Ryan; most non-trivial apps need staging
>
> **Ask triggers (use `❓ AGENT PAUSE`):**
> - Tech Spec → Tech Stack does not name a hosting platform → ask
> - Tech Spec → Deployment & Environments lists environments inconsistent with what Ryan is proposing → ask
> - Ryan proposes a CI tool that doesn't match the hosting platform's typical integration → confirm, do not silently switch
>
> **Cross-reference checklist:**
> - Source docs section in 🛠️ banner ↔ this Overview's source docs lines (same filenames)
> - Tech Spec → Tech Stack named the runtime → matches container strategy choice here
>
> Remove this block before delivering the filled doc.

- **App:** [App Name]
- **Source docs:**
  - Tech Spec: `[AppName]_Technical_Spec.md`
  - Module Breakdown: `[AppName]_Module_Breakdown.md`
  - Database Migration Checklist: `[AppName]_Database_Migration_Checklist.md`
  - Testing Strategy: `[AppName]_Testing_Strategy.md`
- **Hosting platform:** [Render / Railway / Fly.io / AWS / Vercel / etc.]
- **CI/CD tool:** [GitHub Actions / CircleCI / etc.]
- **Container strategy:** [Docker / serverless / native runtime / none]
- **Environments:** [Dev / Staging / Production — list all that exist]

---

## Environment Summary

> 🤖 **AGENT INSTRUCTIONS — Environment Summary**
>
> **Your job:** One row per environment named in Overview. This is the index — full detail lives in Environment Configs further down.
>
> **What a complete answer covers:**
> - Every environment from Overview's list has a row here
> - URL, auto-deploy trigger, DB binding identified for each
> - Notes column flags any non-obvious behavior (e.g., "staging mirrors production config", "production requires approval gate")
>
> **What incomplete looks like:**
> - URL left as `[—]` after Gate 1 (acceptable pre-Gate 1 if hosting isn't provisioned yet; flag as Open Question)
> - "Auto-deploy trigger" column says "manual" for all environments — verify with Ryan; most teams want at least staging auto-deployed on main merge
> - DB column unclear about whether environments share a DB instance (they should not — flag if so)
>
> **Ask triggers:**
> - Ryan proposes shared DB between staging and prod → STOP. This is dangerous and almost always a misunderstanding. Confirm.
> - Auto-deploy trigger differs from Tech Spec → Deployment & Environments → reconcile, do not silently choose.
>
> **Cross-reference checklist:**
> - Every environment listed here gets its own block in Environment Configs section
> - Every environment listed here gets a column in the Environment Variables table
> - Every environment listed here is referenced in CI/CD Pipeline triggers/targets
>
> Remove this block before delivering the filled doc.

| Environment | Purpose | URL | Auto-deploy trigger | DB | Notes |
|-------------|---------|-----|--------------------|----|-------|
| Development | Local dev | `localhost:[port]` | N/A — manual | Local / Docker | Each dev runs their own |
| Staging | Pre-prod testing | [—] | Push to `main` | Staging instance (separate from prod) | Mirrors production config |
| Production | Live app | [—] | Tagged release / manual approval | Production instance | Requires approval gate |

---

## Environment Variables

> 🤖 **AGENT INSTRUCTIONS — Environment Variables**
>
> **Your job:** Build the canonical table of every environment variable the app uses. This table is the source of truth — `.env.example`, secret store entries, and per-environment configs all derive from it.
>
> **Strict ordering rule — read first:** Tech Spec → Environment Variables section is the upstream source. Open it, walk it row by row, transcribe every variable. If you find a variable referenced in Tech Spec's code examples or API auth section that is NOT in its Env Vars list, flag it as a Tech Spec gap — do not silently invent the entry here.
>
> **What a complete answer covers:**
> - Every env var from Tech Spec → Environment Variables appears as a row here (bidirectional check at Gate 2)
> - Every secret named in Tech Spec → Authentication & Authorization appears here AND in the Secrets Management table
> - Required vs. optional explicit per variable
> - One column per environment listed in Environment Summary
> - Description column states what the variable controls AND where it's used (which module/service)
> - Dev defaults are real, runnable values (e.g., `dev-secret-change-me` is fine for a dev default; empty string is not)
> - Staging/Prod columns are EITHER a real value (non-sensitive config like `NODE_ENV`) OR the literal string `[Managed secret — see Secrets Management]` for sensitive values
>
> **What incomplete looks like:**
> - Real production secrets pasted into the Prod column (CRITICAL FAILURE — secrets never live in this doc; rotate immediately if this happens)
> - "Description" column says only "DB URL" without naming what uses it
> - Required column blank — every variable is Yes or No
> - A variable appears in `.env.example` but not in this table (or vice versa)
>
> **Ask triggers:**
> - Tech Spec lists an env var with no description → ask Ryan what it controls before transcribing
> - Tech Spec lists an env var but doesn't say which environments need it → ask
> - Ryan proposes adding a variable not in Tech Spec → STOP. Add it to Tech Spec first, then transcribe here. Do not let this doc drift ahead of Tech Spec.
>
> **Cross-reference checklist:**
> - Every row here ↔ a row in Tech Spec → Environment Variables (bidirectional)
> - Every row marked sensitive (i.e., Prod column = `[Managed secret]`) ↔ a row in Secrets Management table
> - The `.env.example` file in the repo ↔ this table (kept in sync — flag any drift in Mid-Build Review)
>
> Remove this block before delivering the filled doc.

| Variable | Required? | Dev Default | Staging | Prod | Description |
|----------|-----------|-------------|---------|------|-------------|
| `NODE_ENV` | Yes | `development` | `staging` | `production` | Controls log level, error verbosity, feature flag defaults. Used in: all services. |
| `PORT` | No | `3000` | `3000` | `[platform-assigned]` | Server listen port. Platform typically injects this. Used in: server bootstrap. |
| `DATABASE_URL` | Yes | `postgresql://user:pass@localhost:5432/[dbname]` | `[Managed secret]` | `[Managed secret]` | Primary DB connection string. Used in: ORM / migration runner. |
| `JWT_SECRET` | Yes | `dev-secret-change-me` | `[Managed secret]` | `[Managed secret]` | JWT signing secret — must be long, random, unique per env. Used in: auth middleware. |
| `JWT_EXPIRES_IN` | No | `7d` | `7d` | `1d` | Token expiry. Prod should be shorter than dev/staging. Used in: auth service. |
| `[VAR_NAME]` | [Yes / No] | [Dev value or `—`] | [Value or `[Managed secret]`] | [Value or `[Managed secret]`] | [What it does. Used in: which module/service.] |

> **`.env.example` must stay in sync with this table.** It contains placeholder values only — never real secrets. Mid-Build Review checks this.

---

## Secrets Management

> 🤖 **AGENT INSTRUCTIONS — Secrets Management**
>
> **Your job:** Document how every sensitive value listed in the Env Vars table is stored, accessed at runtime, and rotated.
>
> **What a complete answer covers:**
> - Secret store named (one specific tool — GitHub Actions Secrets / AWS Secrets Manager / Doppler / Vault / 1Password / etc.)
> - Access pattern explicit (injected as env vars at deploy time / SDK call at runtime / etc.)
> - Rotation table has a row for every `[Managed secret]` entry in the Env Vars table (bidirectional)
> - Each rotation row has: frequency, who, and **how to rotate without downtime**
> - Hard rules section preserved as-is
>
> **What incomplete looks like:**
> - "Rotation frequency: TBD" on any row at Gate 1
> - "How to rotate without downtime" left blank or says "redeploy" without specifying whether the app supports dual-secret transition
> - The rotation table has fewer rows than there are sensitive env vars (bidirectional check fails)
>
> **Ask triggers:**
> - Ryan hasn't decided on a secret store → STOP. This is a Gate 1 blocker. Surface as Open Question and do not proceed past Gate 1 until decided.
> - A sensitive env var has no rotation answer → ask Ryan: "What's the rotation cadence and procedure for `[VAR]`? If never rotated, state 'No rotation — used only for [reason]' explicitly."
> - JWT or session signing secret has rotation policy "On breach" only → push back. Periodic rotation (quarterly minimum) is best practice; on-breach-only leaves a long-lived attack window.
>
> **Cross-reference checklist:**
> - Every row in this rotation table ↔ a `[Managed secret]` row in Env Vars (bidirectional)
> - The "Hard rules" list survives into the final doc verbatim — it is policy, not advice
>
> Remove this block before delivering the filled doc.

**Secret store:** [GitHub Actions Secrets / AWS Secrets Manager / Doppler / Vault / 1Password / etc.]

**Access pattern:** [How the app retrieves secrets at runtime — injected as env vars at deploy time / SDK call at runtime / mounted volume / etc.]

**Rotation policy:**

| Secret | Rotation frequency | Who rotates | How to rotate without downtime |
|--------|-------------------|-------------|-------------------------------|
| `JWT_SECRET` | [Quarterly minimum, or on breach] | [Role] | [e.g., Rolling deploy with both old and new key valid during transition window] |
| `DATABASE_URL` | [On rotation event / on breach] | [Role] | [Update in secret store, redeploy services in order: workers → API → web] |
| `[SECRET]` | [—] | [—] | [—] |

**Hard rules:**
- No secrets in code, comments, or logs — ever
- No secrets in this doc — only references to where they live
- If a secret is accidentally committed: rotate immediately, then purge from history (deleting the commit is NOT sufficient)
- Developers use `.env.local` (gitignored) for local secrets — never `.env`
- CI logs must redact secrets — verify your CI tool's redaction is working before merging the first pipeline change

---

## Rollback Authority & Policy

> 🤖 **AGENT INSTRUCTIONS — Rollback Authority & Policy**
>
> **Your job:** Lock the rollback decision-making structure BEFORE pipeline stages and manual procedures are written. This is the "who decides what when things go wrong" foundation that everything else in Rollback Procedure depends on.
>
> **Why this lives here, not later:** The Rollback-vs-Fix-Forward rules and Authority table are referenced inside the CI/CD pipeline's failure-handling logic. If we write the pipeline first and the authority later, the pipeline gets the references wrong and we rewrite both. This is the same foundation-lock pattern as Migration Checklist Gate 1.
>
> **What a complete answer covers:**
> - Authority table with named role for each decision (Call rollback / Approve DB migration rollback / Notify)
> - Fallback named for every primary authority — no single point of failure
> - Time-window rule explicit (default 30 min; adjust if Ryan has a different operational tempo)
> - Rollback-vs-Fix-Forward situation table covers at minimum: smoke-test failure, critical bug within window, non-critical bug after window, app down / data at risk, irreversible migration
> - One-line rule about who calls it (no committee decisions)
>
> **What incomplete looks like:**
> - Authority column says "the team" — name a role
> - Fallback column blank — every primary needs a fallback
> - Time window left as `[X minutes]` at Gate 1 — pick a number
> - "Irreversible migration" row missing — this is the most common rollback failure mode and must be addressed
>
> **Ask triggers:**
> - Ryan is the only person on the team → confirm: he is both Primary and Fallback for every row. State that explicitly rather than leaving it ambiguous.
> - Ryan proposes "rollback only with consensus" → push back. Incidents are time-boxed; one person calls it.
> - Time window proposed > 60 min → confirm. Long windows mean more accumulated state that rollback can't undo cleanly.
>
> **Cross-reference checklist:**
> - The "irreversible migration" rule here ↔ Migration Checklist's reversibility declarations on each migration
> - The Authority table's "Approve DB migration rollback" row ↔ Migration Checklist's rollback procedures (whoever owns that there must match here)
> - "Notify" channel here ↔ Monitoring & Alerting alert channels (alerts route to the same place rollback notifications go, so on-call sees both)
>
> Remove this block before delivering the filled doc.

> **Read this before you need it.** A rollback under pressure with no documented plan takes 3× longer and causes additional mistakes. Fill this in before first production deploy.

### Authority Table

| Decision | Primary authority | Fallback if unavailable |
|----------|-------------------|------------------------|
| Call the rollback | [Role / name — e.g., "On-call engineer"] | [Role / name] |
| Approve rollback of a DB migration | [Role / name — e.g., "Tech lead"] | [Role / name] |
| Notify on rollback | [Slack channel / role / person] | [—] |

> **Rule:** One person calls it. No committee decisions during an incident. If the on-call can't reach the approver within [5] minutes, they proceed with rollback and notify async.

### Rollback vs. Fix Forward — Situation Rules

> The first decision after a bad deploy. Get this wrong and you make the incident worse.

| Situation | Action |
|-----------|--------|
| Smoke tests fail immediately after deploy | Rollback immediately — don't investigate first |
| Critical bug found within [30 min] of deploy | Rollback unless a DB migration is irreversible (see below) |
| Non-critical bug found after [30 min] of traffic | Fix forward — rollback disrupts more users than the bug |
| App is down or data is at risk | Rollback immediately regardless of time elapsed |
| **DB migration is irreversible** | **Do NOT rollback app — fix forward only.** Rolling back the app with the new schema breaks the DB. |

> **Time window rule:** After [30 minutes] of live production traffic, default to fix-forward. The rollback window isn't infinite — the longer the app runs on the new version, the more state accumulates that rollback won't account for cleanly.

---

## 🚦 GATE 1 — Foundation Lock

> 🤖 **AGENT INSTRUCTIONS — Gate 1**
>
> **Why this gate matters:** Environments, env vars, secrets, and rollback authority govern every downstream section in this doc. Changing any of them after pipeline stages and runbooks are written means rewriting every affected step. Lock these first.
>
> **What you (the agent) verify before requesting sign-off:**
> - Foundation completeness — every Gate 1 section is at ✅ Done with no `[bracketed placeholders]` and no `[—]` left
> - Bidirectional consistency — every check below has been walked, both directions, with the named source doc open
> - Open Questions captured — anything Ryan punted on is in the Open Questions section, not left implicit
>
> **What human sign-off means:** Ryan has personally verified the four foundation sections and accepts that downstream sections will be written against them. Any change to a Gate 1 section after sign-off requires re-validation of every downstream section that references it.
>
> Remove this instruction block before delivering. Keep the checklist and sign-off line.

**Foundation completeness:**
- [ ] Overview — every field filled, no TBD
- [ ] Environment Summary — every env from Overview has a row
- [ ] Environment Variables — every var from Tech Spec → Environment Variables transcribed
- [ ] Secrets Management — every `[Managed secret]` row in Env Vars has a rotation entry; secret store named
- [ ] Rollback Authority & Policy — Authority table fully named (no "TBD"), time window set, Situation rules cover all 5 baseline cases

**Bidirectional consistency checks (open the source doc and walk each direction):**
- [ ] **Env Vars ↔ Tech Spec → Environment Variables** — every var in Tech Spec appears here; every var here appears in Tech Spec. No orphans either direction.
- [ ] **Env Vars (sensitive rows) ↔ Secrets Management rotation table** — every `[Managed secret]` in Env Vars has a rotation row; every rotation row matches an Env Vars sensitive entry.
- [ ] **Env Vars ↔ `.env.example`** — `.env.example` file in repo has a placeholder for every Env Vars row (or note that `.env.example` doesn't exist yet and add it as an Open Question).
- [ ] **Rollback Authority → "Approve DB migration rollback" row ↔ Migration Checklist rollback ownership** — same role/person referenced in both places.

**Open Questions cleared (block status):**
- [ ] Every Open Question that affects Gate 1 sections is resolved or explicitly deferred with a "can proceed without this" note from Ryan

**Failure modes check:**
- [ ] No path identified that leads to **silent environment drift** — dev / staging / prod configs are explicitly distinct in the Env Vars table and the per-environment plan is consistent with Tech Spec
- [ ] No path identified that leads to **deploy/rollback asymmetry** — every sensitive operation in the foundation has a documented rollback authority

**Human sign-off:**
- [ ] Ryan: ☐ Approved — foundation locked. Pipeline stages and runbooks may begin.

---

## CI/CD Pipeline

> 🤖 **AGENT INSTRUCTIONS — CI/CD Pipeline (whole section)**
>
> **Your job:** Translate Module Breakdown's Phase Plan, Migration Checklist's Registry order, and Testing Strategy's CI/CD Integration into a complete, executable pipeline definition. This is the spec from which `.github/workflows/*.yml` (or equivalent) is generated.
>
> **Strict ordering rule — read first:** Fill Triggers first. Then PR Check. Then Staging Deploy. Then Production Deploy. Each subsequent stage is a superset of the previous. Do not invent steps in later stages that should have been in earlier stages.
>
> **What a complete answer covers (for each pipeline sub-section):**
> - Every step has a tool named (not "linter" — name the linter)
> - Every step has explicit pass criteria (exit code, percentage, output match — measurable, not "looks good")
> - Every step has an explicit on-failure action (abort, block merge, alert + keep previous deploy, rollback)
> - Every step that involves a Module ID, Migration ID, or Test ID references it explicitly (M-XX, DB-XX, AC-XX, etc.)
> - **Reversibility declaration on every Production Deploy step** — one of: `Reversible — auto-rollback handles it`, `Reversible with caveats — see Rollback Runbook step N`, `Non-reversible — fix-forward only`
>
> **What incomplete looks like:**
> - "Run tests" as a step — name which tests (unit, integration, AC contract, etc.) and which threshold
> - "Deploy" without naming the command, CLI, or API call
> - Migration step in pipeline that doesn't reference DB-XX IDs from Migration Checklist
> - Pipeline test stages that don't match Testing Strategy → CI/CD Integration declarations
> - Production deploy step with no reversibility declaration
>
> **Ask triggers:**
> - Module Breakdown's Phase Plan groups modules in a way that conflicts with the deploy strategy here → reconcile, do not silently override
> - Migration Checklist Registry order says DB-03 → DB-04 but pipeline runs DB-04 → DB-03 → STOP. Migration order is sacred; pipeline matches Registry.
> - Testing Strategy declares a pipeline test stage (e.g., AC contract tests) that this pipeline omits → ask Ryan: should Testing Strategy be revised, or this pipeline updated? Don't silently drop the stage.
>
> **Cross-reference checklist (Gate 2 will verify all of these):**
> - **Pipeline migration steps ↔ Migration Checklist Registry order** — exact same order, exact same IDs
> - **Pipeline test stages ↔ Testing Strategy → CI/CD Integration** — every stage declared there appears here (and reverse)
> - **Pipeline phase mapping ↔ Module Breakdown Phase Plan** — which Modules deploy in which pipeline runs
> - **Pipeline coverage thresholds ↔ Testing Strategy → Coverage Plans** — same numbers
> - **Production Deploy reversibility declarations ↔ Migration Checklist reversibility declarations** — consistent
>
> Remove this block before delivering the filled doc.

### Triggers

| Trigger | Pipeline | Target environment |
|---------|----------|--------------------|
| Push to any branch | PR checks (lint + test + build) | None — checks only |
| PR merged to `main` | Full staging pipeline | Staging |
| Git tag `v*` / manual approval | Release pipeline | Production |

---

### PR Check Pipeline

> Runs on every push to a non-`main` branch. Must pass before merge.

| Step | Tool | Pass criteria | On failure |
|------|------|---------------|------------|
| 1. Checkout | Git | Repo cloned at commit SHA | Abort |
| 2. Install dependencies | [npm / yarn / pnpm] | Exit 0; lockfile unchanged | Abort |
| 3. Lint | [ESLint / Prettier / Biome] | Zero errors | Block merge |
| 4. Type check | [tsc / etc.] | Zero errors | Block merge |
| 5. Unit tests (UT-XX) | [Jest / Vitest] | All pass; coverage ≥ [X]% per Testing Strategy → Coverage Plans | Block merge |
| 6. Integration tests (IT-XX) | [—] | All pass | Block merge |
| 7. API contract tests (AC-XX) | [—] | All pass | Block merge |
| 8. Build | [—] | Exit 0; artifact produced | Block merge |

---

### Staging Deploy Pipeline

> Runs on merge to `main`. Deploys to staging automatically.

| Step | Tool | Pass criteria | On failure |
|------|------|---------------|------------|
| 1–8. All PR Check steps | (same as above) | All pass | Abort deploy |
| 9. Build Docker image / artifact | [Docker / etc.] | Exit 0; image tagged with commit SHA | Abort |
| 10. Push to registry | [Docker Hub / ECR / GHCR] | Pushed successfully | Abort |
| 11. Run DB migrations (staging) — `DB-[NN..NN]` per Migration Checklist Registry order | [Migration runner per Migration Checklist Conventions] | Each migration's status updates to ✅ Applied | Abort + alert; do NOT proceed to deploy |
| 12. Deploy to staging | [Platform CLI / API call] | Deploy confirmed; health check returns 200 | Abort + alert |
| 13. Smoke tests (post-deploy) | [Specific smoke test suite ID per Testing Strategy] | Key endpoints return 200; auth flow succeeds | Alert + keep previous deploy artifact |
| 14. E2E tests on staging (E2E-XX) | [Playwright / Cypress / etc.] | All pass | Alert; do not block prod path automatically — Ryan decides |
| 15. Notify | [Slack channel / email] | Notification sent | Log only |

---

### Production Deploy Pipeline

> Triggered by Git tag `v*` or manual approval. Requires explicit gate before deploy.

> **Reversibility column note:** Every step below declares its reversibility. This is the deploy-side analogue of Migration Checklist's reversibility declarations. Do not delete the column; do not leave it blank.

| Step | Tool | Pass criteria | On failure | **Reversibility** |
|------|------|---------------|------------|-------------------|
| 1–10. All Staging Deploy steps (1–10) | (same as above) | All pass | Abort | N/A — pre-deploy |
| 11. Manual approval gate | [GitHub Environments / etc.] | Approved by [role per Rollback Authority table] | Abort | N/A — pre-deploy |
| 12. Run DB migrations (production) — `DB-[NN..NN]` per Migration Checklist Registry order | [Migration runner] | Each migration's status updates to ✅ Applied; backup snapshot taken pre-migration | Abort + alert + rollback (if migrations are reversible per Checklist) | **Per migration's reversibility declaration in Migration Checklist — do not aggregate. Some migrations may be reversible, some not.** |
| 13. Deploy to production | [Platform CLI / API call] | Deploy confirmed; health check returns 200 | Rollback (auto if reversible) + alert | **Reversible — auto-rollback to previous image tag** |
| 14. Smoke tests (post-deploy) | [Smoke test suite] | Key endpoints return 200; auth flow succeeds; critical workflows complete | Rollback + alert | N/A — verification only |
| 15. Tag release in monitoring | [Datadog / Sentry / etc.] | Release marker created | Log only | N/A — metadata only |
| 16. Post-deploy verification window | Wait + monitor for [15 min] | No spike in error rate (> [X]%), no spike in p95 latency (> [X]ms) | Rollback if breach | **Reversible within [30 min] window; fix-forward after** |
| 17. Notify | [Slack channel / email] | Notification sent | Log only | N/A — comms |

---

## Environment Configs

### Development

> 🤖 **AGENT INSTRUCTIONS — Dev Environment Config**
>
> **Your job:** Capture the local dev environment in enough detail that a new developer (or coding agent) can run the app locally from a fresh clone in under 15 minutes.
>
> **What a complete answer covers:** Setup command, DB strategy, seed command, env file convention, hot reload tool, port assignments, dev-specific quirks (stubbed services, disabled emails, etc.)
>
> **Ask triggers:** Tech Spec → Dependencies & Integrations names an external service (Stripe, SendGrid, etc.) with no dev-mode strategy → ask Ryan: real keys / sandbox keys / stubbed locally?
>
> Remove this block before delivering the filled doc.

- **Purpose:** Local development
- **Setup command:** `[e.g., npm run dev]`
- **Database:** Local PostgreSQL or Docker (`docker-compose up`)
- **Seed data:** `[e.g., npm run db:seed]` — pulls from `[AppName]_Sample_Data.md`
- **Env file:** `.env.local` (gitignored — each dev maintains their own; `.env.example` is the template)
- **Hot reload:** [Yes / No — tool]
- **Ports:** App `[3000]`, DB `[5432]`, [other services]
- **External services:** [Stripe sandbox / SendGrid sandbox / stubbed locally — per service]
- **Notes:** [Any dev-specific quirks — e.g., "email sending disabled; logs to console instead"]

---

### Staging

> 🤖 **AGENT INSTRUCTIONS — Staging Environment Config**
>
> **Your job:** Specify staging precisely enough that someone provisioning it from scratch can match production's surface area without provisioning prod itself.
>
> **What a complete answer covers:** Platform, region, URL, deploy trigger, DB (separate from prod), backup policy, data strategy (anonymized prod / synthetic / seed), access controls, staging-specific config differences from prod
>
> **Failure mode to avoid (silent environment drift):** Staging must mirror production's stack and config. Differences from prod must be documented explicitly — anything not documented is assumed identical to prod and gets caught by mid-build drift checks.
>
> **Ask triggers:** Staging proposed to share a DB with prod → STOP. Confirm.
>
> Remove this block before delivering the filled doc.

- **Purpose:** Pre-production testing, QA, stakeholder review
- **Platform:** [Render / Railway / Fly.io / etc.]
- **Region:** [—]
- **URL:** [—]
- **Deploy trigger:** Merge to `main` (auto)
- **Database:** [Managed instance — provider, size, separate from prod, backups enabled]
- **Backups:** [Yes / No — frequency]
- **Data:** [Anonymized prod snapshot / synthetic / empty + seeded]
- **Access:** [Who can access staging? Authenticated? VPN? Allowlist?]
- **Documented differences from prod:** [Email sends to test inbox; rate limits relaxed; analytics disabled; etc. — explicit list]
- **Notes:** [Anything else]

---

### Production

> 🤖 **AGENT INSTRUCTIONS — Production Environment Config**
>
> **Your job:** Specify production with the precision required for a runbook author and an incident responder. This section is the operational reference under pressure.
>
> **What a complete answer covers:** Platform, region(s), URL, deploy trigger, DB (with HA / backup / restore-tested), scaling policy, CDN, SSL, access controls, compliance / data residency
>
> **Ask triggers:**
> - "Backups: yes" without restore-tested date → ask Ryan: when was the last restore tested? Untested backups are not backups.
> - HA not specified → ask: single instance or replicated? Single instance is acceptable for early-stage; just be explicit.
> - Compliance section blank → ask: HIPAA / PCI / GDPR / none? Even "none" must be explicit.
>
> Remove this block before delivering the filled doc.

- **Purpose:** Live app — real users, real data
- **Platform:** [—]
- **Region:** [Primary + any failover]
- **URL:** [—]
- **Deploy trigger:** Tagged release (`v*`) with manual approval
- **Database:** [Managed instance — provider, size, high availability config]
- **Backups:** [Frequency, retention, last restore tested on YYYY-MM-DD]
- **Scaling:** [Manual / auto-scale — rules]
- **CDN:** [Yes / No — provider, what's cached]
- **SSL:** [Managed by platform / Let's Encrypt / custom cert + renewal cadence]
- **Access:** [Production access controls — who can deploy, who can access DB directly, audit log location]
- **Compliance:** [HIPAA / PCI / GDPR / SOC2 / none — explicit; affects logging, backups, data residency]
- **Data residency:** [Region constraints if any]
- **Notes:** [Anything else]

---

## Deployment Runbook

> 🤖 **AGENT INSTRUCTIONS — Deployment Runbook**
>
> **Your job:** Write the human-execution steps that complement the automated pipeline. This is what a human (or coding agent) actually does to ship a release, including the manual gate moments.
>
> **What a complete answer covers:**
> - Pre-deploy checklist — verifiable items, not vague
> - Deploy steps — exact commands, exact URLs, exact buttons (no "go to the platform and deploy")
> - Post-deploy verification — explicit items with measurable pass criteria
>
> **What incomplete looks like:**
> - "Approve the deploy" without naming where (GitHub Environments → which env → which workflow)
> - "Monitor logs" without saying where the logs are or what to look for
> - Post-deploy checklist items that can't be ticked because they're vague ("check things work")
>
> **Ask triggers:**
> - No human-facing approval gate exists yet → ask Ryan: who approves prod deploys, and where do they click?
> - Pre-deploy includes "notify team" but no channel named → ask
>
> **Cross-reference checklist:**
> - Deploy steps reference the same pipeline stages defined in CI/CD Pipeline → Production Deploy
> - Post-Deploy Verification items align with Smoke Tests (step 14) and Verification Window (step 16) from Prod Pipeline
> - Rollback trigger conditions match Rollback Procedure → Decision Tree exactly
>
> Remove this block before delivering the filled doc.

### Pre-Deploy Checklist

- [ ] All tests passing on `main`
- [ ] DB migrations reviewed — every migration has a reversibility declaration in Migration Checklist; no destructive operations without a documented fix-forward plan
- [ ] `.env.example` is up to date with all new vars (matches this doc's Env Vars table)
- [ ] Notify team of deploy window in [Slack channel]
- [ ] Confirm staging deploy was verified by [role per Rollback Authority]
- [ ] Changelog / release notes written
- [ ] Backup snapshot of production DB confirmed (within last [24] hours)

### Deploy Steps

1. [Create and push Git tag: `git tag v[X.Y.Z] && git push origin v[X.Y.Z]`]
2. [Go to GitHub Actions → Release Pipeline → wait for staging steps to complete → approve the manual gate]
3. [Monitor deploy logs in [link to CI run]; abort if any step fails]
4. [After deploy completes: verify smoke tests pass in [monitoring dashboard URL]]
5. [Confirm in [Slack channel]: "Deploy v[X.Y.Z] complete. Monitoring for [15 min]."]

### Post-Deploy Verification

- [ ] App is reachable at production URL (`curl -I [URL]` returns 200)
- [ ] `GET /health` returns `200 OK { "status": "ok" }`
- [ ] Auth flow works (login + access protected route)
- [ ] [Critical user workflow 1] completes end-to-end
- [ ] [Critical user workflow 2] completes end-to-end
- [ ] No spike in error rate over [15 min] in [monitoring dashboard]
- [ ] No spike in p95 latency over [15 min] in [monitoring dashboard]
- [ ] DB migrations applied correctly — query migration tracking table; every DB-XX from this release shows ✅ Applied

---

## Rollback Procedure

### Decision Tree

> 🤖 **AGENT INSTRUCTIONS — Decision Tree**
>
> **Your job:** Render the Rollback-vs-Fix-Forward rules from Gate 1 as an explicit decision tree the on-call engineer walks under pressure.
>
> **What a complete answer covers:** Every situation row from the Authority & Policy section is reachable from the decision tree. The tree branches on observable conditions (smoke test status, time elapsed, migration reversibility) — not subjective judgment.
>
> **What incomplete looks like:** Tree branches into "investigate" without an explicit next-step path. Investigation is not an action — the tree must lead to either "Rollback" or "Fix Forward" in every leaf.
>
> Remove this block before delivering the filled doc.

```
Deploy completed?
├── No → Pipeline aborted before deploy started → No rollback needed. Fix and redeploy.
│
└── Yes → App deployed but something is wrong
    │
    ├── Is the DB migration in this release reversible? (Check Migration Checklist Reversibility column)
    │   ├── No → Fix forward ONLY. Do not rollback app. See Fix-Forward steps below.
    │   └── Yes → Rollback is an option. Continue tree.
    │
    ├── Smoke tests failed immediately → Rollback immediately (see Manual Rollback Steps)
    │
    ├── Problem discovered post-deploy
    │   ├── Within [30 min] rollback window?
    │   │   ├── Yes → Rollback (see Manual Rollback Steps)
    │   │   └── No  → Fix forward (see Fix-Forward Steps)
    │   └── App down / data at risk → Rollback immediately regardless of window
    │
    └── All clear → No action needed. Update incident log if any alerts fired.
```

---

### Manual Rollback Steps

> 🤖 **AGENT INSTRUCTIONS — Manual Rollback Steps**
>
> **Your job:** Write the exact step-by-step commands an on-call engineer executes to roll back a production deploy. Assume the executor is tired, under pressure, and has no prior context.
>
> **What a complete answer covers:**
> - Every step has an exact command, URL, or click target
> - Every step has expected output to confirm success
> - Every step has a time estimate; if any step exceeds it, escalate
> - Notification steps name the channel and provide a template message
> - DB migration rollback references Migration Checklist's rollback procedure for the specific DB-XX
>
> **What incomplete looks like:** "Rollback the app" without naming the command. "Notify the team" without a channel.
>
> Remove this block before delivering the filled doc.

> Execute in order. Do not skip steps. Time each step — if any step takes > 5 min, call for help.

1. **Notify** — Post in [Slack channel]: `"Rollback initiated for v[X.Y.Z]. Reason: [one line]. ETA: 10 min."`
2. **Identify last good release** — Run `git tag --sort=-creatordate | head -5` — confirm which version to roll back to. Write it down.
3. **Check migration status** — Open `[AppName]_Database_Migration_Checklist.md`. For every DB-XX in this release: confirm reversibility column. If ANY are non-reversible → STOP. Switch to Fix-Forward. Do not proceed.
4. **Rollback DB migrations (if reversible)** — For each DB-XX in reverse Registry order, run that migration's rollback command per Migration Checklist's per-migration Detail Block. Verify each rolls back cleanly before proceeding to the next.
5. **Rollback app** — Trigger rollback via [platform dashboard URL / exact CLI command]. Expected output: `[describe success message]`.
6. **Verify rollback** — Run `curl [URL]/health` → expect `200 OK`. Run smoke test suite via [command]. Confirm app version is the previous release via [version endpoint or platform UI].
7. **Notify resolution** — Post in [Slack channel]: `"Rollback to v[previous] complete. App stable. Incident ticket: [link]."`
8. **Create incident ticket** — Open [incident tracker]. Document: what failed, when (timestamps), who was on-call, steps taken, time to resolution, root cause if known.

---

### Fix-Forward Steps

> 🤖 **AGENT INSTRUCTIONS — Fix-Forward Steps**
>
> **Your job:** Document the fix-forward path used when rollback is not viable (irreversible migration) or the rollback window has passed.
>
> **What a complete answer covers:** Hotfix branch convention, staging verification requirement (do NOT bypass CI), normal pipeline path, production verification window, notification.
>
> **Critical anti-pattern to avoid:** "Emergency deploy" that bypasses CI. Emergency means scoped change, not skipped tests. Make this explicit.
>
> Remove this block before delivering the filled doc.

> Used when rollback is not an option (irreversible migration) or the rollback window has passed.

1. **Notify** — Post in [Slack channel]: `"Fixing forward from v[X.Y.Z]. Rollback not viable because [reason: irreversible migration DB-XX / 30-min window passed / etc.]."`
2. **Scope the fix** — Identify the minimum change needed. This is not the time for refactoring.
3. **Develop on a hotfix branch** — `git checkout -b hotfix/[issue-slug]` off the current `main`.
4. **Test on staging** — Push branch, let pipeline deploy to staging (or manually deploy if pipeline is broken), verify fix end-to-end, run smoke tests.
5. **Deploy via normal pipeline** — Tag a new release (`v[X.Y.Z+1]`). Do NOT bypass CI. Emergency does not mean untested.
6. **Verify in production** — Smoke tests + monitor error rate for [15 min] after deploy.
7. **Notify resolution** — Post in [Slack channel]: `"Hotfix v[X.Y.Z+1] deployed. Issue resolved. Incident ticket: [link]."`
8. **Create incident ticket** — Same fields as rollback ticket: what failed, timeline, fix, lessons.

---

### After a Rollback or Hotfix

> 🤖 **AGENT INSTRUCTIONS — After**
>
> **Your job:** Capture the closeout checklist that runs after any rollback or hotfix event. This is the "we don't repeat this" loop.
>
> **What a complete answer covers:** Root cause identified, fix tested before re-deploy, post-mortem written for user-impact incidents, this doc updated if a gap was exposed, monitoring reviewed.
>
> **Cross-reference:** Any rollback event becomes a `BD-XX` entry in Build Decisions Log per the BD-first-then-check rule from Progress Checklist.
>
> Remove this block before delivering the filled doc.

- [ ] Root cause identified — not "it broke" but specifically why
- [ ] Fix implemented and tested on staging before re-deploy
- [ ] Post-mortem written for any production incident that caused user impact (template: [link])
- [ ] Migration Checklist updated if a migration was rolled back (entry status updated to ↩️ Rolled Back)
- [ ] Build Decisions Log updated with a BD-XX entry covering the incident
- [ ] Monitoring thresholds reviewed — did the alert fire in time? Was it the right alert? Update Alert Rules section if not.
- [ ] This Deployment Config doc updated if the incident exposed a gap (missing runbook step, wrong authority, unclear time window, etc.)
- [ ] If this rollback was caused by a pattern that will recur, surface it as a Template Update candidate at Project Closeout

---

## Monitoring & Alerting

> 🤖 **AGENT INSTRUCTIONS — Monitoring & Alerting**
>
> **Your job:** Document what's watched, where it's logged, what fires an alert, and who gets paged. This is the operational sensor layer that drives every incident response.
>
> **What a complete answer covers:**
> - Log destination, format, error tracker, uptime monitor — named tools
> - Health check endpoint with explicit response shape and checks performed
> - Log level table per environment (DEBUG must be ❌ in production)
> - Alert rules table with measurable conditions, severity, channel, and on-call mapping
>
> **What incomplete looks like:**
> - "Monitor error rate" without naming the tool or threshold
> - Alert severity column has only one value — must include at least Critical + Warning
> - Channel column says "Slack" — name the specific channel (`#alerts`, `#incidents`, etc.)
> - "Who gets paged" left blank — name the role per Rollback Authority
>
> **Ask triggers:**
> - Tech Spec → Monitoring & Logging is empty → ask Ryan before transcribing. This is a Tech Spec gap.
> - Alert thresholds proposed without a baseline → ask: what's the current/expected baseline? Thresholds need calibration.
>
> **Cross-reference checklist:**
> - Log destination ↔ Tech Spec → Monitoring & Logging (same tool)
> - Error tracker ↔ Tech Spec → Monitoring & Logging
> - Alert channels ↔ Rollback Authority → "Notify on rollback" (same channels)
> - On-call mapping ↔ Rollback Authority → "Call the rollback" (same role)
>
> Remove this block before delivering the filled doc.

**Log destination:** [Console / CloudWatch / Datadog / Logtail / etc.]

**Log format:** [Structured JSON preferred — easier to query]

**Error tracking:** [Sentry / Bugsnag / Rollbar / etc.]

**Uptime monitoring:** [Better Uptime / UptimeRobot / Pingdom / etc.]

### Health Check

- **Endpoint:** `GET /health`
- **Checks:** [DB connection / external service reachability / memory / etc.]
- **Expected response:** `200 OK` — `{ "status": "ok", "version": "[X.Y.Z]" }`
- **Used by:** [Load balancer / uptime monitor / CI smoke tests]

### Log Levels by Environment

| Level | Development | Staging | Production |
|-------|------------|---------|------------|
| DEBUG | ✅ | ❌ | ❌ |
| INFO | ✅ | ✅ | ✅ |
| WARN | ✅ | ✅ | ✅ |
| ERROR | ✅ | ✅ | ✅ |

> DEBUG logs in production are a security risk — never enable.

### Alert Rules

| Condition | Severity | Alert channel | Who gets paged |
|-----------|----------|--------------|----------------|
| Error rate > [X]% over [5 min] | Critical | [PagerDuty / Slack `#alerts`] | [On-call per Rollback Authority] |
| p95 latency > [Xms] over [5 min] | Warning | [Slack `#alerts`] | [Team] |
| Uptime check fails | Critical | [PagerDuty / SMS] | [On-call] |
| DB connection failures | Critical | [PagerDuty] | [On-call] |
| Disk usage > [80]% | Warning | [Slack `#alerts`] | [Team] |
| [Custom condition tied to a business metric] | [—] | [—] | [—] |

---

## Feature Flags

> 🤖 **AGENT INSTRUCTIONS — Feature Flags**
>
> **Your job:** Document the toggleable-features layer if one exists. If it doesn't, say so explicitly.
>
> **Skip-if-empty rule:** If Ryan has not adopted feature flags for this app, do NOT invent a registry or example flag entries. State explicitly: `"No feature flags — all features are enabled in all environments. If introduced later, this section will be filled."` Then leave the Flag Registry and Rollout Strategy tables removed or marked N/A. Absence is meaningful.
>
> **What a complete answer covers (if flags ARE in use):**
> - Feature flag tool named (LaunchDarkly / PostHog / Unleash / static env var / custom)
> - Flag storage explicit (remote runtime / static deploy-time / both)
> - Flag Registry has one row per active flag; every row has a remove-after date or condition
> - Rollout Strategy table covers any flag with a non-binary rollout (percentage rollout, A/B test)
>
> **What incomplete looks like:**
> - Empty Flag Registry left with placeholder rows — either fill or remove
> - Flag with no "Remove after" entry — flags accumulate; every flag needs a cleanup trigger
>
> **Ask triggers:**
> - Tech Spec doesn't mention feature flags but Ryan introduces them here → confirm. This should have been a Tech Spec decision.
>
> Remove this block before delivering the filled doc.

> **What this is:** A record of toggleable features — things that can be enabled/disabled without a deploy. Useful for gradual rollouts, A/B testing, and "ship dark" (deploy code before enabling it).
>
> **If not using feature flags:** State so explicitly. `"No feature flags — all features are enabled in all environments."` Then remove or N/A the tables below.

**Feature flag tool:** [LaunchDarkly / PostHog / Unleash / Custom env var / None — explain]

**Flag storage:** [Remote (evaluated at runtime) / Static (env var at deploy time) / Both]

---

### Flag Registry

> One row per active flag. Every flag has a remove-after trigger — don't let this list grow indefinitely.

| Flag name | Type | Default (dev) | Default (staging) | Default (prod) | Purpose | Remove after |
|-----------|------|--------------|------------------|---------------|---------|-------------|
| `[feature_name_enabled]` | [Boolean / String / Number] | [on / off] | [on / off] | [off] | [What it gates] | [Trigger — e.g., "After v2.0 ships and metrics stable for 2 weeks"] |
| `[flag_name]` | [—] | [—] | [—] | [—] | [—] | [—] |

> **Naming convention:** `[feature_name]_enabled` or `[feature_name]_variant` — snake_case, descriptive.

---

### Rollout Strategy

> For flags being rolled out gradually.

| Feature | Rollout plan | Success criteria to advance | Rollback trigger |
|---------|--------------|-----------------------------|-----------------|
| [Feature] | [e.g., 10% → 50% → 100% over 2 weeks] | [e.g., Error rate < 0.5%, p95 latency stable] | [e.g., Error rate spike > 1%] |

---

## Open Questions

> 🤖 **AGENT INSTRUCTIONS — Open Questions**
>
> **Your job:** Track every deferred decision. Anything `❓ AGENT PAUSE` raised that Ryan punted on lands here.
>
> **What a complete answer covers:** Every question has Blocking?, Owner, Resolved? columns filled. Blocking questions are resolved before Gate 2.
>
> **Rule:** Gate 2 cannot sign off while any Blocking question is unresolved.
>
> Remove this block before delivering the filled doc.

| # | Question | Blocking? | Owner | Resolved? |
|---|----------|-----------|-------|-----------|
| 1 | [e.g., Which hosting platform are we using?] | Yes | Ryan | ❌ |
| 2 | [e.g., Do we need staging for MVP or go straight to prod?] | Yes | Ryan | ❌ |
| 3 | [Question] | [Yes / No] | [—] | ❌ |

---

## 🚦 GATE 2 — Full Sign-Off

> 🤖 **AGENT INSTRUCTIONS — Gate 2**
>
> **Why this gate matters:** This is the final consistency check before Mid-Build Review picks up operational health monitoring against this doc. Misses here cause "works in CI, fails in prod" outages and rollback ambiguity during incidents.
>
> **What you (the agent) verify before requesting sign-off:**
> - Every section is at ✅ Done
> - Every bidirectional check below has been walked with the source doc open — no checks from memory
> - Every Open Question marked Blocking is Resolved
> - Both failure modes (silent environment drift / deploy-rollback asymmetry) have been actively checked, not just assumed prevented
>
> **What human sign-off means:** Ryan has reviewed the doc end to end and certifies that:
> - The pipeline spec can generate working CI/CD YAML
> - The rollback runbook is executable by an on-call engineer at 2am without further context
> - The env vars and secrets are exhaustive
> - Monitoring will catch the failures the alert table describes
>
> Remove this instruction block before delivering. Keep the checklist and sign-off line.

**Section completeness:**
- [ ] Every section in Status & Next Steps shows ✅ Done
- [ ] No `[bracketed placeholders]` remain anywhere in the doc
- [ ] No `🤖` blocks remain (search for the literal string)
- [ ] No `❓ AGENT PAUSE` markers remain
- [ ] All cleanup verification items from the 🛠️ banner are complete

**Bidirectional ID link checks (open each source doc and walk both directions):**

*Pipeline ↔ Module Breakdown:*
- [ ] Every Module ID (M-XX) in Module Breakdown's Phase Plan is referenced in a pipeline phase mapping here
- [ ] Every Module ID referenced in pipeline phase mapping exists in Module Breakdown's Module Registry

*Pipeline ↔ Migration Checklist:*
- [ ] Every DB-XX in Migration Checklist's Registry appears in the pipeline migration steps (staging + prod)
- [ ] Every DB-XX referenced in pipeline migration steps exists in Migration Checklist's Registry
- [ ] Migration order in the pipeline matches Migration Checklist Registry order exactly (top-to-bottom)
- [ ] Every DB-XX's reversibility declaration in Migration Checklist is consistent with how step 12 (production migration step) handles failure for that migration

*Pipeline ↔ Testing Strategy:*
- [ ] Every pipeline stage declared in Testing Strategy → CI/CD Integration appears here as a pipeline step
- [ ] Every test stage in this pipeline (UT / IT / AC / E2E / smoke) corresponds to a Testing Strategy declaration
- [ ] Coverage thresholds in PR Check step 5 ↔ Testing Strategy → Coverage Plans (same numbers)

*Env Vars ↔ Tech Spec:*
- [ ] Every variable in Tech Spec → Environment Variables has a row in this doc's Env Vars table
- [ ] Every variable in this doc's Env Vars table appears in Tech Spec → Environment Variables
- [ ] Every variable marked `[Managed secret]` in Prod column has a corresponding row in Secrets Management → Rotation table
- [ ] Every row in Secrets Management → Rotation table corresponds to a `[Managed secret]` row in Env Vars

*Monitoring ↔ Tech Spec:*
- [ ] Log destination, error tracker, uptime monitor here ↔ Tech Spec → Monitoring & Logging (same tools)
- [ ] Alert channels here ↔ Rollback Authority's "Notify on rollback" channel (same channel)
- [ ] On-call mapping here ↔ Rollback Authority's "Call the rollback" role (same role)

**Reversibility coverage check:**
- [ ] Every step in CI/CD Pipeline → Production Deploy has a Reversibility column value filled (no blanks)
- [ ] Step 12 (production migrations) defers to Migration Checklist's per-migration reversibility — verified consistent
- [ ] Step 13 (production deploy) reversibility ↔ rollback authority's "Call rollback" rule
- [ ] Step 16 (post-deploy verification window) time-window value ↔ Rollback Authority → time window rule (same number)

**Failure-mode active checks:**

*Silent environment drift:*
- [ ] Dev / Staging / Prod env var columns explicitly differ where they should (NODE_ENV, JWT_EXPIRES_IN, etc.) and explicitly match where they should
- [ ] Staging → "Documented differences from prod" section is non-empty (every deliberate difference logged)
- [ ] Per-environment configs (Dev / Staging / Prod) reviewed side-by-side — no unintended divergences

*Deploy/rollback asymmetry:*
- [ ] Every production deploy step has either an automated rollback path OR a documented Fix-Forward path
- [ ] Manual Rollback Steps cover DB migration rollback explicitly (not just app rollback)
- [ ] Fix-Forward Steps explicitly named as the path when migrations are irreversible
- [ ] Authority table covers DB migration rollback approval (not just app rollback)

**Open Questions cleared:**
- [ ] Every Open Question marked `Blocking? Yes` is `Resolved? ✅`

**Human sign-off:**
- [ ] Ryan: ☐ Approved — Deployment Config complete. Mid-Build Review may now use this doc as the operational-health reference.
