# Mid-Build Review [N]: [App Name]

---

## 🛠️ Read Before Filling

> **This template is AI-optimized AND runtime-maintained.** It contains agent-facing instruction blocks (🤖) that guide the writing agent. **Unlike most templates, these blocks STAY IN THE DOC DURING THE BUILD** because they govern runtime drift-logging behavior. Cleanup happens only at Project Closeout, not at this instance's sign-off.

**Template version:** v1.0 (AI-optimized, runtime maintenance doc pattern)
**Operational mode:** Runtime maintenance doc — agent logs drift continuously during build; gate validates the log at instance close.

**Filename convention:** `[AppName]_Mid_Build_Review_[N].md` — `[N]` is the sequential instance number. First instance is `_1`, second is `_2`, etc. Multiple instances coexist on disk — each is a snapshot of drift observed during a contiguous build window.

---

### When to Open a New Instance

A new instance opens when ANY of the following occur after the prior instance's Closeout Gate has been signed off:

- **2–3 modules from Module Breakdown have moved to ✅ Done** since the last instance closed
- **A phase boundary is approaching** (Mid-Build instance closes; Phase Closeout opens next)
- **Drift is suspected** — anyone (Ryan, the build agent, a coding agent operating Claude Code) flags that something has felt off since the prior instance
- **A material design-doc change occurred mid-build** (a Tech Spec edit, a Schema change, a UI/UX revision) — drift check the code against the change

Do NOT open a new instance for:
- Single-module completions (too granular — wait for 2–3)
- Routine progress (Progress Checklist handles that)
- Cosmetic refactors with no design-doc implications

> **Rule:** Only one Mid-Build Review instance is **open** at a time. If a new instance opens while a prior one is unsigned, STOP — close the prior one first or merge their findings.

---

### How This Instance References Prior Instances

- **First section of every instance after `_1`:** A "Prior Instance Reference" sub-section that names the previous instance file and lists any Findings deferred forward (`Resolved? = Deferred to instance [N]`).
- Deferred Findings are transcribed into the matching section of the new instance with a note: `[Carried from Mid_Build_Review_[N-1].md]`.
- A Finding is never silently dropped. Either it's resolved, accepted as a Build Decision (logged in `[AppName]_Build_Decisions_Log.md` with a BD-XX ID), or explicitly deferred forward.

---

### Operational Mode: Runtime Maintenance Doc

**Agent role: continuous drift logger.** During the build window covered by this instance:

- As coding work proceeds, the agent observes code against design docs and **logs any drift into the appropriate section's Findings table within a few moments of noticing it**.
- The agent does NOT wait for a "review checkpoint" to populate Findings. By the time the human asks for a review, the Findings tables should already be populated from the live observation.
- The Closeout Gate at the end of this instance verifies the log is complete and resolves/defers each item — it does not re-discover drift from scratch.

**Human reads this doc to see drift accumulating; human does not curate the Findings tables. The agent owns the live state.**

**The three rules (apply throughout the instance):**
1. **Log on observation, not on schedule.** If drift is noticed during a coding session, it goes into the Findings table that day — not "next time the human asks for a review."
2. **Doc-first resolution.** A Finding is resolved by updating the source design doc first, then aligning the code. Never the reverse. If the code is correct and the doc is wrong, the doc gets edited and the Finding closes with `Resolved? = Doc updated to match code`.
3. **No silent fixes.** If the agent encounters drift and fixes it in code without logging a Finding here, that is **silent resolution** — a named failure mode. Log first, then fix.

---

### Commit-Time Logging Rule (the strongest mitigation for drift accumulation)

> This rule is what stops silent drift from accumulating. It makes the absence of a Finding visible at the commit level, not just at the gate.

**Every commit that touches design-doc-relevant code MUST do one of the following in its commit body:**

1. **Reference a Mid-Build Review Finding being logged or resolved by this commit:**
   `Mid-Build Review _[N] §[section]: Finding #[X] [logged|resolved]`
   Example: `Mid-Build Review _3 §2: Finding #4 logged — POST /api/projects returns 200 not 201`
2. **Assert no drift:**
   `Mid-Build Review _[N]: no drift`
   Use only when the agent has actively cross-checked the affected code against the relevant design doc(s) in the same coding session and confirmed alignment.
3. **Mark the commit as out-of-scope for drift checking:**
   `Mid-Build Review _[N]: out-of-scope (reason: [why — e.g., "build tooling only, no design-doc relevance"])`

**"Design-doc-relevant code" means any commit that touches:**
- Database migrations, models, or schema definitions → Section 1 territory
- API route handlers, request/response shapes, validation, error handling → Section 2 territory
- State machines, service methods, auth/permission logic, event handlers → Section 3 territory
- React components, screens, navigation, UI strings → Section 4 territory
- Anything else that maps to a design doc

**Out-of-scope examples (legitimately not drift-relevant):**
- Build tooling, CI config, lint rule updates
- Test-only changes that don't change behavior
- Dependency bumps without code changes
- Formatting / whitespace / comment-only edits
- Documentation-only commits to docs that are not design docs (e.g., README)

**This rule applies to commits made by anyone — Ryan, a build agent, a coding agent operating Claude Code. The commit body line is the audit trail.**

**Why this rule works:**
- It makes silent drift accumulation **observable in git log** — searching commits for `Mid-Build Review _[N]` and finding gaps reveals coding sessions that bypassed the rule.
- It moves the cost of logging from "remember to update the doc" to "include one line in the commit message." Much lower friction → much higher compliance.
- The Closeout Gate has a specific check that walks recent commits and verifies each one has the line.

**Failure to follow this rule:**
- If a commit touches design-doc-relevant code without the line, that's logged at the Closeout Gate as an audit gap. Ryan decides whether to retroactively log Findings (probably) or accept the gap with a BD entry (rarely).
- Persistent non-compliance from an agent or human → re-instruct, or treat as a Section 6 Risk for the instance.

---

### Source Docs (read continuously — these are the drift-comparison targets)

This doc has no single upstream source. It checks the code against **every** design and coding doc currently in force. The relevant docs by section:

- **Section 1 (Data Model Drift):** `[AppName]_DB_Schema.md`
- **Section 2 (API Drift):** `[AppName]_Technical_Spec.md` (API Endpoints, State Machines), `[AppName]_API_Contract.md`
- **Section 3 (Business Logic Drift):** `[AppName]_Technical_Spec.md` (State Machines, Events & Side Effects, Auth), `[AppName]_Component_Service_Layer_Map.md` (service contracts)
- **Section 4 (UI Drift):** `[AppName]_UI_UX.md`, `[AppName]_UI_Strings.md`, `[AppName]_Component_Service_Layer_Map.md` (component registry)
- **Section 5 (Open Questions Answered During Coding):** `[AppName]_Decisions_Log.md`, `[AppName]_Build_Decisions_Log.md`, all design docs
- **Section 6 (New Risks):** All design docs as a check for "was this risk anticipated?"
- **Section 7 (Modules Remaining):** `[AppName]_Module_Breakdown.md`, `[AppName]_Progress_Checklist.md`
- **Operational health (also Section 6):** `[AppName]_Deployment_Config.md` — alert rules and monitoring thresholds are the "what should be observed" reference

### Downstream Consumers

- **`[AppName]_Build_Decisions_Log.md`** — every Finding marked `Resolved? = Accepted as BD` produces a BD-XX entry there
- **`[AppName]_Progress_Checklist.md`** — Findings that change module verification rules (e.g., "we changed the auth approach for M-04") may require Progress Checklist updates
- **`[AppName]_Phase_Closeout_[N].md`** — Findings unresolved at phase boundary roll up to Phase Closeout
- **The next Mid-Build Review instance** — deferred Findings carry forward

---

### Agent Role Statement

You are the scribe who logs drift between code and design docs as it accumulates during the build. You are NOT a code reviewer in the general sense — you are specifically the drift watcher. You don't comment on code quality, style, or architecture choices that ARE consistent with the design docs. You log only where code and docs disagree.

**Your judgment is:** Did the code's behavior, shape, or contract diverge from what a design doc says? If yes → Finding. If no → silent.

You do NOT decide how to resolve a Finding. Resolution belongs to Ryan. You log the Finding with a `Resolved? = ❌` and surface a proposed resolution in the rightmost column as a suggestion only.

---

### Two Failure Modes This Doc Is Designed to Prevent

- **Drift accumulation** — Agent codes through obvious drift without logging it. Equivalent of Progress Checklist's silent resolution. Caught by: log-on-observation rule + Closeout Gate's "evidence the agent was actively logging during the build window" check.
- **Cosmetic-flag-as-resolved** — Agent logs a Finding and marks it `✅ Resolved` without the underlying fix actually happening (source doc not actually edited, BD entry not actually logged, code not actually changed). Caught by: per-section verification rules table + Closeout Gate's evidence re-check.

---

### Section Override Conventions (which sections activate for this instance)

Not every Mid-Build instance must populate every section. Sections activate based on what was built since the last instance:

- **Pure data modules built (M-XX with Type = Data) since last instance** → Sections 1 (Data), 3 (Logic if rules added), 5, 6, 7 active. Sections 2 (API), 4 (UI) → N/A or carry-forward only.
- **Pure UI modules built since last instance** → Sections 4 (UI), 5, 6, 7 active. Sections 1, 2, 3 → N/A or carry-forward only.
- **Full-stack modules built since last instance** → All sections active.
- **No modules completed but design doc changes occurred** → Whichever sections those changes affect, plus 5 (Decisions Log update) and 6 (Risks).

This instance's active sections must be declared in the Instance Header below, using the conventions.

---

### When This Instance Is Closed (Closeout Gate Signed)

- The agent stops actively logging Findings to this file
- A new Mid-Build Review instance opens (`_[N+1].md`) the next time drift is observed or a new 2–3 module batch completes
- The 🤖 blocks and `❓ AGENT PAUSE` markers in this file **stay in** — they are not removed at instance close. Cleanup of all Mid-Build instances happens only at Project Closeout.

---

### Cleanup at Project Closeout (NOT at instance sign-off)

When the entire build is complete and Project Closeout runs:
- [ ] All Mid-Build Review instances reviewed; any Findings still deferred have rolled into Build Decisions Log as BD entries or been resolved
- [ ] `🤖` blocks removed from all instances — search for `🤖` across `Mid_Build_Review_*.md` and verify zero hits
- [ ] `❓ AGENT PAUSE` markers removed
- [ ] Agent-instruction prose in Closeout Gate blocks removed; checklists + sign-offs retained
- [ ] All `[bracketed placeholders]` filled or marked N/A with rationale
- [ ] `[AppName]` replaced with real app name in all instances

> Remove this entire 🛠️ Read Before Filling section ONLY at Project Closeout, not at instance sign-off.

---

## Instance Header

> 🤖 **AGENT INSTRUCTIONS — Instance Header**
>
> Fill this at the moment the instance opens. Do not fill the rest of the doc until this header is complete.
>
> **Active sections rule:** Declare which sections will be populated for this instance using the override conventions above. Sections declared inactive can be left with their Findings table empty + a single `N/A — no [data/UI/etc.] work since instance [N-1]` row.
>
> Keep this block in during the build. Remove at Project Closeout only.

| Field | Value |
|-------|-------|
| Instance number | [N] |
| Opened on | [YYYY-MM-DD] |
| Triggered by | [2-3 module completion / phase approaching / drift suspected / design-doc change / other] |
| Trigger detail | [Which modules, which phase, what was suspected, which design doc changed] |
| Modules covered since prior instance | [M-XX, M-XX, M-XX — list] |
| Prior instance | [Mid_Build_Review_[N-1].md — or "N/A — first instance"] |
| **Active sections this instance** | [List of sections active. E.g., "1, 3, 5, 6, 7" with note "pure data modules since last instance"] |
| Closed on | [YYYY-MM-DD — fill at Closeout Gate sign-off] |

---

## Prior Instance Reference

> 🤖 **AGENT INSTRUCTIONS — Prior Instance Reference**
>
> If this is instance `_1`, mark this section "N/A — first instance" and skip.
>
> Otherwise: open the prior instance file. For every Finding row where `Resolved? = Deferred to instance [N]`, transcribe it into the matching section below with the `[Carried from Mid_Build_Review_[N-1].md]` note.
>
> **Strict rule:** A deferred Finding is never silently dropped. If it's no longer relevant (e.g., the code was refactored away), update its Resolved column to `Obsolete — [reason]` rather than omitting it.
>
> Keep this block in during the build. Remove at Project Closeout only.

| # | Carried from | Section | Original Finding | Current resolution status |
|---|--------------|---------|------------------|---------------------------|
| 1 | [Mid_Build_Review_[N-1].md row N] | [Section number] | [One-line summary] | [Status — Resolved / Still deferred / Obsolete] |
| 2 | [—] | [—] | [—] | [—] |

---

## Status & Next Steps

| Section | Status | Owner | Active? | Notes |
|---------|--------|-------|---------|-------|
| 1. Data Model Drift | 🔲 Not Started | — | [Yes / N/A] | Check code vs. DB Schema |
| 2. API Drift | 🔲 Not Started | — | [Yes / N/A] | Check code vs. Tech Spec + API Contract |
| 3. Business Logic Drift | 🔲 Not Started | — | [Yes / N/A] | State machines, side effects, auth, service contracts |
| 4. UI Drift | 🔲 Not Started | — | [Yes / N/A] | Check code vs. UI/UX + UI Strings + Component Map |
| 5. Open Questions Answered During Coding | 🔲 Not Started | — | [Yes / N/A] | Informal decisions to formalize |
| 6. New Risks or Red Flags | 🔲 Not Started | — | [Yes / N/A] | Problems not in design docs; includes operational drift |
| 7. Modules Remaining | 🔲 Not Started | — | [Yes / N/A] | Scope gut-check |
| **🚦 Closeout Gate** | 🔲 Not Started | Ryan | Always | Sign off before opening next instance |

**Status values:** 🔲 Not Started / 🔄 In Progress / 👀 In Review / ✅ Done / 🚫 Blocked

---

## 1. Data Model Drift

> 🤖 **AGENT INSTRUCTIONS — Section 1: Data Model Drift**
>
> **Your job:** Continuously compare actual database state (schema, indexes, constraints, applied migrations) against `[AppName]_DB_Schema.md`. Log every divergence.
>
> **When to log a Finding (during the build window covered by this instance):**
> - You observe a table/column/index/FK in code or applied migrations that doesn't match Schema doc
> - A migration was applied that changes shape without an accompanying Schema doc update
> - A field constraint in code (validation, nullability check) doesn't match the doc's constraint
> - An entity exists in Schema but no migration creates it (orphan in design)
> - An entity exists in DB but is not in Schema (orphan in code)
>
> **What a complete Finding row covers:**
> - Specific entity/field name (not "the users table" — `users.email_normalized`)
> - Exact doc statement (paste the relevant Schema line)
> - Exact code/DB state (paste the migration SQL or model definition)
> - Proposed resolution from one of: `Update doc / Update code / Accept as BD / Discuss with Ryan`
>
> **What incomplete looks like:**
> - "Data model is off" with no specifics
> - Finding logged but Proposed resolution column empty
> - Finding marked `✅ Resolved` without the Resolved row pointing to a specific Schema commit, BD-XX, or code change
>
> **Ask triggers (`❓ AGENT PAUSE`):**
> - Code state and doc state are both plausible — unclear which is correct → ask Ryan
> - Migration ran but its DB-XX entry in Migration Checklist is still 🔄 In Progress (not ✅ Applied) — clarify what actually happened
>
> **Per-section verification rules — when a row can move to ✅ Resolved:**
>
> | Resolution type | Can be ✅ Resolved ONLY when... |
> |---|---|
> | Update doc | The Schema doc is committed with the matching change; commit SHA or date noted in Resolution column |
> | Update code | The code/migration is changed and applied (DB-XX = ✅ Applied or new DB-XX added and applied) |
> | Accept as BD | A specific BD-XX entry exists in `[AppName]_Build_Decisions_Log.md` and is referenced in Resolution column |
> | Discuss with Ryan | Ryan has signed off in chat or via a Decisions Log AD-XX entry; reference the AD-XX or chat date |
>
> Keep this block in during the build. Remove at Project Closeout only.

**Checklist (live — items move from 🔲 to ✅ as the agent verifies them during the build window):**

- [ ] Every entity in the DB matches `[AppName]_DB_Schema.md` (no extra tables, no missing tables)
- [ ] Every field matches the schema definition (type, nullability, default, constraints)
- [ ] All foreign keys and cascade behaviors match the Relationships section
- [ ] All indexes in the DB match the Schema's indexing strategy
- [ ] Any schema changes made during coding are reflected back in `[AppName]_DB_Schema.md` (with a Decisions Log entry if architectural)

**Findings:**

| # | Entity / Field | What the doc says | What the code does | Proposed resolution | Resolved? |
|---|---------------|-------------------|--------------------|--------------------|-----------|
| 1 | [—] | [—] | [—] | [Update doc / Update code / Accept as BD / Discuss] | ❌ |

---

## 2. API Drift

> 🤖 **AGENT INSTRUCTIONS — Section 2: API Drift**
>
> **Your job:** Continuously compare implemented API surface against `[AppName]_Technical_Spec.md` (API Endpoints section) and `[AppName]_API_Contract.md`.
>
> **When to log a Finding:**
> - Endpoint exists in code but not in API Contract (or vice versa)
> - Method/path/auth-requirement differs between code and Contract
> - Request shape (body, query, path params) differs
> - Response shape (success body, headers, status codes) differs
> - Error response format diverges from Tech Spec's standard error format
> - Validation rules in code differ from API Contract's documented rules
> - Endpoint exists in UI/UX → Screen → API Map but no implementation OR no Contract entry
>
> **What a complete Finding row covers:**
> - Specific endpoint (HTTP method + path: `POST /api/projects/:id/members`)
> - Exact Contract statement vs. exact code statement
> - Proposed resolution
>
> **Ask triggers:**
> - Endpoint was added in code with reasonable shape but no Contract entry exists → ask: was this planned and undocumented, or accidental scope creep? The answer changes the resolution path.
> - Code and Contract disagree on error format → ask: which is the source of truth for the error format here?
>
> **Per-section verification rules:**
>
> | Resolution type | Can be ✅ Resolved ONLY when... |
> |---|---|
> | Update doc | `[AppName]_API_Contract.md` (and/or Tech Spec) committed with matching change |
> | Update code | Code changed AND the AC-XX contract test passes in CI for that endpoint |
> | Accept as BD | BD-XX exists in Build Decisions Log |
> | Discuss with Ryan | Signed off in chat or AD-XX entry |
>
> Keep this block in during the build. Remove at Project Closeout only.

**Checklist:**

- [ ] Every implemented endpoint matches API Contract (method, path, auth, request shape, response shape)
- [ ] Every endpoint in API Contract is implemented OR explicitly deferred with a BD entry
- [ ] Validation rules in code match Contract's documented validations
- [ ] Error responses follow the standard format defined in Tech Spec
- [ ] Any new endpoints added during coding are documented in Tech Spec + API Contract + UI/UX Screen → API Map
- [ ] Any endpoints removed or renamed are reflected in all three places above

**Findings:**

| # | Endpoint | What the doc says | What the code does | Proposed resolution | Resolved? |
|---|---------|-------------------|--------------------|--------------------|-----------|
| 1 | [—] | [—] | [—] | [—] | ❌ |

---

## 3. Business Logic Drift

> 🤖 **AGENT INSTRUCTIONS — Section 3: Business Logic Drift**
>
> **Your job:** Continuously compare implemented business logic against the design docs for state machines, side effects, auth/permissions, and service contracts.
>
> **Specific drift targets:**
> - State machine transitions in code vs. Tech Spec → State Machines (legal transitions, guards, side effects per transition)
> - Side effects fired in code vs. Tech Spec → Events & Side Effects (Event Map)
> - Auth checks in code vs. Tech Spec → Authentication & Authorization
> - Service method signatures and transactional boundaries vs. Component/Service Map → Service Detail Blocks
>
> **When to log a Finding:**
> - Code allows a state transition not in the Tech Spec
> - Tech Spec defines a state transition with a side effect, but the code's transition fires no side effect (or the wrong one)
> - Permission check is missing where doc requires one (or present where doc doesn't require one)
> - Service signature in code doesn't match Component/Service Map's contract for that S-XX
> - Transactional boundary in code differs from Service Detail Block
>
> **Critical state machine check:**
> - For any entity with a state machine in Tech Spec: walk every legal transition. Confirm the code's transition function permits it AND fires every side effect named in the Event Map row for that transition. Missing side effects are the most common business logic drift.
>
> **Ask triggers:**
> - Code implements a guard not in Tech Spec — was this a deliberate addition (BD) or scope creep?
> - Side effect exists in code but no Event Map row exists for it — is this an undocumented side effect or an Event Map gap?
>
> **Per-section verification rules:**
>
> | Resolution type | Can be ✅ Resolved ONLY when... |
> |---|---|
> | Update doc | Tech Spec / Component-Service Map committed with matching change |
> | Update code | Code changed; relevant UT-XX / IT-XX tests passing in CI |
> | Accept as BD | BD-XX exists |
> | Discuss with Ryan | Signed off |
>
> Keep this block in during the build. Remove at Project Closeout only.

**Checklist:**

- [ ] State machine transitions in code match Tech Spec (legal transitions, guards, side effects per transition)
- [ ] Every side effect in the Event Map is implemented at the right transition
- [ ] No undocumented side effects exist in code (every side effect traces to an Event Map row)
- [ ] Auth / permission checks in code match the roles and permissions table
- [ ] Service signatures, DI, and transactional boundaries in code match Component/Service Map → Service Detail Blocks
- [ ] Any business logic decisions made during coding are documented (BD-XX or Tech Spec edit)

**Findings:**

| # | Rule / Transition / Service | What the doc says | What the code does | Proposed resolution | Resolved? |
|---|----------------------------|-------------------|--------------------|--------------------|-----------|
| 1 | [—] | [—] | [—] | [—] | ❌ |

---

## 4. UI Drift

> 🤖 **AGENT INSTRUCTIONS — Section 4: UI Drift**
>
> **Your job:** Continuously compare implemented frontend against `[AppName]_UI_UX.md`, `[AppName]_UI_Strings.md`, and Component/Service Map → Component Registry.
>
> **When to log a Finding:**
> - Screen exists in code but not in UI/UX doc (or vice versa)
> - Screen's 4 states (loading, empty, error, populated) are not all implemented per UI/UX spec
> - Component exists in code but not in Component Registry (orphan)
> - Component exists in Component Registry but not used anywhere in screens (orphan in doc)
> - Shared component is duplicated per screen rather than reused once
> - String in UI doesn't match UI Strings doc (English literal, copy variant, error message text)
> - Navigation differs from UI/UX → Navigation / Information Architecture
> - Screen → API Map references an endpoint that doesn't exist or the screen calls an endpoint not in its map
>
> **Critical reuse check:**
> - For every Shared Component in UI/UX Inventory: confirm code has ONE C-XX implementation and N screens import from it, not N duplicate copies. This is the most common UI drift.
>
> **Ask triggers:**
> - A new screen was built with a reasonable UI but no UI/UX entry — undocumented addition or planned-but-undocumented?
> - String literal in code differs from UI Strings — was this a copy fix that should propagate back to UI Strings, or accidental?
>
> **Per-section verification rules:**
>
> | Resolution type | Can be ✅ Resolved ONLY when... |
> |---|---|
> | Update doc | UI/UX / UI Strings / Component Map committed with matching change |
> | Update code | Code changed; relevant E2E-XX or snapshot test passing in CI |
> | Accept as BD | BD-XX exists |
> | Discuss with Ryan | Signed off |
>
> Keep this block in during the build. Remove at Project Closeout only.

**Checklist:**

- [ ] Implemented screens match UI/UX doc (all 4 states present: loading, empty, error, populated)
- [ ] Navigation matches the screen tree / Information Architecture
- [ ] Shared components are built once and reused (no per-screen duplication)
- [ ] Every C-XX in Component Registry is implemented; every implemented component has a C-XX entry
- [ ] Error states match the error inventory
- [ ] Strings in UI match UI Strings doc (or UI Strings updated to match deliberate code changes)
- [ ] Screen → API Map is consistent with what each screen actually calls
- [ ] Any UI changes made during coding are reflected in the relevant doc

**Findings:**

| # | Screen / Component / String | What the doc says | What the code does | Proposed resolution | Resolved? |
|---|----------------------------|-------------------|--------------------|--------------------|-----------|
| 1 | [—] | [—] | [—] | [—] | ❌ |

---

## 5. Open Questions Answered During Coding

> 🤖 **AGENT INSTRUCTIONS — Section 5: Open Questions Answered During Coding**
>
> **Your job:** Capture decisions made informally during the build that should be formally documented. Distinct from Findings 1–4 (which catch divergence) — this section catches **silent decisions that no doc currently reflects in either direction**.
>
> **When to log a row:**
> - Ryan made a call in chat that changed an approach but no doc was updated yet
> - The build agent (or coding agent) had to pick between two plausible options and picked one, with the choice not yet documented
> - A technical constraint was discovered (library limitation, performance ceiling) that shaped an implementation choice
> - An informal naming convention was adopted (variable prefix, file structure pattern) that should propagate
>
> **What a complete row covers:**
> - One-line statement of the decision
> - Where it MUST be documented (one or more: Decisions Log AD-XX / Build Decisions Log BD-XX / Tech Spec edit / Schema edit / UI/UX edit / etc.)
> - Done flag
>
> **Distinction from Build Decisions Log:**
> - This section is the **detector**. Build Decisions Log is the **destination**.
> - A row here points to a BD-XX or AD-XX entry that needs to be created (or already was). When that entry exists and the relevant doc is updated, the row is ✅.
>
> **Ask triggers:**
> - The decision is architectural (would warrant AD-XX in Decisions Log) but Ryan made it casually — confirm AD entry should be opened.
> - The decision contradicts a prior AD-XX — flag the contradiction; Ryan must resolve.
>
> **Per-section verification rules:**
>
> | Resolution type | Can be ✅ Done ONLY when... |
> |---|---|
> | Documented in Decisions Log (AD-XX) | Specific AD-XX entry exists and is committed |
> | Documented in Build Decisions Log (BD-XX) | Specific BD-XX entry exists and is committed |
> | Source doc edited | Specific doc edit committed with rationale in commit message |
>
> Keep this block in during the build. Remove at Project Closeout only.

| # | Decision made | Where it should be documented | Specific entry ID | Done? |
|---|--------------|-------------------------------|-------------------|-------|
| 1 | [e.g., "Decided to use cursor pagination instead of offset for /api/projects list"] | Tech Spec + Decisions Log (architectural) | AD-[XX] | ❌ |
| 2 | [—] | [—] | [—] | ❌ |

---

## 6. New Risks or Red Flags

> 🤖 **AGENT INSTRUCTIONS — Section 6: New Risks or Red Flags**
>
> **Your job:** Capture problems discovered during the build that weren't anticipated in design. Includes operational drift against `[AppName]_Deployment_Config.md` (alert rules firing unexpectedly, monitoring thresholds revealed wrong, env config differences between dev and staging exposed at runtime).
>
> **What counts as a Risk row vs. a Finding row (1–4):**
> - Findings 1–4 are about **divergence between code and an existing doc statement**. The doc said X, code does Y, log it.
> - Section 6 is about **problems that no doc currently addresses**. There's no doc statement to compare against — the design didn't anticipate this.
>
> **When to log a Risk row:**
> - Third-party API behaves unexpectedly (rate limits tighter than expected, response shape varies, downtime patterns)
> - Performance ceiling discovered (query slow, memory growth, latency spike under load)
> - Security concern discovered (data exposure path, missing rate limit, auth edge case)
> - Operational drift against Deployment Config (alert fired but threshold was wrong; staging differs from prod in a way that wasn't documented)
> - Dependency issue (library deprecated, security advisory, version conflict)
>
> **What a complete row covers:**
> - One-line issue
> - Impact assessment (Low / Medium / High + why)
> - Proposed fix (technical, not just "investigate")
> - Blocking flag — does this block continuing the build?
>
> **Ask triggers:**
> - Risk could be High impact but Ryan hasn't been told yet — surface immediately, do not wait for Closeout Gate
> - Risk involves data exposure, security, or PII — escalate immediately
>
> **Per-section verification rules:**
>
> | Resolution type | Can be ✅ Resolved ONLY when... |
> |---|---|
> | Fix implemented | Code/config change committed AND deployed to relevant env AND verified |
> | Accept as known risk (BD) | BD-XX exists explicitly labeled as Accepted Risk with mitigation note |
> | Defer with explicit trigger | Resolution column states: "Defer until [specific trigger event]"; trigger is observable |
>
> Keep this block in during the build. Remove at Project Closeout only.

| # | Issue | Impact (L/M/H + why) | Proposed fix | Blocking? | Resolved? |
|---|-------|---------------------|-------------|-----------|-----------|
| 1 | [e.g., "Third-party API rate limit is 100/min, not 1000/min as assumed in Tech Spec"] | [M — affects bulk import in M-08] | [Queue + throttle; revise Tech Spec rate-limit section] | [No] | ❌ |
| 2 | [—] | [—] | [—] | [—] | ❌ |

---

## 7. Modules Remaining

> 🤖 **AGENT INSTRUCTIONS — Section 7: Modules Remaining**
>
> **Your job:** Scope gut-check against `[AppName]_Module_Breakdown.md` and `[AppName]_Progress_Checklist.md`. Capture estimate changes and surprises.
>
> **When to update:**
> - At every instance open — re-pull the current state of Module Breakdown's Module Registry and Progress Checklist's per-module status
> - Whenever a remaining module's scope estimate shifts (a coding session revealed it's harder than expected, or simpler)
>
> **What a complete answer covers:**
> - One row per remaining module from Module Breakdown (Done modules can be summarized in a single row or omitted)
> - Status pulled from Progress Checklist
> - "Surprises or scope changes" column populated for any module where original estimate diverges from current understanding
> - "Adjusted estimate" line if remaining scope has changed materially since the previous instance
>
> **Cross-reference checklist:**
> - Module list here ↔ Module Breakdown → Module Registry (bidirectional — no orphans either direction)
> - Status column here ↔ Progress Checklist's current state (read, do not invent)
>
> **Ask triggers:**
> - Module marked Done in Progress Checklist but its verification rules haven't been re-checked since — flag for Ryan, don't assume.
> - Estimate has shifted such that overall scope is now materially different from Module Breakdown's Phase Plan — surface as a Section 6 Risk and ask Ryan whether Module Breakdown needs revision.
>
> Keep this block in during the build. Remove at Project Closeout only.

| Module | Status (from Progress Checklist) | Any surprises or scope changes? |
|--------|---------------------------------|--------------------------------|
| [M-01] | ✅ Done | [—] |
| [M-02] | 🔄 In Progress | [—] |
| [M-03] | 🔲 Not Started | [e.g., "More complex than estimated — see Section 6 Risk #2"] |

**Adjusted estimate:** [If remaining scope has changed materially since previous instance, note here. Otherwise: "No material change from Module Breakdown Phase Plan."]

---

## 🚦 Closeout Gate — Instance [N]

> 🤖 **AGENT INSTRUCTIONS — Closeout Gate**
>
> **Why this gate matters:** Closes this instance and authorizes opening a new one. Catches the two named failure modes: drift accumulation (Findings exist that weren't logged in time) and cosmetic-flag-as-resolved (✅ rows whose underlying fix didn't actually happen).
>
> **What you (the agent) verify before requesting sign-off:**
> - Every active section has its checklist boxes evaluated (✅ or unchecked — no 🔲 left ambiguous)
> - Every Finding row has a Proposed resolution AND a Resolved status
> - Every ✅ Resolved row passes its per-section verification rule (see each section's 🤖 block)
> - Every unresolved Finding is explicitly Deferred to instance [N+1] OR Accepted as a BD entry — no silent drops
> - Inactive sections have a single `N/A — [reason]` row stating why
>
> **What human sign-off means:** Ryan has reviewed Findings, confirmed resolutions are real (not cosmetic), and authorizes the instance to close. Build can continue. Next instance opens on the next trigger.
>
> Remove this instruction block at Project Closeout only. Keep the checklist + sign-off below for the lifetime of the build.

**Drift accumulation check (failure mode 1):**
- [ ] Findings in each active section were logged during the build window, not all at once at gate time
  - Evidence: timestamps or commit references on Findings rows (or, if no timestamps, agent attests rows were progressively added during the build window covered by this instance)
- [ ] No "obvious" drift went unlogged — if any coding session this window touched a module's logic without logging at least one Finding or N/A justification, that's suspicious; Ryan re-checks

**Commit-time logging audit (strongest drift-accumulation check):**
- [ ] Run `git log` for the build window covered by this instance (between this instance's open date and now)
- [ ] For every commit in the window, verify the commit body contains exactly one of:
  - `Mid-Build Review _[N] §[section]: Finding #[X] [logged|resolved]`
  - `Mid-Build Review _[N]: no drift`
  - `Mid-Build Review _[N]: out-of-scope (reason: ...)`
- [ ] Count commits in each bucket. Record below.
- [ ] For every "no drift" commit: spot-check at least 20% of them. Open the commit diff and confirm the affected code is genuinely aligned with the relevant design doc. If alignment is unclear, log a retroactive Finding and update the gate report.
- [ ] For every commit that DOES NOT contain any of the three required lines: log it as an audit gap below. Ryan decides whether to retroactively log Findings or accept the gap with a BD entry.

**Commit audit summary:**

| Bucket | Count |
|--------|-------|
| Commits referencing a Finding (logged or resolved) | [N] |
| `no drift` commits | [N] |
| `out-of-scope` commits | [N] |
| **Audit gaps (no line present)** | [N] |
| Total commits in window | [N] |

**Audit gaps detail (one row per missing-line commit):**

| Commit SHA | Date | Touched (files / area) | Retroactive resolution |
|------------|------|-----------------------|------------------------|
| [—] | [—] | [—] | [Finding #X logged retroactively / Accepted as BD-XX / Discussed with Ryan, no action needed] |

**Cosmetic-flag-as-resolved check (failure mode 2):**
- [ ] For every Section 1 Finding marked ✅ Resolved: the relevant Schema doc / migration / BD entry is verified to exist (open and confirm)
- [ ] For every Section 2 Finding marked ✅ Resolved: the API Contract / Tech Spec entry is verified; AC-XX contract test (if applicable) is passing in CI
- [ ] For every Section 3 Finding marked ✅ Resolved: the Tech Spec / Component-Service Map entry is verified; relevant UT/IT tests are passing
- [ ] For every Section 4 Finding marked ✅ Resolved: the UI/UX / UI Strings / Component Map entry is verified
- [ ] For every Section 5 row marked ✅ Done: the AD-XX or BD-XX entry is verified to exist
- [ ] For every Section 6 Risk marked ✅ Resolved: the fix is deployed and verified (not just merged)

**Deferred items roll forward:**
- [ ] Every unresolved Finding has `Resolved? = Deferred to instance [N+1]` OR has been converted to a BD-XX entry with note
- [ ] Findings that block continuing the build are surfaced to Ryan now, NOT deferred

**Phase boundary check (if this instance precedes Phase Closeout):**
- [ ] Unresolved Findings rolled into Phase Closeout for inclusion in that phase's report
- [ ] No Findings left in ambiguous state at phase boundary

**Bidirectional consistency (light — heavy checks happen at Phase Closeout):**
- [ ] Modules listed in Section 7 ↔ Module Breakdown Module Registry (no orphans either direction)
- [ ] Section 7 statuses ↔ Progress Checklist current state (consistent)

**Human sign-off:**
- [ ] Ryan: ☐ Approved — Mid-Build Review instance [N] closed. Next instance opens on next trigger. Build continues.
- [ ] Closed on: [YYYY-MM-DD]
