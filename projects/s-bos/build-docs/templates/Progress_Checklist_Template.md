# Progress Checklist: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized
> **Fill order:** Coding-phase doc. Scaffolded from `[AppName]_Module_Breakdown.md` AFTER Module Breakdown is signed off. Once scaffolded and Gate 0 (Initial Scaffold Lock) is signed off, this doc becomes a **runtime artifact** maintained continuously during the build.
>
> **This doc is different from every other template in the suite.**
> - Most templates are written once, signed off, then read.
> - This doc is written once, signed off (Gate 0), then **updated continuously by the agent during the build** as work completes.
> - The human reads this doc to see progress. The human does not curate the checkboxes — the agent does.
> - `🤖` and `❓ AGENT PAUSE` blocks remain in the doc during the build (they govern runtime behavior). Cleanup happens only at Project Closeout.
>
> **Source doc (scaffolding):**
> - `[AppName]_Module_Breakdown.md` — Module Registry, Phase Plan, Dependency Map, Build Order. Every Module here gets a row in Phase Summary AND a Module Checklist block in this doc.
>
> **Runtime reference docs (the agent reads these to decide if a checkbox can be ✅ honestly):**
> - `[AppName]_Database_Migration_Checklist.md` — migration status. A Data Layer migration checkbox cannot be ✅ until the corresponding DB-XX is marked ✅ Applied there.
> - `[AppName]_Testing_Strategy.md` — test status. A test checkbox cannot be ✅ until the referenced UT-XX / IT-XX / AC-XX / E2E-XX is passing in CI.
> - `[AppName]_API_Contract.md` — endpoint contracts. An API checkbox cannot be ✅ until the endpoint responds AND its AC-XX test passes.
> - `[AppName]_Component_Service_Layer_Map.md` — C-XX and S-XX inventory. A Services/UI checkbox cannot be ✅ unless the C-XX or S-XX it covers exists in code and matches its Detail Block.
> - `[AppName]_Build_Decisions_Log.md` — workarounds, deviations, gaps, concerns. **Any deviation discovered while building a module must be logged as a BD-XX entry BEFORE the related checkbox is checked.** Silent resolution is forbidden.
>
> **Downstream docs that consume this one (write to feed them):**
> - `[AppName]_Mid_Build_Review.md` — drift checks compare reality against the state of this doc.
> - `[AppName]_Phase_Closeout_[N].md` — each Phase Completion Gate here rolls up into a Phase Closeout. BD entries referenced in checked items are listed in Closeout.
> - `[AppName]_Project_Closeout.md` — overall build completion rolls up here at end of project.
> - The human reading this — primary consumer. This doc is the build's progress dashboard.
>
> **Agent role:** Honest checkbox maintenance. The agent owns the state of every checkbox in this doc. The agent updates Status fields as work progresses. The agent refuses to check a box unless the underlying work is verifiable (committed code + passing test + applied migration + matching upstream doc). When the agent encounters a deviation, workaround, gap, or concern: stop, write the BD entry first, then continue.
>
> **The three rules while maintaining this doc:**
> 1. Every check traces to verifiable evidence — committed code, passing test, applied migration, BD entry. No "probably done" checks. No "the code is written so I'll check it" without the test passing.
> 2. If completion is unclear, stop and ask — do not check on a guess. Mark 🔄 In Progress or 🚫 Blocked instead of leaving 🔲 silently.
> 3. Build-time events (workarounds, deviations, gaps, concerns) get logged to `[AppName]_Build_Decisions_Log.md` as `BD-XX` entries BEFORE the related checkbox is checked. Reference the `BD-XX` ID in the row's Notes column.
>
> **Commit-time logging rule (the strongest mitigation against silent resolution):**
>
> This rule mirrors the one in Mid-Build Review and reinforces it from the Progress Checklist side. It makes the absence of a Progress-Checklist update visible at the commit level.
>
> Every commit that touches code mapped to a Progress Checklist box MUST include exactly one of these lines in its commit body:
>
> 1. **Reference a checkbox change:** `Progress Checklist M-[XX] §[section]: [box description] ✅` (when a commit completes the work that justifies checking a box) or `Progress Checklist M-[XX] §[section]: [box description] 🔄` (when a commit advances but doesn't complete a box)
> 2. **Reference a BD entry being logged first:** `Build Decisions Log BD-[XX] logged; Progress Checklist M-[XX] §[section] deferred until BD resolved` (when a commit surfaces a deviation that must be logged before the checkbox can advance)
> 3. **Assert no Progress Checklist relevance:** `Progress Checklist: no relevance (reason: [why — e.g., "build tooling only," "refactor without behavior change," "infrastructure setup"])`
>
> **Why this matters:** Without the rule, the agent can commit code completing a module's work and forget to update the checklist. By the time Phase Completion Gate runs, the disconnect between code state and checklist state is invisible. With the rule, `git log | grep "Progress Checklist"` reveals compliance gaps. Phase Completion Gate has a matching audit check.
>
> **"Code mapped to a Progress Checklist box" means:**
> - Migration files → Data Layer section
> - Service / repository / model code → Business Logic / Services section
> - API route handlers / validators / error handlers → API / Endpoints section
> - React components / screens / hooks → UI / Frontend section
> - Test files → Tests section
> - Documentation-only commits to canonical project docs → may be "no relevance" if no checkbox is touched
>
> **Two failure modes drive most of the design here — both are addressed by gates:**
> - **Checkbox optimism** — agent checks ✅ before the work is verifiable (code committed but tests not run; migration written but not applied; endpoint coded but AC-XX test failing). The per-section verification rules in each Module Checklist block (and the Phase Completion Gates) catch this.
> - **Silent resolution** — agent encounters a deviation or workaround during a module, fixes it on the fly, and checks the box without logging a BD entry. By the time anyone notices, the rationale is lost. The locked rule "BD entry first, then check" and Phase Gate's BD-entry roll-up prevent this.
>
> **When this doc is fully filled and the project is closed out (NOT mid-build):** Remove every `🤖 AGENT INSTRUCTIONS` block, every `❓ AGENT PAUSE` prompt, and the agent-facing instruction prose inside `🚦 GATE` blocks. Keep the gate checklists, sign-off lines, every checkbox state, and every BD reference. The finished doc reads as the project's complete build record for humans.
>
> **Cleanup verification (only at Project Closeout — NOT before):**
> - Search the file for `🤖` — zero hits
> - Search the file for `❓ AGENT PAUSE` — zero hits
> - Search the file for "Remove this block" — zero hits
> - Every `🚦 GATE` block contains only its checklist and sign-off line — no agent prose
>
> **Internal fill order:**
> 1. **Initial Scaffolding** — from Module Breakdown: Phase Summary tables + one Module Checklist block per Module
> 2. **🚦 Gate 0 — Initial Scaffold Lock** (human sign-off before build begins)
> 3. **[ Build happens — agent updates Status, checks boxes, logs BD entries per the runtime rules ]**
> 4. **🚦 Phase Completion Gates** fire as each phase finishes (one per phase)
> 5. **🚦 Overall Build Completion Gate** fires when every phase is complete
> 6. **Project Closeout** — cleanup verification runs at the very end, not before

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Overview | 🔲 Not Started | — | — |
| Phase Summary | 🔲 Not Started | — | Scaffold from Module Breakdown |
| Module Checklist — Template Block | 🔲 Not Started | — | Canonical reference; per-module blocks reference it |
| Module Checklists (per-module blocks) | 🔲 Not Started | — | One block per M-XX |
| 🚦 Gate 0 — Initial Scaffold Lock | 🔲 Not Started | — | Human sign-off before build begins |
| 🚦 Phase Completion Gates | 🔲 Not Started | — | One per phase; fires at phase end |
| 🚦 Overall Build Completion Gate | 🔲 Not Started | — | Fires when all phases complete |

**Status values (this doc):** 🔲 Not Started / 🔄 In Progress / 👀 In Review / ✅ Done / 🚫 Blocked

---

## Overview

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Anchor this doc to its scaffolding source and runtime reference docs. Anyone reading this should immediately know which docs the agent reads to decide if a checkbox can be checked.
>
> **A complete Overview covers:**
> - App name
> - Scaffolding source doc (Module Breakdown)
> - All runtime reference docs explicitly listed
> - "How the agent maintains this doc" — one paragraph stating the agent owns checkbox state and refuses optimistic checks
>
> **Incomplete looks like:**
> - "Source docs: standard" without listing them
> - No statement of the agent's ownership of checkbox state
>
> **Remove this block at Project Closeout (not before).**

- **App:** [App Name]
- **Scaffolding source:** `[AppName]_Module_Breakdown.md` — Module Registry, Phase Plan, Dependency Map
- **Runtime reference docs (read to verify checkbox eligibility):**
  - `[AppName]_Database_Migration_Checklist.md` — migration status
  - `[AppName]_Testing_Strategy.md` — test pass/fail state
  - `[AppName]_API_Contract.md` — endpoint contracts
  - `[AppName]_Component_Service_Layer_Map.md` — C-XX / S-XX inventory
  - `[AppName]_Build_Decisions_Log.md` — BD-XX entries
- **Maintenance model:** The agent owns the state of every checkbox in this doc. The human reads the doc to see progress; the human does not curate boxes. The agent checks ✅ only when the underlying work is verifiable per the rules in each Module Checklist block. The agent refuses to check optimistically. The agent logs BD entries BEFORE checking the related box.

---

## Phase Summary

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Phase Summary is scaffolded BEFORE per-module blocks at doc creation. It is the high-level dashboard the human reads to see overall build state. The agent updates each Module's Status row whenever the corresponding Module Checklist block's Status changes — Phase Summary must always agree with the per-module Status fields.
>
> **Scaffolding (at doc creation):**
> - One sub-table per Phase in Module Breakdown → Phase Plan
> - One row per Module in that phase, copied from Module Breakdown's Module Registry: M-ID, Name, Type, Complexity, Status (🔲), Notes (—)
> - Phase grouping matches Module Breakdown's Phase Plan exactly
>
> **Runtime maintenance (during build):**
> - Whenever a Module's Module Checklist block Status changes, update the Phase Summary row Status to match
> - When a Module hits a blocker, mark 🚫 Blocked here AND in the Module Checklist block AND note the blocker in both Notes columns
> - If Module Breakdown's Phase Plan changes mid-build (Modules moved between phases, added, removed), log a BD-XX entry, then update Phase Summary, then update Module Checklists
>
> **Ask triggers:**
> - Module Breakdown's Module Registry and Phase Plan disagree on which Module belongs to which Phase → stop and flag; fix in Module Breakdown first
> - A Module exists in Module Breakdown's Module Registry but not in any Phase → flag the gap upstream
>
> **Cross-reference checklist (verify at scaffolding):**
> - Every Module in Module Breakdown's Module Registry appears in exactly one Phase Summary row
> - Phase numbers and names match Module Breakdown's Phase Plan
> - Complexity values (S/M/L) match Module Breakdown's Module Registry
>
> **Remove this block at Project Closeout (not before).**

> High-level dashboard of all phases and their modules. Reflects current build state at a glance.
> Detail is in the Module Checklist blocks below.

### Phase 1: [Phase Name]

| Module ID | Module Name | Type | Complexity | Status | Notes |
|-----------|-------------|------|------------|--------|-------|
| M-01 | [Name] | [Type] | [S/M/L] | 🔲 Not Started | — |
| M-02 | [Name] | [Type] | [S/M/L] | 🔲 Not Started | — |
| M-03 | [Name] | [Type] | [S/M/L] | 🔲 Not Started | — |

### Phase 2: [Phase Name]

| Module ID | Module Name | Type | Complexity | Status | Notes |
|-----------|-------------|------|------------|--------|-------|
| M-04 | [Name] | [Type] | [S/M/L] | 🔲 Not Started | — |
| M-05 | [Name] | [Type] | [S/M/L] | 🔲 Not Started | — |

*(Add Phase sub-tables as Module Breakdown's Phase Plan declares them.)*

---

## Module Checklist — Template Block (Canonical Reference)

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** This Template Block is the canonical reference for what a complete Module Checklist looks like. Every per-module block below references this block by section name. Items listed here are the standard items the agent checks for a Module of any type. Per-module blocks inline only the **module-specific items** (e.g., the specific endpoints for an API module, the specific components for a UI module) and any **section overrides** (e.g., "no Data Layer — pure UI module"). This prevents the doc from becoming 50 copies of the same 30-item list.
>
> **Per-section verification rules (THE RULES — these govern when a box can be ✅):**
>
> These rules are how the agent decides if a checkbox can be honestly marked ✅. Memorize them. Apply them on every check.
>
> | Section | Box | Can be ✅ ONLY when... |
> |---------|-----|------------------------|
> | Setup & Scaffolding | Folder structure created | Folder paths exist on disk matching the project's structure convention |
> | Setup & Scaffolding | Files created | Files exist on disk; agent has opened and confirmed they're the right files (not placeholder names) |
> | Setup & Scaffolding | Dependencies installed / imports wired | Project builds without unresolved imports; package manifest reflects new deps |
> | Data Layer | Migration written | DB-XX entry in Migration Checklist has its Detail Block filled in |
> | Data Layer | Migration applied (dev) | DB-XX entry in Migration Checklist is marked ✅ Applied |
> | Data Layer | Seed data added | Rows exist in dev DB matching Sample Data definitions |
> | Data Layer | Repository / query layer implemented | Repository class exists, query methods match Service Detail Block's data-access methods, and at least one query has been manually run successfully OR the IT-XX integration test for it is passing |
> | Business Logic / Services | [ServiceName] implemented | S-XX from Component/Service Map exists in code; class matches its Service Detail Block (methods, dependencies, transactional flags); UT-XX for the service is at least scaffolded |
> | Business Logic / Services | [Key logic / rule] implemented | The specific business rule is encoded; the UT-XX test case verifying the rule passes |
> | Business Logic / Services | Edge cases handled | Every error declared in Component/Service Map → Error Catalog for this S-XX has a UT case verifying it's thrown |
> | Business Logic / Services | Error handling in place | Every thrown error from this service is caught at the correct layer per Tech Spec → Error Handling |
> | API / Endpoints | `[METHOD] /path` implemented | Endpoint responds to a real request; AC-XX happy-path test for it passes |
> | API / Endpoints | Input validation in place | AC-XX validation test cases (400 VALIDATION_ERROR) pass |
> | API / Endpoints | Auth / permission checks in place | AC-XX auth test cases (401, 403) pass |
> | API / Endpoints | Error responses match API Contract | Every error row in API Contract for this endpoint has a passing AC-XX test |
> | UI / Frontend | [ScreenName / ComponentName] built | C-XX from Component/Service Map exists in code; matches its Component Detail Block (props, state, hooks); renders without errors |
> | UI / Frontend | Loading / Error / Empty states handled | Each state is reachable and renders the right content per UI/UX → Screens definitions |
> | UI / Frontend | Responsive behavior verified | Manually verified at breakpoints declared in UI/UX → Responsive Design |
> | Tests | Unit tests written (`UT-XX`) | UT-XX suite exists in Testing Strategy; test file exists in code |
> | Tests | Integration tests written (`IT-XX`) | IT-XX suite exists; test file exists |
> | Tests | API contract tests written (`AC-XX`) | AC-XX suite exists; test file exists |
> | Tests | E2E tests written (`E2E-XX`) | E2E-XX suite exists; test file exists |
> | Tests | All tests passing | CI shows all referenced tests for this Module passing on main |
> | Review & Completion | Code reviewed | Code has been reviewed (human or another agent); review comments resolved |
> | Review & Completion | Acceptance criteria met | Every Acceptance Criterion in this Module's Module Breakdown Detail Block has a verifying test ID listed AND that test is passing |
> | Review & Completion | BD entries logged | Every workaround, deviation, gap, or concern from this module is in Build Decisions Log as BD-XX, OR explicit "no BD entries this module" with one-line justification in the row's Notes |
> | Review & Completion | Module Breakdown status updated | Module Breakdown's Module Registry row for this M-XX is updated to ✅ Done |
>
> **The rule for ambiguity:** If verification is unclear for any single box, mark 🔄 In Progress or 🚫 Blocked at the Module level. Do NOT mark the box 🔲 and silently skip. Do NOT mark it ✅ on the assumption the work is "probably" done.
>
> **The rule for deviations:** If during a module the agent must deviate from any upstream doc (Module Breakdown, API Contract, Schema, Component/Service Map, etc.), STOP. Write the BD-XX entry first. Reference its ID in the row's Notes column. Then proceed.
>
> **Section override conventions for per-module blocks:**
> - Pure data Module (no API, no UI) → Sections active: Setup, Data, Tests, Review (omit API, UI)
> - Pure UI Module (no Data, no API) → Sections active: Setup, UI, Tests, Review (omit Data, API)
> - API + Data Module (no UI — e.g., headless service) → Sections active: Setup, Data, Services, API, Tests, Review (omit UI)
> - Full-stack Module → Sections active: all
>
> **Remove this block at Project Closeout (not before).**

> This is the canonical template every per-module block references. The agent checks boxes according to the per-section verification rules above.

### Sections (canonical)

- **Setup & Scaffolding**
  - Folder structure created
  - Files created
  - Dependencies installed / imports wired
- **Data Layer**
  - Migration written (`DB-XX`)
  - Migration applied (dev)
  - Seed data added (if needed)
  - Repository / query layer implemented
- **Business Logic / Services**
  - [ServiceName] implemented
  - [Key logic / rule] implemented
  - Edge cases handled
  - Error handling in place
- **API / Endpoints**
  - `[METHOD] /path` implemented (one box per endpoint)
  - Input validation in place
  - Auth / permission checks in place
  - Error responses match API Contract
- **UI / Frontend**
  - [ScreenName / ComponentName] built (one box per screen/component)
  - Loading states handled
  - Error states handled
  - Empty states handled
  - Responsive behavior verified
- **Tests**
  - Unit tests written (`UT-XX`)
  - Integration tests written (`IT-XX`)
  - API contract tests written (`AC-XX`)
  - E2E tests written (`E2E-XX`) — if applicable
  - All tests passing in CI
- **Review & Completion**
  - Code reviewed
  - Acceptance criteria from Module Breakdown met (every criterion has a passing test ID)
  - BD entries logged (or explicit "none this module" with justification)
  - Module Breakdown status updated to ✅ Done

---

## Module Checklists (Per-Module Blocks)

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Per-module blocks are scaffolded AFTER Phase Summary and AFTER the Template Block above is locked. Each per-module block declares which Template Block sections are active for that Module, then inlines only the module-specific items (specific endpoints, specific components, specific migrations). Section names without module-specific items are listed by header only.
>
> **Scaffolding (at doc creation):**
> - One block per Module in Module Breakdown's Module Registry
> - Header line: `### M-XX · [Module Name] · [Type]`
> - **Status:** 🔲 Not Started (at scaffold; updated during build)
> - **Phase / Complexity / Depends On:** copied from Module Breakdown's Module Registry and Dependency Map
> - **Sections active:** declare which sections from the Template Block apply (per the section override conventions above)
> - **Module-specific items:** list the specific endpoints (from API Contract), specific components (from Component/Service Map), specific migrations (from Migration Checklist), specific tests (from Testing Strategy) for this Module. Do NOT inline the full Template Block — reference its sections by name and only list what's specific.
>
> **Runtime maintenance (during build):**
> - When work starts on a Module: update Status to 🔄 In Progress; mirror to Phase Summary
> - As each box becomes verifiable per the Template Block verification rules: check ✅. NOT before.
> - When a box can't be verified: mark Status 🔄 In Progress (most of the module is going) or 🚫 Blocked (something specific is preventing progress), document the blocker, mirror to Phase Summary
> - When a deviation occurs: write BD-XX first, reference it in Notes, then check the related box
> - When all boxes are ✅: mark Status 👀 In Review (human or another agent reviews); after review, mark ✅ Done; mirror to Phase Summary; update Module Breakdown's Registry row to ✅ Done
>
> **Ask triggers:**
> - The agent cannot verify a box but the human is asking to mark the module complete → push back; cite the unverifiable box; offer 🚫 Blocked instead
> - A Module is being built that wasn't in the original Module Breakdown → STOP; this is a scope change; flag for BD-XX entry AND Module Breakdown amendment first
>
> **Remove this block at Project Closeout (not before).**

> One block per Module. Items follow the Template Block's verification rules. Find any module by searching its ID (e.g., M-01).

---

### M-01 · [Module Name] · [Type]

**Status:** 🔲 Not Started
**Phase:** [#] · **Complexity:** [S / M / L] · **Depends On:** [M-XX / —]
**Sections active:** [list per the override conventions, e.g., "Setup, Data, Services, API, Tests, Review (no UI — headless service)"]

**Blocker (if any):** —
**BD entries logged this module:** [list `BD-XX` IDs as they accrue, or "—"]

#### Setup & Scaffolding
- [ ] Standard items per Template Block

#### Data Layer
- [ ] Migrations to apply: `DB-01`, `DB-02`
- [ ] Standard items per Template Block

#### Business Logic / Services
- [ ] Services to implement: `S-01: [ServiceName]`
- [ ] Key logic / rule: [list module-specific rules]
- [ ] Standard items per Template Block

#### API / Endpoints
- [ ] Endpoints to implement:
  - `POST /api/v1/[resource]`
  - `GET /api/v1/[resource]/:id`
- [ ] Standard items per Template Block

#### Tests
- [ ] Test suites in scope for this Module:
  - Unit: `UT-01`, `UT-02`
  - Integration: `IT-01`
  - API Contract: `AC-01`, `AC-02`
- [ ] Standard items per Template Block

#### Review & Completion
- [ ] Standard items per Template Block

---

### M-02 · [Module Name] · [Type]

**Status:** 🔲 Not Started
**Phase:** [#] · **Complexity:** [S / M / L] · **Depends On:** [M-01]
**Sections active:** [list]

**Blocker (if any):** —
**BD entries logged this module:** —

*(Per-module body — same shape as M-01, with module-specific items only.)*

---

*(Continue pattern for every Module in Module Breakdown's Module Registry. One block per M-XX.)*

---

## 🚦 Gate 0 — Initial Scaffold Lock

> 🤖 **AGENT INSTRUCTIONS**
>
> **Why this gate matters:** This gate verifies the doc was scaffolded faithfully from Module Breakdown BEFORE the build begins. If Phase Summary, Module Checklist blocks, or the Template Block reference are wrong at the start, every checkbox checked during the build will be checked against a flawed scaffold. Catching the drift here is cheap; catching it after 20 boxes are checked is expensive.
>
> **Human sign-off is required before the build begins.** Do not begin building Module M-01 (or any Module) until this gate is checked.
>
> **Gate procedure:**
> 1. For every Module in Module Breakdown's Module Registry, open both this doc and Module Breakdown side-by-side and verify the scaffold. Do not check from memory.
> 2. Bidirectional checks: every Module in Module Breakdown has a row here AND every row here exists in Module Breakdown.
> 3. If any check fails, do NOT silently fix it. Stop, flag the gap, fix Module Breakdown first (if the gap is upstream) or re-scaffold this doc (if the gap is here), then re-run this gate.
>
> **Remove this instruction block at Project Closeout (not before). Keep the checklist and sign-off line.**

### Phase Summary Scaffold Checks

- [ ] Every Phase in Module Breakdown's Phase Plan has a sub-table in Phase Summary
- [ ] Every Module in Module Breakdown's Module Registry appears in exactly one Phase Summary row
- [ ] Phase numbers and names match Module Breakdown's Phase Plan exactly
- [ ] Complexity values (S/M/L) match Module Breakdown's Module Registry exactly
- [ ] All Status fields = 🔲 Not Started at scaffold time

### Module Checklist Block Scaffold Checks

- [ ] Every Module in Module Breakdown has a per-module block here (`### M-XX · [Name] · [Type]`)
- [ ] Module Name and Type match Module Breakdown's Module Registry exactly
- [ ] Phase / Complexity fields match Module Breakdown's Module Registry exactly
- [ ] Depends On field matches Module Breakdown's Dependency Map exactly (no missing dependencies, no extras)
- [ ] Sections active field is declared per the override conventions in the Template Block
- [ ] Module-specific items (endpoints, components, migrations, tests) are listed and match upstream docs:
  - Endpoints listed match API Contract's endpoints for this Module's Owning Module value
  - Components listed match Component/Service Map's Components owned by this Module
  - Migrations listed match Database Migration Checklist's Migrations owned by this Module
  - Tests listed match Testing Strategy's Coverage Plan by Module for this M-XX

### Bidirectional Module ID Link Checks

- [ ] **Module Breakdown → Progress Checklist:** opened Module Breakdown's Module Registry. For every M-XX row, confirmed a per-module block exists here.
- [ ] **Progress Checklist → Module Breakdown:** opened Progress Checklist's per-module blocks. For every block, confirmed an M-XX row exists in Module Breakdown's Module Registry. No orphan blocks.

### Template Block Reference Checks

- [ ] Module Checklist — Template Block section is filled with the canonical sections
- [ ] Per-section verification rules table is present in the Template Block 🤖 instructions
- [ ] Section override conventions are listed in the Template Block 🤖 instructions
- [ ] Every per-module block's "Sections active" field uses the override conventions correctly

### Sign-Off

- [ ] **Human sign-off** — Progress Checklist is faithfully scaffolded from Module Breakdown. Build may begin.

Signed: _____________________ Date: ___________

### Commit-Time Logging Rule Acknowledgement

> The commit-time logging rule (declared in the banner above) starts firing the moment the build begins. Before signing off this gate, confirm the rule is understood by every agent that will commit code during the build.

- [ ] Build agent (Claude Code) has acknowledged the commit-time logging rule and the three required commit-body line formats
- [ ] If multiple agents will commit during the build (e.g., a separate test agent), each has acknowledged the rule
- [ ] First commit after this gate signs off will be a baseline commit that includes a `Progress Checklist: no relevance (reason: gate sign-off commit)` line to establish the pattern

---

## 🚦 Phase Completion Gates

> 🤖 **AGENT INSTRUCTIONS**
>
> **Why these gates matter:** Each Phase Completion Gate verifies the agent's checkbox discipline held during the phase. The phase doesn't end because the agent checked all the boxes — it ends because the boxes were checked honestly (verifiable evidence) and every deviation was logged. This gate is where checkbox optimism and silent resolution get caught if they slipped through.
>
> **Human sign-off is required before declaring a Phase complete and beginning the next Phase.**
>
> **Gate procedure for each Phase:**
> 1. For every Module in the Phase, open its per-module block and verify all boxes are ✅. No 🔲, 🔄, or 🚫 remaining.
> 2. For every ✅ box in a Tests section, open Testing Strategy / CI and confirm the referenced test ID is actually passing — not just written.
> 3. For every ✅ box in a Data Layer section, open Migration Checklist and confirm the DB-XX is ✅ Applied — not just written.
> 4. For every ✅ box in an API section, open API Contract and confirm the AC-XX test for the endpoint passes.
> 5. For BD entries: every BD-XX referenced in Notes is checked against Build Decisions Log — confirm the entry exists and is in a closed state (✅ Resolved or 🟣 Accepted) OR has an explicit disposition for carrying forward to the next phase.
> 6. If any check fails: do NOT silently fix it. Stop, flag the gap, resolve, re-run this gate.
>
> **Roll-up to Phase Closeout:** Every BD entry from this Phase that was logged in Module Checklist Notes is listed in `[AppName]_Phase_Closeout_[N].md` → Build Decisions Log Summary, or explicit "no BD entries this phase + justification."
>
> **Remove this instruction block at Project Closeout (not before). Keep each phase's checklist and sign-off line.**

### Phase 1 Completion Gate

- [ ] All Phase 1 Module Checklist blocks have Status = ✅ Done
- [ ] All Phase 1 boxes are ✅ — no 🔲, 🔄, or 🚫 in any active section
- [ ] All Phase 1 migrations (`DB-XX`) are ✅ Applied in Migration Checklist (dev minimum)
- [ ] All Phase 1 tests (`UT-XX`, `IT-XX`, `AC-XX`, `E2E-XX`) are passing in CI on main
- [ ] All Phase 1 BD entries are logged in Build Decisions Log and listed in `[AppName]_Phase_Closeout_1.md`, OR explicit "no BD entries this phase + justification" in Closeout
- [ ] All open 🟠 Open entries in Build Decisions Log from this phase have been adjudicated (✅ Resolved, 🟣 Accepted, or explicit "carry forward to Phase 2" with rationale)
- [ ] Phase 1 smoke test executed: [describe what was manually verified]
- [ ] Phase Summary Status column for every Phase 1 Module = ✅ Done (mirrors Module Checklist block Status)

#### Commit-Time Logging Audit (Phase 1)

> Walk `git log` for the Phase 1 build window. For every commit, verify the commit body contains exactly one of the three required lines per the banner's commit-time logging rule. Count buckets. Spot-check "no relevance" commits.

- [ ] Ran `git log` for the Phase 1 window (between Gate 0 sign-off and now)
- [ ] Counted commits in each bucket (record below)
- [ ] For every "no relevance" commit: spot-checked at least 20% by opening the diff and confirming no Progress Checklist box was actually touched. If any false-negative found, retroactively check the appropriate box AND log the audit finding.
- [ ] For every commit missing any of the three lines: logged as audit gap (below)

**Commit audit summary — Phase 1:**

| Bucket | Count |
|--------|-------|
| Commits referencing a checkbox change (✅ or 🔄) | [N] |
| Commits referencing a BD entry logged first | [N] |
| `no relevance` commits | [N] |
| **Audit gaps (no required line present)** | [N] |
| Total commits in window | [N] |

**Audit gaps detail (one row per missing-line commit):**

| Commit SHA | Date | Touched (files / area) | Retroactive resolution |
|------------|------|------------------------|------------------------|
| [—] | [—] | [—] | [Box ✅ retroactively / BD-XX logged retroactively / Accepted as audit gap with rationale] |

#### Sign-Off

- [ ] **Human sign-off** — Phase 1 is complete. Phase 2 may begin.

Signed: _____________________ Date: ___________

---

### Phase 2 Completion Gate

- [ ] All Phase 2 Module Checklist blocks have Status = ✅ Done
- [ ] All Phase 2 boxes are ✅ — no 🔲, 🔄, or 🚫 in any active section
- [ ] All Phase 2 migrations (`DB-XX`) are ✅ Applied in Migration Checklist
- [ ] All Phase 2 tests are passing in CI on main
- [ ] All Phase 2 BD entries are logged and listed in `[AppName]_Phase_Closeout_2.md`, OR explicit "no BD entries this phase + justification"
- [ ] All open 🟠 Open entries from this phase have been adjudicated
- [ ] Phase 2 smoke test executed: [describe what was manually verified]
- [ ] Phase Summary Status column for every Phase 2 Module = ✅ Done

#### Commit-Time Logging Audit (Phase 2)

> Same procedure as Phase 1. Run `git log` for the Phase 2 build window.

- [ ] Commit audit ran for the Phase 2 window
- [ ] Bucket counts recorded
- [ ] `no relevance` spot-checks completed (≥20%)
- [ ] Audit gaps resolved or accepted with rationale

**Commit audit summary — Phase 2:**

| Bucket | Count |
|--------|-------|
| Commits referencing a checkbox change | [N] |
| Commits referencing a BD entry logged first | [N] |
| `no relevance` commits | [N] |
| **Audit gaps** | [N] |
| Total commits in window | [N] |

#### Sign-Off

- [ ] **Human sign-off** — Phase 2 is complete. Phase 3 may begin (or Overall Build Completion Gate if final phase).

Signed: _____________________ Date: ___________

---

*(Add Phase Completion Gate sub-sections per Phase in Module Breakdown's Phase Plan.)*

---

## 🚦 Overall Build Completion Gate

> 🤖 **AGENT INSTRUCTIONS**
>
> **Why this gate matters:** The app is code-complete only when every Phase is closed AND every artifact across every upstream doc has been touched by the build. This is the last cross-doc consistency check before Project Closeout. Drift here means an artifact (Module, endpoint, service, migration, test) was declared in design but never built.
>
> **Human sign-off is required.** Do not declare the build complete without explicit human approval.
>
> **Gate procedure:**
> 1. Walk every checklist item. For each, open the relevant upstream doc and verify — do not check from memory.
> 2. Bidirectional artifact checks across every upstream doc.
> 3. If any check fails: stop, flag the gap, resolve. The build is not complete until every artifact is accounted for.
>
> **Remove this instruction block at Project Closeout (not before). Keep the checklist and sign-off line.**

### Phase Completion

- [ ] Every Phase Completion Gate is signed off
- [ ] Phase Summary Status column for every Module = ✅ Done
- [ ] No Module Checklist block has Status ≠ ✅ Done

### Bidirectional Artifact Completion Checks

- [ ] **Module Breakdown ↔ Progress Checklist** — every M-XX in Module Breakdown is ✅ Done here; every ✅ Done here exists in Module Breakdown
- [ ] **Migration Checklist ↔ Progress Checklist** — every DB-XX in Migration Checklist is ✅ Applied; every Module's Data Layer section's migration list matches the Module's owning migrations in Migration Checklist
- [ ] **API Contract ↔ Progress Checklist** — every endpoint in API Contract is checked as implemented in some Module's API section; every endpoint listed in a Module's API section exists in API Contract
- [ ] **Component/Service Map ↔ Progress Checklist** — every C-XX and S-XX is built per its owning Module's Services / UI sections; every component/service listed in a Module's sections exists in Component/Service Map
- [ ] **Testing Strategy ↔ Progress Checklist** — every UT/IT/AC/E2E suite in Testing Strategy is checked as written + passing; every test ID listed in a Module's Tests section exists in Testing Strategy

### CI / Deployment Readiness

- [ ] Full test suite passing in CI on main
- [ ] Full test suite passing in CI on staging
- [ ] All migrations applied to staging
- [ ] Deployment Config filled in and staging deploy executed successfully
- [ ] Post-deploy verification checklist from Deployment Config passed

### Build Decisions Log Final State

- [ ] Every BD-XX entry across all phases has a final disposition: ✅ Resolved, 🟣 Accepted, or moved to V2 backlog with rationale
- [ ] No 🟠 Open BD entries remain
- [ ] All carry-forward dispositions from prior phases have been adjudicated

### Sign-Off

- [ ] **Human sign-off** — Build is code-complete. Project Closeout may begin.

Signed: _____________________ Date: ___________

---

## Project Closeout — Cleanup Verification

> 🤖 **AGENT INSTRUCTIONS**
>
> **Run only at Project Closeout — NOT before.** This is the final step before this doc is archived as the permanent build record.
>
> **Cleanup procedure:**
> 1. Confirm Overall Build Completion Gate is signed off
> 2. Confirm Project Closeout doc (`[AppName]_Project_Closeout.md`) has been written
> 3. Remove every `🤖 AGENT INSTRUCTIONS` block from this doc
> 4. Remove every `❓ AGENT PAUSE` prompt
> 5. Remove agent-facing instruction prose from inside `🚦 GATE` blocks (keep checklists and sign-off lines)
> 6. Verify with the search strings below
>
> **Remove this entire section after running the cleanup.**

- [ ] Searched the file for `🤖` — zero hits
- [ ] Searched the file for `❓ AGENT PAUSE` — zero hits
- [ ] Searched the file for "Remove this block" — zero hits
- [ ] Every `🚦 GATE` block contains only its checklist and sign-off line — no agent prose
- [ ] Every checkbox state in the doc is preserved (none accidentally reset)
- [ ] Every `BD-XX` reference is preserved
- [ ] Doc reads cleanly as the project's complete build record

Signed: _____________________ Date: ___________
