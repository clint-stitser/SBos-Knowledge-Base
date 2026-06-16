# Testing Strategy: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized
> **Fill order:** Coding-phase doc. Fills after Module Breakdown is signed off. Best filled AFTER API Contract and Component/Service Layer Map are signed off, since this doc references their C-XX / S-XX / endpoint IDs in every Test Case Spec. Can be drafted in parallel with them at the Coverage Plan level, but Test Case Specs cannot be finalized until the upstream IDs are locked.
>
> **Source docs (every test traces upstream to these):**
> - `[AppName]_Module_Breakdown.md` — Module Registry. Every Module must appear in Coverage Plan by Module. Module IDs are referenced in every Test Case Spec.
> - `[AppName]_Component_Service_Layer_Map.md` — Component Registry (`C-XX` IDs) and Service Registry (`S-XX` IDs). **Every C-XX with logic and every S-XX needs unit test coverage.** Every Service Detail Block's Exposed Methods table drives the per-method unit test cases here.
> - `[AppName]_API_Contract.md` — Endpoint Contracts. **Every endpoint here gets at least one AC-XX test suite.** Every error row in an Endpoint Contract becomes a test case. Every example payload becomes test input.
> - `[AppName]_Technical_Spec.md` — Authentication & Authorization (drives auth-related test cases), Error Handling (drives error-path test cases), State Machines (drives transition-path test cases), Events & Side Effects (drives integration test scope for async behavior), Deployment & Environments (drives smoke test environments).
> - `[AppName]_Product_Design_Doc.md` — User Workflows (drives E2E test scope — every critical workflow becomes an E2E suite), Success Metrics (drives performance test targets).
> - `[AppName]_DB_Schema.md` — Constraints / Validation Rules (drives validation error test cases), Sample Data (drives test fixture design).
>
> **Downstream docs that consume this one (write to feed them):**
> - `[AppName]_Module_Breakdown.md` — Each Module Detail Block's Acceptance Criteria → Verification section references test IDs (`UT-XX`, `IT-XX`, `AC-XX`, `E2E-XX`) from here. Bidirectional link — every test ID listed in a Module's Verification must exist here.
> - `[AppName]_Deployment_Config.md` — The CI/CD pipeline section here drives the build-time test stages declared there. Smoke tests defined here run post-deploy per Deployment Config's runbooks.
> - `[AppName]_Build_Decisions_Log.md` — BD entries reference test IDs when a Concern is resolved by a passing test (e.g., "Resolved by UT-14-03"). The Testing Philosophy rule "Every bug gets a test" connects directly to BD lifecycle.
> - `[AppName]_Mid_Build_Review.md` — Drift checks compare test coverage against built code.
> - `[AppName]_Pre_Build_Validation_Checklist.md` — Verifies that every C-XX, every S-XX, every endpoint, and every Module has at least one test ID referencing it from this doc.
> - Frontend & backend coding agents — read this doc directly when scaffolding tests. **The downstream-precision standard: a coding agent can write a complete test file (setup, mocks, test cases, assertions) from one Test Case Spec block — without re-reading Component/Service Map or API Contract.**
>
> **Agent role:** Translate every upstream artifact (Module, Component, Service, Endpoint, Workflow) into a test coverage plan and a set of executable Test Case Specs. The human is the designer; the agent enforces coverage completeness (no orphan artifacts) and test-pyramid discipline (unit-heavy, integration in the middle, E2E sparse). No invented test types. No invented test IDs.
>
> **The three rules while filling this doc:**
> 1. Everything written traces to Module Breakdown + Component/Service Map + API Contract + Tech Spec + PDD + Schema + confirmed human input. No invented test cases. No invented coverage assertions. Every UT-XX traces to a service method or component; every AC-XX traces to an endpoint; every E2E-XX traces to a PDD User Workflow.
> 2. If a test case's setup, input, or expected outcome is unclear, stop and ask. Do not write a vague test — a vague test that "passes" misleads more than no test at all.
> 3. Output must be specific enough that a coding agent can write a complete test file (setup, mocks, test cases with exact assertions) from one Test Case Spec block, without re-reading any upstream doc.
>
> **Two failure modes drive most of the design here — both are addressed by gates:**
> - **Coverage gaps disguised as percentages.** "80% line coverage" can completely miss critical paths if the missing 20% is the error handling. The real check is coverage by artifact: every endpoint has an AC test, every service has UT tests, every critical workflow has an E2E test. Gate 2 enforces artifact coverage, not percentage coverage.
> - **Test type confusion.** Writing E2E tests for things a unit test can cover, or writing unit tests for things only integration can verify (real DB constraints, real external API behavior). Inverts the test pyramid and produces a slow, brittle suite. Gate 1 locks the Test Type Reference and the rule "test at the right level"; Gate 2 verifies the pyramid shape (unit count > integration count > E2E count).
>
> **When this doc is fully filled and approved:** Remove every `🤖 AGENT INSTRUCTIONS` block, every `❓ AGENT PAUSE` prompt, and the agent-facing instruction prose inside `🚦 GATE` blocks. Keep the gate checklists, sign-off lines, and all filled content. The finished doc reads clean for humans.
>
> **Cleanup verification (before declaring the doc done):**
> - Search the file for `🤖` — zero hits
> - Search the file for `❓ AGENT PAUSE` — zero hits
> - Search the file for "Remove this block" — zero hits
> - Every `🚦 GATE` block contains only its checklist and sign-off line — no agent prose
>
> **Internal fill order (strict — section dependencies):**
> 1. Overview
> 2. **Testing Philosophy & Rules** — must come first; every test honors these rules
> 3. **Test Type Reference** — must come before Coverage Plans; coverage plans reference these types
> 4. **Coverage Plan by Module** — must come before Test Case Specs; spec IDs are allocated against this plan
> 5. **Coverage Plan by Feature / Workflow** — parallel with Coverage Plan by Module (different lens)
> 6. **🚦 Gate 1 — Foundation & Coverage Plan Lock** (human sign-off before writing individual Test Case Specs)
> 7. **Test Data & Fixtures** — must come before Test Case Specs (specs reference fixtures by name)
> 8. **Test Case Specs — Unit / Integration / API Contract / E2E / Other** — fill in this order; unit tests are most numerous, E2E most expensive
> 9. **CI/CD Integration** — fill after Test Case Specs (needs to know what's running and where)
> 10. **Coverage Targets** — fill after Test Case Specs (targets reflect what was written, not aspirational)
> 11. Open Questions — populate as they surface
> 12. **🚦 Gate 2 — Full Sign-Off** (final gate before downstream docs consume this)

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Overview | 🔲 Not Started | — | — |
| Testing Philosophy & Rules | 🔲 Not Started | — | Strict ordering — foundation lock |
| Test Type Reference | 🔲 Not Started | — | Strict ordering — locks test vocabulary |
| Coverage Plan by Module | 🔲 Not Started | — | Strict ordering — fill before Test Case Specs |
| Coverage Plan by Feature / Workflow | 🔲 Not Started | — | Parallel with Coverage Plan by Module |
| 🚦 Gate 1 — Foundation & Coverage Plan Lock | 🔲 Not Started | — | Human sign-off before Test Case Specs |
| Test Data & Fixtures | 🔲 Not Started | — | Strict ordering — fill before Test Case Specs reference fixtures |
| Test Case Specs — Unit | 🔲 Not Started | — | Most numerous test type |
| Test Case Specs — Integration | 🔲 Not Started | — | — |
| Test Case Specs — API Contract | 🔲 Not Started | — | One suite per endpoint |
| Test Case Specs — E2E | 🔲 Not Started | — | Critical user paths only |
| Test Case Specs — Other | 🔲 Not Started | — | Snapshot, Performance, Security, A11Y, Smoke — mark N/A if unused |
| CI/CD Integration | 🔲 Not Started | — | Fill after Test Case Specs |
| Coverage Targets | 🔲 Not Started | — | Fill after Test Case Specs |
| Open Questions | 🔲 Not Started | — | Populate as they surface |
| 🚦 Gate 2 — Full Sign-Off | 🔲 Not Started | — | Final gate before downstream docs |

**Coding Status values:** 🔲 Not Started / 🔄 In Progress / 👀 In Review / ✅ Done / 🚫 Blocked

---

## Overview

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Anchor this doc to its sources and declare the test frameworks. Anyone reading this should immediately know what's tested, what frameworks are used per layer, and where the source-of-truth docs live.
>
> **A complete Overview covers:**
> - App name
> - All six source docs explicitly listed
> - Test frameworks per layer (Unit backend, Unit frontend, Integration, API Contract, E2E, Other) — each row filled from Tech Spec → Tech Stack
> - Total test count by type (filled at Gate 2, blank until then)
>
> **Incomplete looks like:**
> - "Jest" in the Unit row without specifying backend or frontend
> - A framework row marked "—" when that test type is actually used in the project
> - Source docs missing
>
> **Ask triggers:**
> - Tech Spec → Tech Stack is silent on test frameworks → ask before assuming defaults
> - Project uses a test framework not declared in Tech Spec → flag the gap upstream
>
> **Remove this block before delivering the filled doc.**

- **App:** [App Name]
- **Source docs:**
  - Module Breakdown: `[AppName]_Module_Breakdown.md`
  - Component/Service Map: `[AppName]_Component_Service_Layer_Map.md`
  - API Contract: `[AppName]_API_Contract.md`
  - Tech Spec: `[AppName]_Technical_Spec.md`
  - PDD: `[AppName]_Product_Design_Doc.md`
  - Schema: `[AppName]_DB_Schema.md`
- **Test frameworks:**

| Layer | Framework | Notes |
|-------|-----------|-------|
| Unit (backend) | [e.g., Jest / Vitest / pytest] | — |
| Unit (frontend) | [e.g., Vitest + React Testing Library] | — |
| Integration | [e.g., Jest + Testcontainers / pytest + Docker Compose] | — |
| API Contract | [e.g., Supertest / pytest + httpx] | — |
| E2E | [e.g., Playwright / Cypress / Detox] | — |
| Other | [e.g., k6 for perf, axe-core for a11y] | — |

- **Total tests at Gate 2 (filled at sign-off):**
  - Unit: [#] suites, [#] cases
  - Integration: [#] suites, [#] cases
  - API Contract: [#] suites, [#] cases
  - E2E: [#] suites, [#] cases
  - Other: [#]

---

## Testing Philosophy & Rules

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Testing Philosophy & Rules is filled BEFORE Test Type Reference and BEFORE any Coverage Plan. These rules govern every test in the project. Once locked at Gate 1, deviations require an explicit Build Decisions Log entry.
>
> **Your job:** Lock the testing rules for the project. These are not negotiable per test — they are the contract every test honors.
>
> **A complete Testing Philosophy & Rules section covers:**
> - "What We Test" rules — 5 defaults, all kept unless the human explicitly removes one with rationale
> - "What We Don't Test" rules — 4 defaults, all kept unless the human explicitly removes one with rationale
> - Naming convention table — one pattern picked per row, no ambiguity
> - "Every bug gets a test" rule is reconciled with Build Decisions Log Concern → Resolved lifecycle (test ID closes the BD entry)
>
> **Incomplete looks like:**
> - Rules listed without descriptions
> - Two different naming conventions for the same test type (e.g., `.test.ts` AND `.spec.ts` — pick one)
> - "Every bug gets a test" without specifying how it connects to Build Decisions Log Concerns
>
> **Ask triggers:**
> - Tech Spec → Tech Stack uses a framework whose convention contradicts a template default (e.g., framework expects `__tests__` folders) → ask before keeping the default
> - Project plans to skip a default rule (e.g., "we don't write E2E tests") → ask for rationale before deleting the rule from the table; this becomes a Build Decisions Log entry
>
> **Remove this block before delivering the filled doc.**

> These rules govern how tests are written across the entire project. Every deviation is a deliberate exception logged in Build Decisions Log — not a habit.

### What We Test

| Rule | Description |
|------|-------------|
| **Test behavior, not implementation** | Tests verify what the code does, not how it does it. Refactoring internals without changing behavior should not break tests. |
| **Test at the right level** | Unit tests for isolated logic. Integration tests for wired-together layers. E2E for critical user paths only. Don't write E2E tests for things a unit test can cover cheaper. |
| **Every bug gets a test** | When a bug is found and fixed, a test is added that would have caught it. This prevents regression. **If the bug was the subject of an open `⚠️ Concern` entry in the Build Decisions Log, the passing test is what closes that Concern — update the BD entry status to ✅ Resolved with a reference to the test ID (e.g., `Resolved by UT-14-03`).** |
| **Tests must be deterministic** | A test that passes sometimes and fails sometimes is worse than no test. Flaky tests get fixed or deleted — never ignored. |
| **Tests are first-class code** | Tests get the same care as production code — clear naming, no copy-paste, meaningful assertions. |

### What We Don't Test

| Rule | Description |
|------|-------------|
| **Don't test framework internals** | Don't write tests that verify your ORM saves to the DB — that's the ORM's job. Test your logic on top of it. |
| **Don't test getters and setters** | Trivial accessors with no logic don't need tests. |
| **Don't test third-party libraries** | Mock them. Test your code's behavior when they return expected and unexpected results. |
| **Don't test implementation details** | If a test breaks when you rename a private method, the test is wrong. |

### Naming Convention

> Pick one convention per row and use it everywhere. No mixing within the same test type.

| Layer | Convention | Example |
|-------|-----------|---------|
| Unit test file | `[filename].test.[ext]` OR `[filename].spec.[ext]` (pick one) | `UserService.test.ts` |
| Integration test file | `[filename].integration.test.[ext]` | `UserService.integration.test.ts` |
| E2E test file | `[filename].e2e.test.[ext]` OR `[filename].e2e.[ext]` (pick one) | `login.e2e.ts` |
| Test case name | `[method/action] [condition] [expected result]` | `"createUser when email already exists throws DuplicateEmailError"` |
| Describe block | `[ClassName / component / route]` | `"UserService"`, `"POST /api/v1/users"` |

---

## Test Type Reference

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Test Type Reference is filled AFTER Testing Philosophy & Rules and BEFORE either Coverage Plan. Coverage Plans reference these types by name; without the type vocabulary locked, plans reference shapes that aren't defined.
>
> **Your job:** Confirm the test types cover this project's needs. Add types only if the project genuinely needs one not listed (rare). Do not delete rows — keep the full reference even if some types are unused this project (mark "Not used" in Notes for those).
>
> **A complete Test Type Reference:**
> - All 11 standard types are present (Unit, Integration, API Contract, E2E, Snapshot, Performance, Security, Accessibility, Visual Regression, Smoke, Contract/Pact)
> - Project-specific types (if any) are appended with the same column structure
> - Types not used this project are marked "Not used" in a Notes column rather than deleted (preserves the reference)
> - Test pyramid rule is preserved (Unit > Integration > E2E in count)
>
> **Ask triggers:**
> - Project introduces a test pattern not covered by the 11 standard types (e.g., mutation testing, chaos engineering) → ask the human whether to add a new row
> - Project decides to skip a major type entirely (e.g., "no E2E tests at all") → ask for rationale; log in Build Decisions Log
>
> **Remove this block before delivering the filled doc.**

> Use these types in the Coverage Plans and Test Case Specs below. Add types as needed — this list is a starting point.

| Type | Scope | Speed | What It Tests | When to Use |
|------|-------|-------|--------------|-------------|
| **Unit** | Single function, method, or class | Fast (ms) | Isolated business logic, edge cases, error paths | Every service method, utility function, and complex component logic |
| **Integration** | Multiple real components wired together | Medium (seconds) | Layer interaction — service + repository, service + external client | DB queries, service-to-service calls, middleware chains |
| **API Contract** | HTTP endpoint, request in / response out | Medium | Endpoint behavior, validation, auth, status codes, response shape | Every API endpoint, at least happy path + key error paths |
| **E2E** | Full system, real browser or client | Slow (seconds–minutes) | Complete user workflows from UI to DB and back | Critical paths only — login, primary CRUD, checkout, etc. |
| **Snapshot** | UI component output | Fast | Renders consistently — catches unintended visual regressions | Shared UI components, layout components |
| **Performance** | System under load | Variable | Latency, throughput, degradation under concurrent requests | Key endpoints before launch; re-run after major changes |
| **Security** | Auth, input handling, access control | Variable | Auth bypass, injection, unauthorized access, sensitive data exposure | Auth flows, permission boundaries, all input surfaces |
| **Accessibility** | UI rendering | Fast–medium | WCAG compliance, keyboard nav, screen reader compatibility | Every screen; automated pass first, manual follow-up |
| **Visual Regression** | UI screenshot diff | Medium | Pixel-level or component-level rendering changes | Design system components; CI-gated on shared components |
| **Smoke** | Critical paths only, post-deploy | Fast | System is basically up and running after a deploy | Run after every deploy to staging and production |
| **Contract (Pact)** | API consumer/provider agreement | Medium | Consumer and provider stay in sync when contracts change | Multi-team APIs; consumer-driven contract testing |

> ⚠️ **Test pyramid:** Unit tests should be most numerous, integration tests fewer, E2E tests fewest. Inverting this makes your suite slow and brittle. Gate 2 verifies this shape holds.

### ID Format Declarations

> Auxiliary test IDs are declared per-project per the convention in `Design_Document_Template_Context.md` → ID Conventions. Confirm the format below; adjust if a project uses a different aux ID.

| Type | ID Format | Notes |
|------|-----------|-------|
| Unit | `UT-NN` | Core ID per Context doc |
| Integration | `IT-NN` | Core ID per Context doc |
| API Contract | `AC-NN` | Core ID per Context doc |
| E2E | `E2E-NN` | Core ID per Context doc |
| Snapshot | `SNAP-NN` | Auxiliary — declared here |
| Performance | `PERF-NN` | Auxiliary — declared here |
| Security | `SEC-NN` | Auxiliary — declared here |
| Accessibility | `A11Y-NN` | Auxiliary — declared here |
| Smoke | `SMOKE-NN` | Auxiliary — declared here |

---

## Coverage Plan by Module

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Coverage Plan by Module is filled AFTER Test Type Reference and BEFORE any Test Case Spec. Every Test Case Spec ID is allocated against this plan. Without the plan locked, specs are written against guesses.
>
> **Your job:** For every Module in `[AppName]_Module_Breakdown.md`, declare which test types are needed and at what priority. This is the high-level coverage plan — detail lives in the Test Case Spec sections below.
>
> **A complete Coverage Plan by Module covers:**
> - One row per Module in Module Breakdown (every M-XX appears)
> - Each row marks each test type as ✅ Needed, ⚠️ Optional, — (not applicable), or 🚫 Skip (with reason in Notes)
> - Priority column (High / Med / Low) reflects launch criticality
> - Notes column captures non-obvious coverage decisions
>
> **Incomplete looks like:**
> - A Module from Module Breakdown missing from this table
> - All cells marked ⚠️ Optional (no commitment — that's a planning failure)
> - A 🚫 Skip with no reason in Notes
> - High-priority Module with no test type marked ✅ Needed
>
> **Coverage discipline (the agent applies this for every Module row):**
> - Every Module with Service Detail Blocks in Component/Service Map needs Unit tests (✅ Needed)
> - Every Module that introduces or modifies an endpoint in API Contract needs API Contract tests (✅ Needed)
> - Every Module touching the database needs at least one Integration test for DB behavior (✅ Needed for first such Module; ⚠️ Optional for subsequent Modules covering the same patterns)
> - Every Module that owns a critical user workflow (per PDD) needs E2E coverage (✅ Needed)
> - UI-only Modules (purely presentational components) typically need Unit + Snapshot, not Integration
>
> **Ask triggers:**
> - A Module has no test type marked ✅ Needed → ask whether the Module is purely structural (rare) or whether coverage was overlooked
> - A High-priority Module is marked 🚫 Skip for E2E without a critical workflow trace → ask before accepting the skip
>
> **Cross-reference checklist (verify before declaring section done):**
> - Every M-XX in `[AppName]_Module_Breakdown.md` Module Registry has a row here
> - Every Module that owns endpoints in API Contract has ✅ Needed in the API Contract column
> - Every Module that owns services in Component/Service Map has ✅ Needed in the Unit column
>
> **Remove this block before delivering the filled doc.**

> High-level view of what tests are needed per module. References Module IDs from `[AppName]_Module_Breakdown.md`.
> This is a planning tool — detail lives in the Test Case Spec sections below.

| Module ID | Module Name | Unit | Integration | API Contract | E2E | Other | Priority | Notes |
|-----------|-------------|------|-------------|--------------|-----|-------|----------|-------|
| M-01 | [Name] | ✅ Needed | ✅ Needed | — | — | — | High | — |
| M-02 | [Name] | ✅ Needed | ✅ Needed | ✅ Needed | — | — | High | — |
| M-03 | [Name] | ✅ Needed | — | ✅ Needed | ✅ Needed | — | High | — |
| M-04 | [Name] | ✅ Needed | — | — | — | Snapshot | Med | UI components only |

> **Priority guide:** High = blocks launch / core functionality. Med = important but not blocking. Low = nice to have / polish.

### Coverage Matrix Legend

| Symbol | Meaning |
|--------|---------|
| ✅ Needed | This test type is required for this module |
| ⚠️ Optional | Useful but not required |
| — | Not applicable |
| 🚫 Skip | Explicitly decided not to test this way — reason required in Notes; log in Build Decisions Log |

---

## Coverage Plan by Feature / Workflow

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Map every user-facing feature and workflow (from PDD) to test coverage. This is a complementary lens to Coverage Plan by Module — Module view is technical, Feature/Workflow view is user-facing. Both views must reach Gate 2 with no gaps.
>
> **A complete Coverage Plan by Feature / Workflow covers:**
> - One row per Feature in PDD → Core Features
> - One row per Workflow in PDD → User Workflows (especially error paths)
> - Each row declares whether the happy path is covered, whether key error paths are covered, the test types used, and the specific Test IDs
> - Test IDs are filled at Gate 2 (or as Test Case Specs are written) — until then they're 🔲 placeholders
>
> **Incomplete looks like:**
> - A PDD Feature missing from this table
> - A PDD Workflow missing from this table
> - Happy Path Covered = ⏳ in a critical workflow at Gate 2 (must be ✅)
> - Test IDs column empty after Test Case Specs are written
>
> **Error paths to always cover (default list, deviations need rationale):**
> - Input missing or invalid (400)
> - Not authenticated (401)
> - Not permitted (403)
> - Resource not found (404)
> - Business rule violation (422)
> - Concurrent modification / duplicate (409)
>
> **Ask triggers:**
> - A PDD Workflow has 8+ steps and only one test → ask whether sub-flows should be split into separate test cases
> - A Feature has no error path tests planned → ask which error paths matter for this feature
>
> **Cross-reference checklist:**
> - Every PDD Core Feature has a row
> - Every PDD User Workflow has a row
> - Every Test ID listed exists in the relevant Test Case Spec section
>
> **Remove this block before delivering the filled doc.**

> Maps user-facing features and workflows (from PDD) to test coverage. Answers "is this thing the user does actually tested?"
> Use in conjunction with the by-module view — they're complementary, not redundant.

| Feature / Workflow | Happy Path Covered? | Key Error Paths Covered? | Test Types | Test IDs | Notes |
|--------------------|--------------------|--------------------------|-----------|---------|----|
| [e.g., User registers] | ⏳ | ⏳ | Unit, API, E2E | UT-01, AC-01, E2E-01 | — |
| [e.g., User logs in] | ⏳ | ⏳ | Unit, API, E2E | UT-02, AC-02, E2E-02 | — |
| [e.g., Create project] | ⏳ | ⏳ | Unit, API | UT-05, AC-04 | No E2E — covered by unit + API |
| [e.g., Soft delete task] | ⏳ | ⏳ | Unit, Integration | UT-10, IT-03 | — |

> **Error paths to always cover:**
> - Input missing or invalid (400)
> - Not authenticated (401)
> - Not permitted (403)
> - Resource not found (404)
> - Business rule violation (422)
> - Concurrent modification / duplicate (409)

---

## 🚦 Gate 1 — Foundation & Coverage Plan Lock

> 🤖 **AGENT INSTRUCTIONS**
>
> **Why this gate matters:** Testing Philosophy, Test Type Reference, and both Coverage Plans are referenced by every Test Case Spec. Mistakes here mean rewriting hundreds of specs. This gate exists to catch foundation problems before that cost is incurred.
>
> **Human sign-off is required before Test Case Specs begin.** Do not start writing UT-XX, IT-XX, AC-XX, or E2E-XX blocks until this gate is checked.
>
> **Gate procedure:**
> 1. Walk every checklist item. For each, open the relevant section and verify — do not check from memory.
> 2. Coverage Plan checks require opening Module Breakdown and PDD to confirm every Module and Feature is represented. Do not skip this.
> 3. If any check fails, do NOT silently fix it. Stop, flag the gap, and resolve before continuing.
>
> **Remove this instruction block before delivering. Keep the checklist and sign-off line.**

### Testing Philosophy Checks

- [ ] All 5 "What We Test" rules are present (or removed with documented rationale + BD entry)
- [ ] All 4 "What We Don't Test" rules are present (or removed with rationale)
- [ ] Naming convention is decided (one pattern per row — no mixing)
- [ ] "Every bug gets a test" rule explicitly connects to Build Decisions Log Concern → Resolved lifecycle

### Test Type Reference Checks

- [ ] All 11 standard test types are present
- [ ] Types not used this project are marked "Not used" in Notes (not deleted)
- [ ] Test pyramid principle is preserved in the table
- [ ] ID Format Declarations table is filled for every type used

### Coverage Plan by Module Checks

- [ ] Every M-XX in `[AppName]_Module_Breakdown.md` Module Registry has a row here
- [ ] Every row has at least one test type marked ✅ Needed OR a 🚫 Skip with documented rationale
- [ ] Every Module with Service Detail Blocks in Component/Service Map has ✅ Needed in the Unit column
- [ ] Every Module that introduces or modifies an endpoint in API Contract has ✅ Needed in the API Contract column
- [ ] Every Module owning a critical PDD User Workflow has ✅ Needed in the E2E column
- [ ] Every 🚫 Skip has a reason in Notes AND a Build Decisions Log entry

### Coverage Plan by Feature / Workflow Checks

- [ ] Every PDD Core Feature has a row
- [ ] Every PDD User Workflow has a row
- [ ] Test Types column is filled for every row (no ⏳ in the Test Types column itself — Test IDs may be ⏳ until specs are written)
- [ ] Error path defaults (400 / 401 / 403 / 404 / 422 / 409) are accounted for in every Feature where they apply

### Cross-Doc Consistency Checks

- [ ] **Module Breakdown ↔ Coverage Plan by Module** — every M-XX appears here; no extras
- [ ] **PDD ↔ Coverage Plan by Feature** — every Feature and every Workflow appears here; no extras
- [ ] **Tech Spec ↔ Test Type Reference** — test frameworks declared here match Tech Spec → Tech Stack

### Sign-Off

- [ ] **Human sign-off** — Testing Philosophy, Test Type Reference, and both Coverage Plans approved. Test Case Specs may begin.

Signed: _____________________ Date: ___________

---

## Test Data & Fixtures

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Test Data & Fixtures is filled AFTER Gate 1 sign-off and BEFORE any Test Case Spec. Specs reference fixtures by name (`createUser()`, `TEST_USER_EMAIL`); without fixtures defined, specs reference values that don't exist.
>
> **Your job:** Centralize the test data the entire suite uses. Single source of truth — no magic values scattered across spec files. Cross-reference with `[AppName]_Sample_Data.md` to keep sample data and test data aligned.
>
> **A complete Test Data & Fixtures section covers:**
> - Seed datasets (named, scoped to which test types, location on disk)
> - Fixed test values (constants used across multiple tests — emails, IDs, tokens)
> - Factory functions / builders (programmatic test data generation)
>
> **Incomplete looks like:**
> - "Seed data: users" without specifying which test types use it
> - A test value referenced in any spec block but missing from this section
> - Factory with no defaults declared
>
> **Cross-reference checklist:**
> - Seed data is consistent with `[AppName]_Sample_Data.md` (don't have two competing definitions of "test users")
> - Fixed test values use realistic shapes per Schema → Data Dictionary (e.g., UUID format for IDs)
> - Factories produce shapes consistent with Schema → Entities
>
> **Ask triggers:**
> - Sample Data and Test Data overlap but differ → ask whether to unify or document the difference
> - A test value embeds real production-like data (e.g., real email domains, real names) → ask before keeping
>
> **Remove this block before delivering the filled doc.**

> Defines the shared test data used across suites. Single source of truth — don't scatter magic values in test files.

### Seed Data

| Dataset | Used By | Description | Location |
|---------|---------|-------------|----------|
| `users.seed` | Unit, Integration, API, E2E | [N] test users with known credentials and IDs | `[path/to/seeds]` |
| `[entity].seed` | [Test types] | [Description] | `[path]` |

### Fixed Test Values

> Values that appear in multiple tests. Centralize them — when they change, change in one place.

| Name | Value | Used In | Notes |
|------|-------|---------|-------|
| `TEST_USER_EMAIL` | `test@example.com` | Auth tests, E2E | Exists in seed data |
| `TEST_USER_PASSWORD` | `hunter2` | Auth tests, E2E | Never use in production |
| `TEST_USER_ID` | `[UUID]` | Unit mocks, API tests | Matches seed |
| `VALID_JWT` | `[token]` | API tests | Generated from test secret, long expiry |
| `EXPIRED_JWT` | `[token]` | Auth error tests | Pre-expired |
| `[NAME]` | `[value]` | [—] | [—] |

### Factory Functions / Builders

> If using a factory pattern (e.g., `factory-girl`, `fishery`, custom builders), document what factories exist.

| Factory | Produces | Defaults | Overrides |
|---------|---------|---------|----------|
| `createUser()` | `User` | `{ status: "active", role: "user" }` | Any field |
| `createProject()` | `Project` | `{ status: "active" }` | Any field |
| `create[Entity]()` | `[Entity]` | `{ [defaults] }` | Any field |

---

## Test Case Specs — Unit

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Unit specs are filled AFTER Gate 1 sign-off and Test Data & Fixtures. Unit specs are typically the most numerous — write them first to lock in the testable shape of the service layer.
>
> **Your job:** Produce one Test Case Spec block per service class, utility module, or component with logic. Every public method of every service gets test cases. Every error path in Component/Service Map → Error Catalog gets a test case.
>
> **A complete Unit spec block covers:**
> - Header: `UT-NN · [ClassName/ComponentName]` — exactly matches Coverage Plan by Module
> - Module reference (M-XX)
> - File path
> - "What it tests" — one sentence
> - Dependencies mocked — every Injected Dependency from Component/Service Map → Service Detail Block must be mocked here
> - Test Cases table — one row per case; ID format `UT-NN-MM`
> - Detailed Test Case Blocks for non-obvious cases (happy paths rarely need them; error paths often do)
>
> **Coverage discipline for unit tests (the agent applies this for every spec):**
> - Every public method of every service has at least one happy-path test case
> - Every error a method throws (per Service Contract) has a test case verifying that error is thrown
> - Every boundary condition (null inputs, empty collections, max-length strings, etc.) has a test case
> - Every state-machine transition (per Tech Spec → State Machines) has a test case for the transition AND a test case for the guard failure
>
> **Incomplete looks like:**
> - A service method with only a happy-path test (no error case)
> - "Dependencies mocked: standard" without listing them
> - "Expected output: works correctly" — not specific
> - A test case named "edge case" with no specifics
>
> **Cross-reference checklist (verify before declaring each block done):**
> - UT-NN ID is unique
> - ClassName matches a C-XX or S-XX in Component/Service Map
> - Module ID matches Coverage Plan by Module
> - Every error tested exists in Component/Service Map → Error Catalog
> - Every input shape uses a DTO from Component/Service Map → DTOs
>
> **Ask triggers:**
> - A service method has 8+ test cases → ask whether the method is doing too much (Design Rules: Single Responsibility) or whether the cases can be table-driven
> - A test case requires complex setup → ask whether the setup itself needs a factory function
>
> **Remove this block before delivering the filled doc.**

> One block per test suite (typically one per service class, utility module, or component with logic).
> Find any test by its ID (e.g., UT-01).

**ID format:** `UT-NN`

---

### UT-01 · [ServiceName / ClassName / ComponentName]

**Module:** M-[##]
**File:** `[path/to/test/file]`
**What it tests:** [One sentence — what unit is under test and why it matters]

**Dependencies mocked:**
- `[DependencyName]` — [What it does and why it's mocked, not real]
- `[DependencyName]` — [e.g., Repository — avoid real DB in unit tests]

---

#### Test Cases

| ID | Name | Input | Expected Output | Edge Case? | Status |
|----|------|-------|----------------|------------|--------|
| UT-01-01 | [method] happy path | [Input description] | [Expected return or side effect] | No | 🔲 |
| UT-01-02 | [method] [condition] throws [ErrorName] | [Input that triggers error] | Throws `[ErrorName]` | Yes | 🔲 |
| UT-01-03 | [method] [boundary condition] | [Boundary input] | [Expected behavior at boundary] | Yes | 🔲 |

---

#### Detailed Test Case Blocks

> Use these blocks for non-obvious test cases. Skip for straightforward happy paths.

##### UT-01-02 · [method] [condition] throws [ErrorName]

**Setup:**
```
[Describe the state of the world before the test — mock return values, DB state, etc.]
mock.[dependency].[method].returns([value])
```

**Input:**
```
[Exact input to the method under test]
[ServiceName].[method]({ field: "value", ... })
```

**Expected behavior:**
- Throws `[ErrorName]`
- [Any other assertions — e.g., "does NOT call repository.save()"]
- [e.g., "logs the error at WARN level"]

**Why this matters:** [Why is this edge case worth explicitly testing?]

---

### UT-02 · [ServiceName / ClassName / ComponentName]

*(Continue pattern for UT-03, UT-04, etc. One block per service / class / component with logic.)*

---

## Test Case Specs — Integration

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Produce one Test Case Spec block per real-component-pair under test. Integration tests cover the interactions unit tests cannot: real DB queries against real schema, real external API calls against sandboxes or fixtures, real middleware chains.
>
> **A complete Integration spec block covers:**
> - Header: `IT-NN · [ServiceName] + [RepositoryName / ExternalClient]`
> - Module reference
> - File path
> - "What it tests" — what real interaction is being verified
> - Setup (DB state, external client configuration)
> - Test Cases table with Preconditions, Input, Expected Outcome, Teardown columns
> - Detailed blocks for transaction rollback, retry/timeout behavior, FK constraint enforcement
>
> **Coverage discipline for integration tests:**
> - Every Transactional Operation declared in Component/Service Map → Service Detail Blocks gets an IT test verifying atomicity (success path + rollback path)
> - Every External client integration (Stripe, SendGrid, etc.) gets IT tests for success, error response, and timeout behavior
> - Every FK constraint declared in Schema gets at least one IT test verifying enforcement
> - Every state machine transition that touches the DB gets an IT test (unit covers the logic; integration confirms the write)
>
> **Incomplete looks like:**
> - A Transactional Operation with no rollback test
> - An External client integration with only a happy-path test (no error or timeout case)
> - "Preconditions: DB seeded" without specifying what's seeded
> - "Teardown: cleanup" without specifying what cleans up what
>
> **Cross-reference checklist:**
> - IT-NN ID is unique
> - ServiceName matches an S-XX in Component/Service Map
> - Every Transactional Operation declared in Component/Service Map appears at least once across IT specs
> - Every External client in Tech Spec → Dependencies & Integrations has at least one IT spec
>
> **Ask triggers:**
> - A test seems to need a real third-party API call → ask whether sandbox / fixture / mock server is preferred (and what the project standard is)
> - A test would take minutes to run → ask whether it should be moved to a slower CI stage (nightly)
>
> **Remove this block before delivering the filled doc.**

> Integration tests wire real components together — typically service + repository against a real (test) database, or service + external client against a sandbox/mock server.
> These tests are slower. Run them in CI but not necessarily on every save.

**ID format:** `IT-NN`

**Database strategy:** [Test DB spun up per run / Shared test DB with transaction rollback per test / In-memory DB / Docker Compose]

**External services strategy:** [Sandbox environments / Recorded HTTP fixtures (e.g., VCR) / Mock server (e.g., WireMock) / Real calls gated behind env flag]

---

### IT-01 · [ServiceName] + [RepositoryName / ExternalClient]

**Module:** M-[##]
**File:** `[path/to/test/file]`
**What it tests:** [What real interaction is being verified — e.g., "UserService correctly persists and retrieves users via UserRepository against a test PostgreSQL instance"]

**Setup:**
- [DB seeded with: describe initial state]
- [External client configured as: sandbox / mock]

---

#### Test Cases

| ID | Name | Preconditions | Input | Expected Outcome | Teardown | Status |
|----|------|--------------|-------|-----------------|----------|--------|
| IT-01-01 | [name] happy path | [DB state / dependencies] | [Input] | [What's true after — rows created, response shape, etc.] | [Rollback / cleanup] | 🔲 |
| IT-01-02 | [name] DB constraint violation | [Duplicate row seeded] | [Input that would duplicate] | Throws `[ErrorName]` / returns 409 | Rollback | 🔲 |
| IT-01-03 | [name] transaction rollback on error | [Partial state seeded] | [Input triggering partial failure] | [No partial writes — atomic rollback confirmed] | Rollback | 🔲 |

---

#### Detailed Test Case Blocks

##### IT-01-03 · [name] transaction rollback on error

**Preconditions:**
```
[Describe DB state before test runs]
users table: empty
projects table: empty
```

**Input:**
```
[ServiceName].[method]({ ... })
```

**Expected behavior:**
- [Step 1 of the operation succeeds: e.g., user row inserted]
- [Step 2 fails: e.g., project insert throws FK violation]
- Transaction rolls back — user row is NOT present after failure
- Throws `[ErrorName]`

**Assertions:**
- `users` table row count = [expected count] after rollback
- `projects` table row count = [expected count] after rollback
- Error type is `[ErrorName]`

**Why this matters:** Confirms atomicity — partial writes don't corrupt state.

---

### IT-02 · [ServiceName] + [ExternalClient]

*(Continue pattern for IT-03, IT-04, etc.)*

---

## Test Case Specs — API Contract

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Produce one Test Case Spec block per endpoint in `[AppName]_API_Contract.md`. Every endpoint contract has exactly one AC-NN suite. Every error row in the endpoint contract becomes a test case in this suite.
>
> **A complete API Contract spec block covers:**
> - Header: `AC-NN · [HTTP_METHOD] [path]`
> - Module reference
> - Endpoint (full path including version)
> - Auth requirement (matches API Contract per-endpoint Auth value)
> - API Contract ref (link back to the source endpoint contract)
> - Test Cases table — one row for happy path + one row per error scenario from the API Contract
> - Detailed Test Case Blocks for non-obvious cases (request/response examples)
>
> **Coverage discipline for API Contract tests (mandatory — Gate 2 enforces this):**
> - Every endpoint in API Contract has exactly one AC-NN suite — no endpoint missing
> - Happy path test case (200/201) for every endpoint
> - 400 VALIDATION_ERROR test case for every endpoint that takes input (missing required field + invalid value)
> - 401 UNAUTHORIZED test case for every endpoint marked Auth: Required
> - 403 FORBIDDEN test case for every endpoint with role/permission requirements
> - 404 NOT_FOUND test case for every endpoint with a path parameter (`:id`)
> - 409 CONFLICT test case for every endpoint that can fail uniqueness/state-machine guards
> - 422 BUSINESS_RULE_VIOLATION test case for every endpoint with business-rule guards
>
> **Incomplete looks like:**
> - An endpoint in API Contract with no AC-NN suite here
> - An AC suite with only a happy-path case (no error coverage)
> - A test case body that says "with valid input" without showing the actual JSON
> - An expected response that says "the resource" without the field-level shape
>
> **Cross-reference checklist:**
> - AC-NN ID is unique
> - Endpoint path matches the API Contract entry exactly (including version prefix)
> - Auth value matches API Contract per-endpoint Auth value
> - Every error case here matches an error row in the API Contract endpoint
> - Expected response shape matches the API Contract response schema
>
> **Ask triggers:**
> - An endpoint contract has an error row that doesn't fit a standard HTTP code → ask before mapping
> - An endpoint has state-machine guards from Tech Spec but no 409 test → ask before skipping
>
> **Remove this block before delivering the filled doc.**

> Tests the HTTP interface. Sends real HTTP requests to a running test server and asserts on response status, headers, and body.
> These are the "does the API behave as documented in `[AppName]_API_Contract.md`?" tests.

**ID format:** `AC-NN`

**Test server strategy:** [In-process test server / Docker Compose / Dedicated test environment]

**Auth strategy:** [Test JWT with known payload / Seeded test user / API key for test env]

**DB strategy:** [Test DB, transactions rolled back per test / DB seeded per suite and cleaned up]

---

### AC-01 · POST /api/v1/[resource]

**Module:** M-[##]
**Endpoint:** `POST /api/v1/[resource]`
**Auth:** Required
**API Contract ref:** `[AppName]_API_Contract.md` → Resource: [Name]

---

#### Test Cases

| ID | Name | Auth | Request Body | Expected Status | Expected Response | Status |
|----|------|------|-------------|----------------|------------------|--------|
| AC-01-01 | Happy path — creates resource | Valid token | `{ "name": "Test", ... }` | 201 | Resource object with `id`, `created_at` | 🔲 |
| AC-01-02 | Missing required field | Valid token | `{ }` (no name) | 400 | `VALIDATION_ERROR`, details include `name` | 🔲 |
| AC-01-03 | Field exceeds max length | Valid token | `{ "name": "[256 chars]" }` | 400 | `VALIDATION_ERROR`, details include `name` | 🔲 |
| AC-01-04 | Invalid enum value | Valid token | `{ "status": "bogus" }` | 400 | `VALIDATION_ERROR`, details include `status` | 🔲 |
| AC-01-05 | No auth token | None | Valid body | 401 | `UNAUTHORIZED` | 🔲 |
| AC-01-06 | Expired token | Expired JWT | Valid body | 401 | `UNAUTHORIZED` | 🔲 |
| AC-01-07 | Duplicate unique field | Valid token | `{ "name": "[name already exists]" }` | 409 | `CONFLICT` | 🔲 |

---

#### Detailed Test Case Blocks

> Use for non-obvious cases. Happy paths rarely need a detail block — error paths often do.

##### AC-01-02 · Missing required field — 400

**Request:**
```http
POST /api/v1/[resource]
Authorization: Bearer [valid-test-token]
Content-Type: application/json

{}
```

**Expected response:**
```json
{
  "status": 400,
  "error": "VALIDATION_ERROR",
  "message": "[user-friendly message]",
  "details": [
    { "field": "name", "issue": "is required" }
  ]
}
```

**Assertions:**
- Status code is `400`
- `error` is `"VALIDATION_ERROR"`
- `details` array contains an entry with `field: "name"`
- No resource is created in DB

---

### AC-02 · GET /api/v1/[resource]/:id

**Module:** M-[##]
**Endpoint:** `GET /api/v1/[resource]/:id`
**Auth:** Required
**API Contract ref:** [Section]

#### Test Cases

| ID | Name | Auth | Path | Expected Status | Expected Response | Status |
|----|------|------|------|----------------|------------------|--------|
| AC-02-01 | Happy path — returns resource | Valid token, owns resource | Valid ID | 200 | Full resource object | 🔲 |
| AC-02-02 | ID not found | Valid token | Non-existent ID | 404 | `NOT_FOUND` | 🔲 |
| AC-02-03 | Resource belongs to different user | Valid token, different user | Valid ID | 404 | `NOT_FOUND` (not 403 — don't confirm existence) | 🔲 |
| AC-02-04 | No auth token | None | Valid ID | 401 | `UNAUTHORIZED` | 🔲 |

*(Continue pattern for AC-03, AC-04, etc. — one suite per endpoint)*

---

## Test Case Specs — E2E

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Produce one Test Case Spec block per critical user workflow from `[AppName]_Product_Design_Doc.md` → User Workflows. E2E tests are expensive — cover only critical paths.
>
> **A complete E2E spec block covers:**
> - Header: `E2E-NN · [Workflow Name]`
> - Workflow ref (link to PDD User Workflow)
> - Module reference(s)
> - Priority (High / Med / Low — only High and critical Med get E2E coverage)
> - Estimated runtime
> - Preconditions (what's true before the test runs)
> - Steps table (Actor / Action / Expected Result per step)
> - Teardown
> - "What it does NOT test" — explicitly scope the test to prevent bloat
> - Error Path Variants table (most common deviations from happy path)
>
> **Coverage discipline for E2E tests (mandatory — Gate 2 enforces this):**
> - Every PDD User Workflow marked critical OR linked to a High-priority Module gets an E2E spec
> - Workflows that are sub-paths of others (e.g., login is a precondition for most workflows) don't need their own E2E — covered by inclusion
> - Error path variants cover at minimum: wrong credentials, session expiry mid-flow, network failure mid-flow
> - Total E2E suite count should be a small fraction of total Unit suite count (test pyramid)
>
> **Incomplete looks like:**
> - A critical PDD Workflow with no E2E spec
> - Steps without "Expected Result" populated
> - "Teardown: cleanup" without specifying what
> - Missing "What it does NOT test" section (omission causes scope creep over time)
>
> **Cross-reference checklist:**
> - E2E-NN ID is unique
> - Workflow ref points to an actual PDD User Workflow
> - Module IDs match Coverage Plan by Module
> - Preconditions reference seed data from Test Data & Fixtures
>
> **Ask triggers:**
> - A workflow has 12+ steps → ask whether to split into multiple E2E suites or whether to drop sub-steps not user-visible
> - Estimated runtime exceeds 2 minutes → ask whether the suite is doing too much
>
> **Remove this block before delivering the filled doc.**

> End-to-end tests drive the full system — real browser (or API client), real server, real database.
> Cover critical user paths only. These are the most expensive to write and maintain.

**ID format:** `E2E-NN`

**Tool:** [Playwright / Cypress / Selenium / Detox (mobile) / Supertest (API-only E2E) / etc.]

**Environment:** [Dedicated E2E environment / Staging / Local Docker Compose]

**Data strategy:** [Seed before each test / Seed before suite + cleanup / Fixed test accounts]

**Parallelism:** [Run in parallel / Serial — and why]

**When E2E runs:** [On PR / Nightly / Pre-deploy only / Manual trigger]

---

### E2E-01 · [Workflow Name]

**Workflow ref:** `[AppName]_Product_Design_Doc.md` → Workflow: [Name]
**Module(s):** M-[##], M-[##]
**Priority:** High / Med / Low
**Estimated runtime:** [Xm Xs]

**Preconditions:**
- [e.g., User account exists with email `test@example.com` and password `hunter2`]
- [e.g., At least one project seeded for this user]

**Steps:**

| Step | Actor | Action | Expected Result |
|------|-------|--------|----------------|
| 1 | User | [e.g., Navigates to `/login`] | [Login screen renders] |
| 2 | User | [e.g., Enters email and password, submits] | [Redirected to `/dashboard`] |
| 3 | System | [e.g., Dashboard loads] | [User's projects are visible] |
| 4 | User | [e.g., Clicks "New Project", fills in name, submits] | [New project appears in list] |
| 5 | System | [e.g., New project persisted] | [Refresh shows project still present] |

**Teardown:**
- [e.g., Delete created project, reset user state]

**What it does NOT test:** [Explicitly note what's out of scope for this test — prevents bloat]

---

#### Error Path Variants

| ID | Variant | Change from Happy Path | Expected Outcome |
|----|---------|----------------------|-----------------|
| E2E-01-E1 | [e.g., Wrong password on login] | [Enter invalid password] | [Error message shown, no redirect] |
| E2E-01-E2 | [e.g., Session expires mid-workflow] | [Expire token between step 3 and 4] | [Redirected to login, workflow state preserved if applicable] |

---

### E2E-02 · [Workflow Name]

*(Continue pattern for E2E-03, E2E-04, etc.)*

---

## Test Case Specs — Other

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Cover the test types that aren't unit / integration / API / E2E. Add a subsection per type used. Mark "Not used in this project" for types skipped — do not delete the subsection, the reference is useful.
>
> **A complete Other section:**
> - Has one subsection per type from Test Type Reference that's used this project
> - Each subsection declares ID format, tool, when run, and lists test cases
> - Snapshot subsection lists components whose render is snapshotted (matches Component Registry Shared/Layout entries)
> - Performance subsection lists endpoints with latency/throughput targets (matches API Contract critical endpoints)
> - Security subsection covers the standard 8 items (auth bypass, JWT forge, expired tokens, IDOR, SQLi, XSS, brute-force, sensitive data exposure, dep CVEs)
> - Accessibility subsection lists every Screen from UI/UX with automated + manual coverage flags
> - Smoke subsection lists the post-deploy health checks
>
> **Ask triggers:**
> - Project skips Security tests entirely → ask before accepting (most projects need at least the standard 8)
> - Project skips Accessibility tests entirely → ask before accepting; WCAG 2.1 AA is the default per Context doc
>
> **Cross-reference checklist:**
> - Snapshot tests reference C-XX entries from Component/Service Map
> - Performance tests reference endpoints from API Contract
> - Security tests cover auth scenarios from Tech Spec → Authentication & Authorization
> - Accessibility tests reference Screens from UI/UX
> - Smoke tests reference the `/health` endpoint declared in Tech Spec → Deployment & Environments
>
> **Remove this block before delivering the filled doc.**

> Use this section for test types that don't fit the categories above. Add a subsection per type.
> Mark "Not used in this project" for types skipped — do not delete the subsection.

---

### Snapshot Tests

**ID format:** `SNAP-NN`
**Tool:** [Jest + React Testing Library / Storybook / etc.]
**What gets snapshotted:** [Shared UI components, layout components — not page-level composites]
**Update policy:** [Snapshots updated only by intentional design changes — never "just to make CI pass"]

| ID | Component | Variants Tested | Status |
|----|-----------|----------------|--------|
| SNAP-01 | [ComponentName] | Default, disabled, error state | 🔲 |
| SNAP-02 | [ComponentName] | All size variants | 🔲 |

---

### Performance Tests

**ID format:** `PERF-NN`
**Tool:** [k6 / Locust / JMeter / Artillery / etc.]
**When run:** [Before launch / After major changes / Nightly on staging]
**Baseline established:** [Date or "Not yet"]

| ID | Endpoint / Operation | Load Profile | Latency Target (p95) | Throughput Target | Status |
|----|---------------------|-------------|---------------------|------------------|--------|
| PERF-01 | `GET /api/v1/[resource]` | [X concurrent users, Y rps] | < [Xms] | [Y rps] | 🔲 |
| PERF-02 | `POST /api/v1/[resource]` | [X concurrent users] | < [Xms] | — | 🔲 |

---

### Security Tests

**ID format:** `SEC-NN`
**Tool:** [OWASP ZAP / Burp Suite / manual / npm audit / Snyk / etc.]
**When run:** [Pre-launch / Quarterly / On auth changes]

| ID | Area | What's Tested | Method | Status |
|----|------|--------------|--------|--------|
| SEC-01 | Auth | JWT cannot be forged with wrong secret | Unit / API | 🔲 |
| SEC-02 | Auth | Expired tokens are rejected | API | 🔲 |
| SEC-03 | Auth | User cannot access another user's resource (IDOR) | API | 🔲 |
| SEC-04 | Input | SQL injection attempts return 400, not 500 | API | 🔲 |
| SEC-05 | Input | XSS payloads in text fields are stored as plain text, not executed | E2E / API | 🔲 |
| SEC-06 | Auth | Brute-force login is rate-limited | API | 🔲 |
| SEC-07 | Data | Sensitive fields (password_hash, tokens) never appear in API responses | API | 🔲 |
| SEC-08 | Dependencies | No known high/critical CVEs in dependencies | `npm audit` / Snyk | 🔲 |

---

### Accessibility Tests

**ID format:** `A11Y-NN`
**Standard:** WCAG 2.1 AA minimum
**Tool (automated):** [axe-core / Lighthouse / Pa11y]
**Tool (manual):** [Screen reader — VoiceOver / NVDA / JAWS]
**When run:** [Per PR (automated) / Per release (manual)]

| ID | Screen / Component | Automated | Manual | Status |
|----|-------------------|-----------|--------|--------|
| A11Y-01 | [Screen/Component name] | ✅ axe-core in CI | ⏳ Manual | 🔲 |
| A11Y-02 | [Screen/Component name] | ✅ | — | 🔲 |

**Manual test checklist (per screen):**
- [ ] All interactive elements reachable via Tab in logical order
- [ ] Focus ring visible at all times
- [ ] Screen reader announces page title, headings, and form labels correctly
- [ ] Error messages are announced on input blur and form submit
- [ ] All images have meaningful `alt` text (or `alt=""` for decorative images)
- [ ] Color is not the sole indicator of state
- [ ] Animations respect `prefers-reduced-motion`

---

### Smoke Tests

**ID format:** `SMOKE-NN`
**When run:** After every deploy to staging and production
**Failure action:** [Block deploy / Alert on-call / Auto-rollback]
**Target runtime:** [Under X minutes — smoke tests must be fast]

| ID | What's Checked | Method | Expected Result |
|----|---------------|--------|----------------|
| SMOKE-01 | App server is up | `GET /health` | 200 OK |
| SMOKE-02 | DB connection is healthy | `GET /health` (checks DB) | 200, `db: "ok"` |
| SMOKE-03 | Auth endpoint responds | `POST /api/v1/auth/login` with test credentials | 200, token returned |
| SMOKE-04 | Primary read endpoint responds | `GET /api/v1/[resource]` with auth | 200, data array |
| SMOKE-05 | Static assets load | [Home page URL] | 200, no missing assets |

---

## CI/CD Integration

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** CI/CD Integration is filled AFTER Test Case Specs are written. The pipeline declares which test types run when — without knowing what test specs exist, the pipeline can't reference them accurately.
>
> **Your job:** Map every test type to its place in the pipeline. Every test type from Test Type Reference (that's used) has a stage. Every stage has a trigger, target duration, and failure behavior.
>
> **A complete CI/CD Integration section covers:**
> - Pipeline Stages table — every stage from pre-commit through post-deploy
> - Test Parallelism table — which test types can run in parallel
> - Environment Variables Required in CI — every secret or config the test suite needs
>
> **Incomplete looks like:**
> - A test type used in the project but missing from Pipeline Stages
> - A stage with no Target Duration (can't enforce timing without it)
> - "Environment variables required in CI: standard" without listing them
>
> **Cross-reference checklist:**
> - Every test type marked ✅ Needed in Coverage Plan appears in at least one stage
> - Pipeline stages align with Deployment Config CI/CD Pipeline (cross-reference Deployment Config when it's written)
> - Environment variables match Tech Spec → Environment Variables for test-prefixed vars
>
> **Ask triggers:**
> - Project has no CI pipeline declared yet → flag to the human; CI integration is not optional for AI-driven coding
> - A stage's failure behavior is undefined → ask before assuming "block deploy"
>
> **Remove this block before delivering the filled doc.**

> How tests plug into the pipeline. Every test type must have a defined place in the pipeline — or an explicit decision that it doesn't run in CI.

### Pipeline Stages

| Stage | Trigger | Tests Run | Fail Behavior | Target Duration |
|-------|---------|-----------|--------------|----------------|
| Pre-commit | `git commit` hook | [Unit only / Lint only] | Block commit | < 30s |
| PR check | PR opened or updated | Unit, Integration, API Contract | Block merge | < [X min] |
| Merge to main | Push to `main` | Unit, Integration, API Contract, Snapshot | Block deploy | < [X min] |
| Pre-deploy staging | Before staging deploy | All CI tests + Smoke | Block deploy | < [X min] |
| Post-deploy staging | After staging deploy | Smoke, E2E | Alert — does not auto-rollback | < [X min] |
| Pre-deploy production | Before production deploy | Smoke | Block deploy | < [X min] |
| Post-deploy production | After production deploy | Smoke | Alert on-call | < [X min] |
| Scheduled | Nightly on staging | Full suite including perf, security, a11y | Alert team | — |

### Test Parallelism

| Test Type | Parallel? | Notes |
|-----------|-----------|-------|
| Unit | Yes | Fully parallel — no shared state |
| Integration | Partial | Parallel per service; DB tests use transaction isolation |
| API Contract | Yes | Each test gets isolated DB state via transactions |
| E2E | [Yes / No] | [If yes — how is test isolation managed across workers?] |

### Environment Variables Required in CI

| Variable | Purpose | Where Set |
|----------|---------|-----------|
| `TEST_DATABASE_URL` | Test DB connection | CI secrets |
| `TEST_JWT_SECRET` | Known secret for test tokens | CI secrets |
| `[VAR]` | [Purpose] | [CI secrets / env config] |

---

## Coverage Targets

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Coverage Targets is filled AFTER Test Case Specs are written. Targets must reflect what was actually written, not aspirational numbers.
>
> **Your job:** Declare the coverage floor below which CI blocks. Coverage is enforced per-layer with both line/branch percentages AND artifact coverage. Percentage alone is misleading — artifact coverage is the real check.
>
> **A complete Coverage Targets section covers:**
> - Layer-by-layer coverage targets (Backend services, Backend utilities, Frontend components, API endpoints, Critical workflows)
> - "Current" column filled with the actual count/percentage at Gate 2 (blank until then)
> - "Enforced in CI?" column declaring whether CI blocks below the target
> - "What Coverage Doesn't Catch" callout — explicit limits so there's no false confidence
>
> **Incomplete looks like:**
> - "Backend services: 80% line coverage" with no artifact coverage rule
> - A target with no "Enforced in CI?" decision
> - Missing the "What Coverage Doesn't Catch" section
>
> **Coverage discipline:**
> - 100% artifact coverage is mandatory for critical layers (endpoints, services, workflows) — every artifact has at least one test
> - Percentage targets are floors, not ceilings
> - Targets reflect the test pyramid (unit highest, integration medium, E2E targeted by workflow count)
>
> **Ask triggers:**
> - A target is set below 50% line coverage → ask before accepting; that's not a meaningful target
> - 100% target for any percentage → push back; that creates pressure to write meaningless tests for last-mile coverage
>
> **Remove this block before delivering the filled doc.**

> Targets are minimums. CI enforces them — a PR that drops below target is blocked.

| Layer | Coverage Type | Target | Current | Enforced in CI? |
|-------|--------------|--------|---------|----------------|
| Backend services | Line / Branch | [e.g., 80%] | — | [Yes / No] |
| Backend utilities | Line | [e.g., 90%] | — | [Yes / No] |
| Frontend components (unit) | Line | [e.g., 70%] | — | [Yes / No] |
| API endpoints | Artifact coverage | 100% of endpoints have ≥ 1 AC test | — | [Yes / No] |
| Backend services | Artifact coverage | 100% of services have ≥ 1 UT suite | — | [Yes / No] |
| Critical user workflows | E2E coverage | [N] workflows covered (one E2E per critical PDD Workflow) | — | [Yes / No] |

> ⚠️ **Coverage percentage is a floor, not a goal. 80% coverage with meaningful assertions beats 100% coverage with assertions that only check "function was called."**

### What Coverage Doesn't Catch

Document explicitly so there's no false confidence:

- Race conditions
- Infrastructure failures (network partition, disk full)
- Data migration correctness
- Third-party API behavioral changes
- UI rendering bugs not covered by automated tests
- Performance regression (covered separately by PERF tests, not unit coverage)

---

## Open Questions

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Track any open question that blocks a Test Case Spec or coverage decision. One row per question. Resolve before Gate 2.
>
> **A complete Open Questions table:**
> - Has zero unresolved rows when this doc reaches Gate 2
> - Each row names the specific test ID, Module, or coverage gap affected
> - Each row has an owner (Ryan / Claude / TBD)
> - Each row has a "Needed By" — typically a phase or specific date
>
> **Remove this block before delivering the filled doc.**

| Question | Affects | Priority | Owner | Needed By | Resolution |
|----------|---------|----------|-------|-----------|------------|
| [Question] | [Test type / Module / Test ID] | High / Med / Low | [Ryan / Claude / TBD] | [Phase or date] | [Empty until resolved] |

---

## 🚦 Gate 2 — Full Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Why this gate matters:** This is the last gate before this doc is consumed by Deployment Config (CI/CD pipeline stages), Module Breakdown (Acceptance Criteria → Verification references test IDs), Build Decisions Log (Concern → Resolved by test ID), and direct coding agents writing test files. Errors here cascade into every downstream artifact.
>
> **Human sign-off is required.** Do not declare this doc Done without explicit human approval.
>
> **Gate procedure:**
> 1. Walk every checklist item. For each, open the relevant section AND the upstream doc (Module Breakdown, Component/Service Map, API Contract, PDD) and verify — do not check from memory.
> 2. Coverage completeness is the headline check. Drift between this doc and upstream docs is expected; resolve it before declaring Done.
> 3. Verify the test pyramid shape — unit count should be largest, E2E count smallest. Inverted pyramid is a fail.
> 4. If any check fails, flag to the human and resolve before declaring Done.
>
> **Remove this instruction block before delivering. Keep the checklist and sign-off line.**

### Completeness

- [ ] Overview is filled — app, source docs, frameworks per layer, total test counts
- [ ] Testing Philosophy & Rules locked (Gate 1 signed off)
- [ ] Test Type Reference locked (Gate 1 signed off)
- [ ] Coverage Plan by Module locked (Gate 1 signed off)
- [ ] Coverage Plan by Feature / Workflow locked (Gate 1 signed off)
- [ ] Test Data & Fixtures has seed data, fixed values, and factories declared
- [ ] Every Module marked ✅ Needed for Unit has at least one UT-NN suite
- [ ] Every Module marked ✅ Needed for Integration has at least one IT-NN suite
- [ ] Every endpoint in API Contract has exactly one AC-NN suite
- [ ] Every critical workflow in PDD has at least one E2E-NN suite
- [ ] "Other" test types are populated for every type marked ✅ Needed in Coverage Plan
- [ ] CI/CD Integration declares every test type's pipeline stage
- [ ] Coverage Targets declared with Current values filled and CI enforcement decided
- [ ] Open Questions has zero blocking entries

### Coverage Completeness (Artifact-Level — the headline check)

- [ ] **Every M-XX in Module Breakdown** has at least one test ID referencing it across UT / IT / AC / E2E specs
- [ ] **Every C-XX with logic in Component/Service Map** has at least one UT-NN test case
- [ ] **Every S-XX in Component/Service Map** has at least one UT-NN suite covering its public methods
- [ ] **Every Service Detail Block's Transactional Operation** has at least one IT-NN test verifying atomicity (success + rollback)
- [ ] **Every endpoint in API Contract** has exactly one AC-NN suite (no missing endpoint, no duplicate suite)
- [ ] **Every error row in every API Contract endpoint** has a corresponding test case in the AC suite
- [ ] **Every named error in Component/Service Map → Error Catalog** has at least one UT or IT test case verifying it's thrown
- [ ] **Every state-machine transition in Tech Spec → State Machines** has a UT test for the transition AND a UT test for the guard failure
- [ ] **Every external client in Tech Spec → Dependencies & Integrations** has at least one IT-NN suite covering success + error + timeout
- [ ] **Every critical PDD User Workflow** has at least one E2E-NN suite
- [ ] **Every Screen in UI/UX** has at least one A11Y-NN entry (automated minimum)

### Three-Way Consistency (Coverage Plan ↔ Spec Table Row ↔ Detailed Block)

> Every test is described at up to three fidelity tiers in this doc: (1) declared in **Coverage Plan by Module** as a needed test type for a Module; (2) listed in a **Test Case Spec table row** with full ID, name, input, expected outcome; (3) for non-obvious cases, expanded in a **Detailed Test Case Block** with setup, assertions, and rationale. All three tiers must agree on Module, test type, ID, and name. Drift between tiers means the spec table or detailed block has silently diverged from what was planned.

- [ ] For every Module row in Coverage Plan by Module marked ✅ Needed for Unit: opened the Unit Test Case Specs section and confirmed at least one UT-NN suite has Module = M-XX. Reverse direction too: every UT-NN suite's Module field maps to a Module row marked ✅ Needed for Unit (no orphan specs).
- [ ] For every Module row marked ✅ Needed for Integration: at least one IT-NN suite has Module = M-XX. Reverse direction: every IT-NN's Module maps to a row marked ✅ Needed for Integration.
- [ ] For every Module row marked ✅ Needed for API Contract: every endpoint owned by that Module (per API Contract) has its AC-NN suite present, and each AC-NN's Module = M-XX. Reverse direction: every AC-NN's Module maps to a row marked ✅ Needed for API Contract.
- [ ] For every Module row marked ✅ Needed for E2E: at least one E2E-NN suite references a Workflow owned by that Module. Reverse: every E2E-NN suite's Module(s) map to rows marked ✅ Needed for E2E.
- [ ] For every Detailed Test Case Block present (UT, IT, AC, or E2E): the block's case ID (e.g., `UT-01-02`) and case name exactly match a row in the parent suite's Test Cases table. No detailed block exists without a matching table row.
- [ ] For every 🚫 Skip in Coverage Plan by Module: confirmed no test spec of that type exists for that Module here (spec presence contradicts the skip).

### Test Pyramid Sanity Check

- [ ] Total UT-NN suites > Total IT-NN suites > Total E2E-NN suites
- [ ] No E2E suite duplicates a Unit test (E2E covers the full path; unit covers the logic — not both for the same thing)
- [ ] Total E2E suites ≤ Total critical PDD Workflows (no scope creep)

### Cross-Doc Consistency

- [ ] **Module Breakdown ↔ Test ID references** — for every Module Detail Block's Acceptance Criteria → Verification section, opened Module Breakdown and confirmed the test IDs listed exist here.
- [ ] **API Contract ↔ AC suites** — every endpoint here exists in API Contract; every endpoint in API Contract has an AC suite here.
- [ ] **Component/Service Map ↔ UT/IT suites** — every C-XX with logic has UT coverage; every S-XX has UT + IT coverage.
- [ ] **PDD ↔ E2E suites** — every E2E Workflow ref points to a real PDD User Workflow.
- [ ] **Tech Spec ↔ CI/CD Integration** — pipeline stages match Tech Spec → Deployment & Environments and (when written) Deployment Config CI/CD Pipeline.
- [ ] **Sample Data ↔ Test Data & Fixtures** — seed data is consistent with `[AppName]_Sample_Data.md` (no competing definitions of "test users").

### Cleanup Verification

- [ ] Searched the file for `🤖` — zero hits
- [ ] Searched the file for `❓ AGENT PAUSE` — zero hits
- [ ] Searched the file for "Remove this block" — zero hits
- [ ] Every `🚦 GATE` block contains only its checklist and sign-off line — no agent prose

### Sign-Off

- [ ] **Human sign-off** — Testing Strategy is complete, internally consistent, cross-doc consistent, achieves artifact-level coverage, and ready to drive CI/CD pipeline configuration and downstream coding agents.

Signed: _____________________ Date: ___________
