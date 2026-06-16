# Component / Service Layer Map: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized
> **Fill order:** Coding-phase doc. Begin after Module Breakdown is signed off. Fills in parallel with API Contract, Database Migration Checklist, and Testing Strategy — but Service Contracts here should reference final API Contract endpoint shapes, so completing API Contract first reduces rework.
>
> **Source docs (every component and service traces upstream to these):**
> - `[AppName]_Module_Breakdown.md` — Module Registry and Detail Blocks. **Primary source.** Every Module Detail Block's "Domain / Class Entities" column names the components and services that will live here; this doc gives them their `C-XX` / `S-XX` IDs and contracts. Bidirectional link — every C-XX / S-XX here references the Module ID that owns it.
> - `[AppName]_UI_UX.md` — Screens (every Page component traces to a Screen) and Shared Component Inventory (every Shared component traces to an entry there). Component types (Page / Layout / Feature / Shared) align with UI/UX's structure.
> - `[AppName]_API_Contract.md` — Endpoint Contracts. Every service method that calls an API maps to a specific endpoint here; every DTO that crosses the API boundary matches a request/response shape there. If API Contract isn't done yet, services can be drafted but Service Contracts can't be finalized until API Contract is locked.
> - `[AppName]_Technical_Spec.md` — Architecture Overview (drives the frontend/backend split and DI approach), Tech Stack (drives file extensions, frameworks, naming conventions), Authentication & Authorization (drives the AuthService design and permission-check pattern), Error Handling (drives the Error Catalog).
> - `[AppName]_DB_Schema.md` — Entities (drives Repository naming and Service domain ownership — one service per domain, one repository per entity).
>
> **Downstream docs that consume this one (write to feed them):**
> - `[AppName]_Testing_Strategy.md` — Every `C-XX` becomes one or more component tests (`UT-XX` for logic, snapshot tests for rendering); every `S-XX` becomes one or more service tests (`UT-XX` for methods, `IT-XX` for integrations). Test IDs reference the C-XX / S-XX they cover. **Missing a Detail Block here = missing test coverage there.**
> - `[AppName]_Build_Decisions_Log.md` — BD entries reference C-XX or S-XX when a workaround / deviation affects a specific component or service.
> - `[AppName]_Mid_Build_Review.md` — Drift checks compare built code against C-XX and S-XX specs here.
> - `[AppName]_Pre_Build_Validation_Checklist.md` — Verifies every endpoint in API Contract maps to a hook/service in Component/Service Map's component side AND a service in Component/Service Map's backend side. Verifies every Module Detail Block's named class entities have C-XX or S-XX IDs here.
> - Frontend & backend coding agents — read this doc directly. **The downstream-precision standard for this doc is: a frontend agent can scaffold a complete component (props, state, hooks, error handling) from one C-XX Detail Block; a backend agent can scaffold a complete service (signature, DI, transactional boundaries, errors thrown) from one S-XX Detail Block. Neither agent should need to re-read Tech Spec or Schema.**
>
> **Agent role:** Translate Module Breakdown's named class entities into a complete inventory of components and services with full contracts, dependencies, and error surfaces. The human is the designer; the agent enforces the architectural rules (thin components, single-responsibility services, named errors, DTOs at boundaries) and produces the per-block precision a coding agent can build from.
>
> **The three rules while filling this doc:**
> 1. Everything written traces to Module Breakdown + UI/UX + API Contract + Tech Spec + Schema + confirmed human input. No invented components. No invented services. No invented methods.
> 2. If a component's responsibility, a service's transactional scope, or a DTO's field shape is unclear, stop and ask. Do not write a Detail Block on a guess — wrong contracts produce wrong code at scale.
> 3. Output must be specific enough that a frontend agent can scaffold a complete component from one C-XX block, and a backend agent can scaffold a complete service from one S-XX block, without re-reading any upstream doc.
>
> **Two failure modes drive most of the design here — both are addressed by gates:**
> - **Architectural drift.** A component reaches into a service's internals, or makes a raw API call, or formats a business string. A service returns a DB entity instead of a DTO, or splits a logical operation across two calls and hopes the caller wraps a transaction. Once a few components or services break the rules, the rules are dead. The Design Rules section + Gate 1 exist to lock the rules before any Detail Block is written.
> - **Service surface mismatch.** The Service Registry says one method signature, the Service Contract says another, the DTO Registry shows a third shape. Frontend code is generated from one, backend from another, and runtime explodes at integration time. Gate 2 enforces three-way consistency across Registry / Contract / DTO.
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
> 2. **Design Rules** — must come first; every component and service is structured against these
> 3. **File & Naming Conventions** — must come before Registries; every ID, filename, and folder reference assumes these conventions
> 4. **🚦 Gate 1 — Foundation Lock** (Design Rules + Naming Conventions sign-off before Registries begin)
> 5. **Frontend Component Registry** — must come before Component Detail Blocks
> 6. **Backend Service Registry** — must come before Service Detail Blocks (can fill in parallel with Component Registry — different domains)
> 7. **DTOs (Registry + Detail Blocks)** — must come before Service Contracts (contracts reference DTOs by name)
> 8. **Component Detail Blocks** — must come after Component Registry; can fill in parallel with Service Contracts (different teams)
> 9. **Service Detail Blocks** — must come after Service Registry; sets up Service Contracts
> 10. **Service Contracts** — must come after Service Detail Blocks (a contract is the formal restatement of a Detail Block's method table)
> 11. **Error Catalog** — can be filled concurrently with Service Detail Blocks; every service method that throws references an error from here
> 12. **Dependency Map** — must come after Component Registry, Service Registry, and Service Contracts (maps every relationship those declare)
> 13. **🚦 Gate 2 — Full Sign-Off** (final gate before downstream docs consume this)

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Overview | 🔲 Not Started | — | — |
| Design Rules | 🔲 Not Started | — | Strict ordering — foundation lock |
| File & Naming Conventions | 🔲 Not Started | — | Strict ordering — foundation lock |
| 🚦 Gate 1 — Foundation Lock | 🔲 Not Started | — | Human sign-off required before Registries |
| Frontend Component Registry | 🔲 Not Started | — | Strict ordering — fill before Component Detail Blocks |
| Backend Service Registry | 🔲 Not Started | — | Strict ordering — fill before Service Detail Blocks |
| DTOs | 🔲 Not Started | — | Strict ordering — fill before Service Contracts |
| Component Detail Blocks | 🔲 Not Started | — | One block per non-trivial component |
| Service Detail Blocks | 🔲 Not Started | — | One block per service |
| Service Contracts | 🔲 Not Started | — | Strict ordering — fill after Service Detail Blocks |
| Error Catalog | 🔲 Not Started | — | Concurrent with Service Detail Blocks |
| Dependency Map | 🔲 Not Started | — | Strict ordering — fill last among content sections |
| Open Questions | 🔲 Not Started | — | Populate as they surface |
| 🚦 Gate 2 — Full Sign-Off | 🔲 Not Started | — | Final gate before downstream docs |

**Coding Status values:** 🔲 Not Started / 🔄 In Progress / 👀 In Review / ✅ Done / 🚫 Blocked

---

## Overview

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Anchor this doc to its sources. Anyone reading this should immediately understand the app's architecture shape (SPA + API, fullstack monolith, mobile + backend, etc.), the service pattern in use, the dependency injection approach, and which upstream docs are the sources of truth.
>
> **A complete Overview covers:**
> - App name
> - Architecture pattern (pulled from Tech Spec → Architecture Overview)
> - Frontend stack (framework, language, state management) — from Tech Spec → Tech Stack
> - Backend stack (framework, language, ORM) — from Tech Spec → Tech Stack
> - Service pattern (class-based, function modules, repository pattern, etc.)
> - Dependency injection approach (constructor injection, IoC container, manual wiring, framework-managed)
> - Total component count (matches Component Registry row count)
> - Total service count (matches Service Registry row count)
> - Explicit references to all five source-of-truth docs
>
> **Incomplete looks like:**
> - "React + Node" without specifying the framework versions
> - "Class-based services" without specifying how dependencies are injected
> - Component count or service count that doesn't match Registry rows
>
> **Ask triggers:**
> - Tech Spec → Architecture Overview describes a pattern this template doesn't anticipate (e.g., microservices, event-sourced backend) → ask before assuming a single Service Registry is the right shape
> - Tech Spec is silent on DI approach → ask the human; do not assume framework defaults
>
> **Remove this block before delivering the filled doc.**

- **App:** [App Name]
- **Architecture:** [e.g., React SPA + REST API / Next.js fullstack / mobile + backend — from Tech Spec → Architecture Overview]
- **Frontend stack:** [Framework + version, language, state management — from Tech Spec → Tech Stack]
- **Backend stack:** [Framework + version, language, ORM — from Tech Spec → Tech Stack]
- **Service pattern:** [e.g., Class-based services / Function modules / Repository pattern]
- **DI approach:** [e.g., Constructor injection / IoC container (which one) / Manual wiring / Framework-managed]
- **Total components:** [#] (must match Component Registry row count)
- **Total services:** [#] (must match Service Registry row count)
- **Source-of-truth docs:**
  - Module Breakdown: `[AppName]_Module_Breakdown.md`
  - UI/UX: `[AppName]_UI_UX.md`
  - API Contract: `[AppName]_API_Contract.md`
  - Tech Spec: `[AppName]_Technical_Spec.md`
  - DB Schema: `[AppName]_DB_Schema.md`

---

## Design Rules

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Design Rules are filled BEFORE Naming Conventions and BEFORE any Registry. These rules govern how every component and service is structured. Once locked at Gate 1, deviations require an explicit Build Decisions Log entry.
>
> **Your job:** Lock the architectural rules for components and services. These are not negotiable per-Detail-Block — they are the contract every Detail Block honors. Do not soften the rules to make a known violation easier to document.
>
> **A complete Design Rules section covers:**
> - Component rules — five default rules, all kept unless the human explicitly removes one with rationale
> - Service rules — six default rules, all kept unless the human explicitly removes one with rationale
> - Any project-specific rules appended at the end (e.g., "All async service methods return Result<T, E> instead of throwing")
> - Each rule has a clear description that a code reviewer can apply mechanically — "no business logic in components" is mechanically enforceable; "components should be clean" is not
>
> **Incomplete looks like:**
> - A rule listed without a description of what counts as a violation
> - A project-specific rule that contradicts a default rule without explicit rationale
> - "Thin components" rule but the human plans to put domain logic in Pages "because they're top-level" (the rule applies to all component types — flag this)
>
> **Ask triggers:**
> - Project's architecture genuinely doesn't fit the default rules (e.g., backend is event-sourced, service pattern doesn't apply) → ask before deleting rules
> - Tech Spec's Error Handling section conflicts with the "Throws named errors" rule (e.g., uses Result/Either pattern) → ask which pattern wins
>
> **Remove this block before delivering the filled doc.**

> These rules govern how components and services are structured across the entire codebase. Every deviation is a deliberate exception that must be documented in Build Decisions Log — not a habit.

### Component Rules (Frontend)

| Rule | Description |
|------|-------------|
| **Thin components** | Components handle rendering, user input, and local UI state only. No business logic. No raw DB/API calls. |
| **No domain logic in components** | Calculating totals, checking permissions, applying business rules — all of this lives in the service layer. |
| **No domain models in components** | Components receive DTOs, not raw DB entities or internal domain objects. |
| **No direct API calls from components** | Components call a service or hook that wraps the API. Never `fetch()` directly from a component. |
| **State scope** | Local UI state (loading, open/closed, hover) belongs in the component. Domain state (current user, cart contents) belongs in a store or context. |

### Service Rules (Backend)

| Rule | Description |
|------|-------------|
| **Single Responsibility** | Each service owns one domain or entity. No "God services." If a service handles 5 unrelated things, split it. |
| **Owns transactional boundaries** | A service method that spans multiple DB operations wraps them in a single transaction. Never split a logical operation across two service calls and hope the caller manages atomicity. |
| **Receives dependencies via injection** | Services declare what they need (repositories, external clients, other services) and receive them — never instantiate dependencies internally. |
| **Returns DTOs, not entities** | Services return data shapes safe for the caller. Internal domain models and ORM entities do not leak out. |
| **Throws named errors** | Services throw specific, meaningful errors (e.g., `UserNotFoundException`, `InsufficientStockError`). Never return `null` to signal failure. Never throw generic `Error("something went wrong")`. |
| **No presentation logic** | Services do not format strings for display, build HTML, or make decisions about what the UI shows. |

### Project-Specific Rules

> Append any project-specific rules below. Each rule needs a description and rationale (one sentence).

| Rule | Description | Rationale |
|------|-------------|-----------|
| — | — | — |

---

## File & Naming Conventions

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Naming Conventions are filled AFTER Design Rules and BEFORE any Registry. The IDs in Registry rows, the filenames in Detail Blocks, and the folder references in Dependency Map all depend on these conventions. If a human asks you to start the Registry before Naming Conventions is locked, push back:
>
> > "Naming Conventions defines the filename format every Registry row will use. Filling Registry first means rewriting filenames once conventions are set. Let's finish Naming Conventions — should take 5 minutes."
>
> **Your job:** Pin down where every artifact lives and what every artifact is called. Once locked at Gate 1, deviations are an explicit Build Decisions Log entry.
>
> **A complete File & Naming Conventions section covers:**
> - Folder structure for frontend (pages, layouts, features, components, dtos, stores)
> - Folder structure for backend (services, repositories, dtos, errors, controllers, middleware)
> - Naming rules for every artifact type (Page / Feature / Shared component, Service, Repository, Input DTO, Output DTO, Custom error, Hook)
> - A consistency rule statement (e.g., "Once decided, don't mix `UserService` and `projectService` in the same codebase")
>
> **Incomplete looks like:**
> - Folder structure without file extensions (the agent can't generate filenames)
> - Naming rules that contradict the framework's conventions (e.g., `[Name]Page.tsx` in Next.js where pages live at `app/[route]/page.tsx`)
> - Two different naming patterns for the same artifact (e.g., "Input DTOs are `CreateUserInput` or `UserInputDto`" — pick one)
>
> **Ask triggers:**
> - Framework dictates a folder structure incompatible with the template defaults (e.g., Next.js App Router, SvelteKit) → ask the human to confirm framework-driven structure before filling
> - Tech Spec's language choice means file extension is ambiguous (e.g., TypeScript vs. JavaScript in the same project) → ask
>
> **Cross-reference checklist:**
> - File extensions match Tech Spec → Tech Stack language choice
> - Folder structure compatible with framework conventions in Tech Spec → Tech Stack
> - No naming pattern conflicts with reserved framework keywords
>
> **Remove this block before delivering the filled doc.**

> Single source of truth for where things live and what they're called. Resolve ambiguity here before the first file is created.

### Folder Structure

```
[project root]
├── [frontend root — e.g., src/]
│   ├── pages/          ← Page components (one per route)
│   ├── layouts/        ← Layout components
│   ├── features/       ← Feature components, organized by domain
│   │   ├── [domain]/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   └── [domain].service.[ext]  ← Frontend service layer (if applicable)
│   ├── components/     ← Shared UI primitives
│   ├── dtos/           ← DTO type definitions
│   └── stores/         ← App-level state
│
└── [backend root — e.g., src/ or app/]
    ├── services/       ← Business logic services
    ├── repositories/   ← DB access layer
    ├── dtos/           ← Input/output DTO definitions
    ├── errors/         ← Custom error/exception classes
    ├── controllers/    ← HTTP route handlers (thin — delegate to services)
    └── middleware/     ← Auth, error handling, logging
```

> Adjust paths to match your actual stack. Once set, don't deviate — consistency matters more than perfect structure.

### Naming Rules

| Artifact | Convention | Example |
|----------|-----------|---------|
| Page component | `[Name]Page.[ext]` | `DashboardPage.tsx` |
| Layout component | `[Name]Layout.[ext]` | `AppShell.tsx`, `AuthLayout.tsx` |
| Feature component | `[Name].[ext]` | `ProjectCard.tsx` |
| Shared component | `[Name].[ext]` | `Button.tsx` |
| Backend service | `[Domain]Service.[ext]` | `UserService.ts` |
| Repository | `[Domain]Repository.[ext]` | `UserRepository.ts` |
| Input DTO | `[Name]Input.[ext]` or `Create[Name]Dto.[ext]` | `CreateUserInput.ts` |
| Output DTO | `[Name].[ext]` or `[Name]Dto.[ext]` | `UserDto.ts` |
| Custom error | `[Domain][Reason]Error.[ext]` | `UserNotFoundError.ts` |
| Hook (frontend) | `use[Name].[ext]` | `useProjects.ts` |

> **Consistency rule:** Once a naming pattern is decided, it applies everywhere. Don't mix `UserService` and `projectService` in the same codebase. Don't mix `CreateUserInput` and `UserInputDto` for input DTOs — pick one and stick to it.

---

## 🚦 Gate 1 — Foundation Lock (Design Rules & Naming Conventions)

> 🤖 **AGENT INSTRUCTIONS**
>
> **Why this gate matters:** Design Rules and Naming Conventions are referenced by every Registry row, every Detail Block, and every Dependency Map edge. Changing them after Detail Blocks are written means rewriting every affected block. This gate exists to catch foundation problems before that cost is incurred.
>
> **Human sign-off is required before Registries begin.** Do not start writing Component Registry or Service Registry rows until this gate is checked.
>
> **Gate procedure:**
> 1. Walk every checklist item. For each, open the relevant section and verify — do not check from memory.
> 2. If any check fails, do NOT silently fix it. Stop, flag the gap to the human, and resolve before continuing.
> 3. Pay special attention to cross-reference checks against Tech Spec — Tech Spec is the upstream source of truth for architecture pattern, DI approach, and error handling pattern.
>
> **Remove this instruction block before delivering. Keep the checklist and sign-off line.**

### Design Rules Checks

- [ ] All 5 Component Rules are present (or removed with documented rationale)
- [ ] All 6 Service Rules are present (or removed with documented rationale)
- [ ] Every rule has a mechanically-enforceable description (no "should be clean" type language)
- [ ] Project-specific rules (if any) include a description and rationale
- [ ] No project-specific rule contradicts a default rule without explicit rationale
- [ ] "Throws named errors" rule is reconciled with Tech Spec → Error Handling (Result pattern vs. throw — pick one, name the choice)

### Naming Conventions Checks

- [ ] Folder structure declared for frontend (every type from Component Registry has a folder)
- [ ] Folder structure declared for backend (every type from Service Registry has a folder)
- [ ] Naming rule exists for every artifact type used in the project
- [ ] File extensions match Tech Spec → Tech Stack language choice
- [ ] No naming pattern conflicts with framework reserved keywords (verified against Tech Spec → Tech Stack framework)
- [ ] Input DTO and Output DTO naming patterns are decided (only one pattern each — no mixing)

### Cross-Doc Consistency Checks

- [ ] **Tech Spec ↔ Design Rules** — Architecture Overview, Service Pattern, DI Approach, and Error Handling are reflected accurately in Design Rules
- [ ] **Tech Spec ↔ Naming Conventions** — Folder structure is compatible with framework conventions declared in Tech Stack

### Sign-Off

- [ ] **Human sign-off** — Design Rules and Naming Conventions approved. Registries may begin.

Signed: _____________________ Date: ___________

---

## Frontend Component Registry

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Component Registry is filled AFTER Gate 1 sign-off and BEFORE Component Detail Blocks. Every Detail Block references its C-XX ID from this Registry.
>
> **Your job:** Produce the canonical inventory of every frontend component in the app. One row per component. Every component is categorized as Page, Layout, Feature, or Shared.
>
> **A complete Component Registry covers:**
> - One row per component
> - Every component has a C-XX ID (zero-padded if total ≥ 10: C-01, C-02... C-10)
> - Every component has a Type (Page / Layout / Feature / Shared)
> - Every component has a Module ID from `[AppName]_Module_Breakdown.md` Module Registry
> - Every Page component maps to a Screen in `[AppName]_UI_UX.md` Screens section
> - Every Shared component maps to an entry in `[AppName]_UI_UX.md` Shared Component Inventory
> - Status column starts at 🔲 for new components
>
> **Incomplete looks like:**
> - A component with no Module ID (every component must trace to a Module)
> - A Page component with no corresponding Screen in UI/UX
> - A Feature component listed but the corresponding domain isn't in any Module Detail Block's "Domain / Class Entities" column
> - A component named in a Module Detail Block but missing from this Registry
>
> **Ask triggers:**
> - UI/UX has a Screen with no corresponding Page component here → ask whether the Screen is missing a Page or whether it's covered by an existing Page with sub-routes
> - A Module Detail Block names a component that doesn't fit any of the four types → ask before inventing a new type
> - Two Pages have nearly identical purposes → ask before assuming one is redundant
>
> **Cross-reference checklist (verify before declaring section done):**
> - Every Page C-XX maps to a UI/UX Screen
> - Every Shared C-XX maps to a UI/UX Shared Component Inventory entry
> - Every C-XX has a Module ID that exists in Module Breakdown Module Registry
> - Every component named in any Module Detail Block's "Domain / Class Entities" exists here with a C-XX or S-XX ID
>
> **Remove this block before delivering the filled doc.**

> One row per component. Components are either **Pages**, **Layouts**, **Feature** components (domain-specific), or **Shared** UI primitives.

### Component Types

| Type | Description | Examples |
|------|-------------|---------|
| **Page** | Top-level route component. Composes layouts and features. Owns page-level data fetching. | `DashboardPage`, `LoginPage` |
| **Layout** | Structural wrapper reused across pages. No domain logic. | `AppShell`, `AuthLayout`, `SidebarLayout` |
| **Feature** | Domain-specific component tied to one part of the app. May call services/hooks. | `ProjectCard`, `TaskList`, `UserAvatar` |
| **Shared** | Pure UI primitive with no domain awareness. Props-only, no service calls. | `Button`, `Modal`, `DataTable`, `Toast`, `EmptyState` |

### Registry

| ID | Component Name | Type | Route / Location | Module | UI/UX Reference | Status |
|----|---------------|------|-----------------|--------|----------------|--------|
| C-01 | [Name] | Page | `/[route]` | M-[##] | UI/UX → Screens → [Screen name] | 🔲 |
| C-02 | [Name] | Layout | `layouts/` | M-[##] | — | 🔲 |
| C-03 | [Name] | Feature | `features/[domain]/` | M-[##] | UI/UX → Screens → [Screen using it] | 🔲 |
| C-04 | [Name] | Shared | `components/shared/` | M-[##] | UI/UX → Shared Component Inventory → [entry] | 🔲 |

> **UI/UX Reference column rule:** Page components reference the Screen they render. Feature components reference a Screen that uses them. Shared components reference the Shared Component Inventory entry. Layout components may have no UI/UX reference (they're structural, not visual content).

---

## Backend Service Registry

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Service Registry is filled AFTER Gate 1 sign-off, BEFORE Service Detail Blocks, and BEFORE Service Contracts. Service Registry can be filled in parallel with Component Registry (different domains — frontend vs. backend).
>
> **Your job:** Produce the canonical inventory of every backend service in the app. One row per service. Each service owns a specific business domain. If two rows seem to own the same domain, merge them or flag to the human.
>
> **A complete Service Registry covers:**
> - One row per service
> - Every service has an S-XX ID (zero-padded if total ≥ 10)
> - Every service has a Domain (one phrase — Users / Projects / Auth / Notifications / etc.)
> - Every service has a Module ID from Module Breakdown
> - Every service's Depends On column lists S-XX IDs only (no method names, no Module IDs) — empty for foundation services
> - Status column starts at 🔲
>
> **Incomplete looks like:**
> - Two services with the same Domain (likely should be merged — flag)
> - A service with no Module ID
> - A service listed but no corresponding "Domain / Class Entities" entry in any Module Detail Block
> - Depends On listing a service that depends on this one (cycle — fatal)
>
> **Service rule reminders:**
> - One service = one domain (Single Responsibility — Design Rules)
> - No God services — if a service is named `UtilService` or `HelperService`, push back
> - Service dependency graph must be acyclic
>
> **Ask triggers:**
> - A domain has 8+ methods and the service is feeling crowded → ask whether to split the service
> - A method seems to belong to no specific domain → ask which service owns it; if none, ask whether to create a new service
> - Two services have nearly identical Depends On → consider whether one wraps the other
>
> **Cross-reference checklist (verify before declaring section done):**
> - Every S-XX has a Module ID that exists in Module Breakdown
> - Every service named in any Module Detail Block's "Domain / Class Entities" exists here
> - Every Depends On entry points to an S-XX that exists in this Registry
> - No cycles (trace every Depends On chain to confirm it terminates)
>
> **Remove this block before delivering the filled doc.**

> One row per service. Each service owns a specific domain. If two rows share the same domain, consider merging them.

| ID | Service Name | Domain | Module | Depends On | Status |
|----|-------------|--------|--------|------------|--------|
| S-01 | [Name] | [Domain — e.g., Users, Projects, Auth] | M-[##] | — | 🔲 |
| S-02 | [Name] | [Domain] | M-[##] | S-01 | 🔲 |
| S-03 | [Name] | [Domain] | M-[##] | S-01, S-02 | 🔲 |

### Service Dependency Validation

- [ ] No cycles confirmed (every Depends On chain terminates)
- [ ] No self-references confirmed
- [ ] Every Depends On entry exists in this Registry
- [ ] Every service has a unique Domain (no two services own the same domain)

---

## DTOs

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** DTOs are filled AFTER both Registries are locked and BEFORE Service Contracts. Service Contracts reference DTOs by name; without DTOs defined, contracts reference shapes that don't exist.
>
> **Your job:** Define the data shapes that cross every layer boundary in the app. Input DTOs (component → service), Output DTOs (service → component), Internal DTOs (service → service). Every DTO is fully specified — field name, type, nullability, validation rules, and what's intentionally excluded.
>
> **A complete DTOs section covers:**
> - DTO Registry table listing every DTO in the app
> - One Detail Block per DTO (or per pair of Input + Output if they're tightly coupled)
> - Every Detail Block has field-level rows with type, required/nullable, validation, and notes
> - Every Detail Block has an "Excluded fields and why" section — this is the security-critical part
>
> **Incomplete looks like:**
> - A DTO with type `[Type]` (placeholder not resolved)
> - An Output DTO that exposes sensitive fields (password_hash, internal flags, deleted_at) — security violation
> - An Input DTO that accepts fields the server should set (id, created_at, updated_at) — trust violation
> - A DTO with no "Used By" entry (orphan — flag)
>
> **DTO rule reminders (from architecture intent):**
> - DTOs contain only what the caller needs
> - DTOs are immutable (point-in-time snapshots)
> - Input DTOs and Output DTOs are typically different shapes
> - Two callers needing different shapes from the same operation → two different Output DTOs
>
> **Ask triggers:**
> - A field's type isn't in Schema → ask which Schema field it maps to, or whether it's computed
> - An Input DTO mirrors a DB entity exactly → ask which fields are server-set (almost always at least id and timestamps)
> - An Output DTO exposes a field flagged as sensitive in Schema → ask whether it should be excluded
>
> **Cross-reference checklist (verify before declaring section done):**
> - Every DTO is referenced by at least one C-XX or S-XX
> - Every Input DTO's field types are consistent with Schema → Data Dictionary (and API Contract request schemas)
> - Every Output DTO's field types are consistent with Schema → Data Dictionary (and API Contract response schemas)
> - No Output DTO exposes fields marked as sensitive/internal in Schema
>
> **Remove this block before delivering the filled doc.**

> Data Transfer Objects are the shapes that cross the boundary between component and service (or between services). They are not DB entities. They are not UI view models. They are the agreed-upon data contract between two layers.
>
> **Rules:**
> - DTOs contain only what the caller needs — no internal IDs, no sensitive fields the caller shouldn't see
> - DTOs are immutable — they describe a point-in-time snapshot, not a live object
> - Separate input DTOs (what you send) from output DTOs (what you receive) — they're rarely the same shape
> - If two callers need different shapes from the same operation, use two different output DTOs

### DTO Registry

| DTO Name | Direction | Used By | Source Service | Notes |
|----------|-----------|---------|---------------|-------|
| `[Name]Dto` | Input | C-[##] → S-[##] | [ServiceName] | [e.g., Create payload] |
| `[Name]Dto` | Output | S-[##] → C-[##] | [ServiceName] | [e.g., Safe public representation] |
| `[Name]Dto` | Internal | S-[##] → S-[##] | [ServiceName] | [e.g., Cross-service data pass] |

---

### DTO Detail Blocks

---

#### `[Name]InputDto`

**Direction:** Input (Component → Service)
**Used in:** `[ServiceName].[methodName]`

| Field | Type | Required | Validation | Notes |
|-------|------|----------|------------|-------|
| `[field]` | [Type] | Yes / No | [Rules — e.g., min length, format, allowed values] | [e.g., Stripped of HTML before use] |

**What's excluded and why:** [Fields intentionally omitted from input — e.g., `id` is system-generated, `created_at` is set by server, `password_hash` is never accepted as input]

---

#### `[Name]OutputDto`

**Direction:** Output (Service → Component)
**Returned by:** `[ServiceName].[methodName]`

| Field | Type | Nullable | Notes |
|-------|------|----------|-------|
| `[field]` | [Type] | Yes / No | [e.g., Never exposes password_hash; computed from raw price + tax] |

**What's excluded and why:** [Internal fields not safe or relevant for the caller — e.g., `deleted_at`, `password_hash`, internal state flags, raw `updated_by_id`]

---

## Component Detail Blocks

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Component Detail Blocks are filled AFTER Component Registry is locked. Skip Detail Blocks for trivial Shared primitives (Button, Spacer, Divider) — those are documented in the design system, not here.
>
> **Your job:** Produce one full specification per non-trivial component. A frontend agent must be able to scaffold the component (props, local state, hooks called, error handling) from this single block, without re-reading any upstream doc.
>
> **A complete Component Detail Block covers all 7 sub-sections — no skipping:**
> 1. **Header** — C-XX · ComponentName · Type — exactly matches Registry row
> 2. **Status / Route or Location / Module** — three single-line facts
> 3. **Purpose** — one sentence: what this component does for the user
> 4. **Props** — table with name, type, required, default, description
> 5. **Local State** — table with name, type, initial value, description (UI state only — domain state is a Design Rules violation)
> 6. **Services / Hooks Called** — table with service.method, when called, what it returns
> 7. **DTOs Used + Error Handling + Notes** — DTOs in/out, errors caught and UI response, any non-obvious behavior
>
> **Incomplete looks like:**
> - A Page component with no "Services / Hooks Called" rows (every Page fetches data — flag)
> - "Local State" containing domain data (current user, cart contents) — Design Rules violation, flag and move to a store
> - "Props: standard" without listing the props
> - "Error Handling: handle errors" without specifying which errors and what the UI does
>
> **Design Rules enforcement (the agent checks this for every Detail Block):**
> - No business logic in Local State (loading/open/closed only)
> - No raw `fetch()` calls in Services / Hooks Called (must go through a service or hook)
> - No domain models in Props or DTOs Used (DTOs only)
>
> **Ask triggers:**
> - Component has 15+ props → ask whether to break it up or use a context/store
> - Component needs to call 4+ services on mount → ask whether the parent should orchestrate
> - Component's Local State has 8+ entries → ask whether some belongs in a store
>
> **Cross-reference checklist (verify before declaring each block done):**
> - C-XX matches Registry row
> - Component type matches Registry Type
> - Module ID matches Registry Module
> - Every service/hook called has a corresponding S-XX in Service Registry (or is a framework-provided hook like `useRouter`)
> - Every DTO referenced exists in DTO Registry
> - Every error caught exists in Error Catalog
>
> **Remove this block before delivering the filled doc.**

> One block per non-trivial component. Skip detail blocks for simple Shared primitives — document those in the design system instead.

---

#### C-01 · [ComponentName] · [Type]

**Status:** 🔲 Not Started
**Route / Location:** `[route or file path]`
**Module:** M-[##]

**Purpose:** [What does this component do for the user? One sentence.]

**Props**

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `[prop]` | [Type] | Yes / No | — | [What it controls] |

**Local State**

| State | Type | Initial Value | Description |
|-------|------|---------------|-------------|
| `[stateVar]` | [Type] | [value] | [What it tracks — loading, open/closed, form values, etc.] |

> ⚠️ If local state is growing to track business data (not just UI state), that's a Design Rules violation. Move it to a service or store.

**Services / Hooks Called**

| Service / Hook | When Called | What It Returns |
|----------------|------------|-----------------|
| `[ServiceName].[method]` | [On mount / on submit / on user action] | [What data comes back] |

**DTOs Used**

| DTO | Direction | Notes |
|-----|-----------|-------|
| `[DtoName]` | In / Out | [What it represents] |

**Error Handling**

| Error | Source | How Component Responds |
|-------|--------|----------------------|
| `[ErrorName]` | Service / API | [Show toast / inline message / redirect] |

**Notes:** [Any non-obvious behavior, conditional rendering rules, or performance concerns]

---

## Service Detail Blocks

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Service Detail Blocks are filled AFTER Service Registry is locked and AFTER DTOs are defined. Service Contracts (next section) restate the Exposed Methods table here in a stricter pre/postcondition format — both must agree.
>
> **Your job:** Produce one full specification per service. A backend agent must be able to scaffold the service (constructor with DI, methods with signatures, transactional boundaries, errors thrown) from this single block, without re-reading Tech Spec or Schema.
>
> **A complete Service Detail Block covers all 7 sub-sections — no skipping:**
> 1. **Header** — S-XX · ServiceName — exactly matches Registry row
> 2. **Status / Domain / Module / File location** — four single-line facts
> 3. **Purpose** — 2–3 sentences: what this service is responsible for; what breaks if it doesn't work correctly
> 4. **Injected Dependencies** — table listing repositories, external clients, other services this service needs
> 5. **Exposed Methods** — table with method name, input DTO, returns, throws, notes
> 6. **Transactional Operations** — table listing methods that span multiple DB calls and must be atomic
> 7. **DB Entities Accessed + Notes / Risks** — entities + operation type (R/W/D), any non-obvious behavior
>
> **Incomplete looks like:**
> - A service method that lists multiple DB writes but isn't in Transactional Operations (atomicity gap — flag)
> - A method with no Throws column populated (every method that can fail must declare its errors)
> - A method that returns an ORM entity instead of a DTO (Design Rules violation — flag)
> - A service that instantiates `new SomeClient()` instead of receiving it via DI (Design Rules violation — flag)
> - "Injected Dependencies: standard" without listing them
>
> **Design Rules enforcement (the agent checks this for every Detail Block):**
> - Every method returns a DTO, void, or `void` — never an ORM entity or domain object
> - Every multi-write method appears in Transactional Operations
> - Every method that can fail declares specific named errors (not generic `Error`)
> - All dependencies are in Injected Dependencies (no `new SomeClient()` inside methods)
> - No presentation logic in any method (no string formatting for display, no HTML, no UI decisions)
>
> **Ask triggers:**
> - A method does 6+ things → ask whether to split into multiple methods or whether some operations belong in another service
> - A method's transactional scope is unclear (some writes can fail independently?) → ask before guessing
> - A service has 10+ methods → ask whether it's becoming a God service
>
> **Cross-reference checklist (verify before declaring each block done):**
> - S-XX matches Registry row
> - Module ID matches Registry Module
> - Domain matches Registry Domain
> - File location matches Naming Conventions
> - Every Injected Dependency is either a repository, an external client (declared in Tech Spec → Dependencies & Integrations), or another S-XX
> - Every Input DTO and Output DTO referenced exists in DTO Registry
> - Every error in Throws exists in Error Catalog
> - Every DB entity accessed exists in `[AppName]_DB_Schema.md` Entities
>
> **Remove this block before delivering the filled doc.**

---

#### S-01 · [ServiceName]

**Status:** 🔲 Not Started
**Domain:** [What business domain this service owns]
**Module:** M-[##]
**File location:** `services/[name].[ext]`

**Purpose:** [What this service is responsible for. 2–3 sentences. What breaks if it doesn't work correctly?]

**Injected Dependencies**

> What this service needs provided to it. Never instantiated internally.

| Dependency | Type | What It's Used For |
|------------|------|-------------------|
| `[RepositoryName]` | Repository | [DB access for which entity] |
| `[ExternalClient]` | External API client | [What external service — must match Tech Spec → Dependencies & Integrations] |
| `[OtherService]` | Service | [What it delegates to — must be an S-XX in Registry] |

**Exposed Methods**

> The public interface of this service. Full contracts (with pre/postconditions) are in the Service Contracts section.

| Method | Input DTO | Returns | Throws | Notes |
|--------|-----------|---------|--------|-------|
| `[methodName]` | `[InputDto]` | `[OutputDto]` | `[ErrorName]` | [Any non-obvious behavior] |
| `[methodName]` | `[InputDto]` | `void` | `[ErrorName]` | [e.g., Wraps in transaction] |

**Transactional Operations**

> Methods that span multiple DB calls and must be atomic. List them explicitly.

| Method | Operations Included | Failure Behavior |
|--------|--------------------|--------------------|
| `[methodName]` | [e.g., insert order + decrement stock + create audit log] | [Rollback all / partial rollback + compensating action] |

**DB Entities Accessed**

| Entity | Operations | Notes |
|--------|------------|-------|
| `[EntityName]` | Read / Write / Delete | [Any access pattern notes] |

**Notes / Risks:** [Anything non-obvious — concurrency concerns, external dependency behavior, rate limits, etc.]

---

## Service Contracts

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Service Contracts are filled AFTER Service Detail Blocks. A contract is the formal restatement of a Detail Block's Exposed Methods table — adding preconditions, postconditions, transactional declaration, and explicit throws conditions. Both must agree; the Detail Block is the working surface, the Contract is the testable interface.
>
> **Your job:** Restate every service method as a formal contract: preconditions (what must be true before calling), postconditions (what is guaranteed after success), throws conditions (what failures look like), and transactional declaration. This is the surface against which integration tests and mocks are written.
>
> **A complete Service Contract covers:**
> - One contract block per service (same S-XX as Service Detail Block)
> - One entry per public method in the service
> - Every entry has explicit preconditions (the caller's responsibilities)
> - Every entry has explicit postconditions (what the service guarantees on success)
> - Every entry has throws conditions (which named error for which condition)
> - Methods that span multiple writes declare `Transaction: YES`
>
> **Incomplete looks like:**
> - "Preconditions: caller is authenticated" without specifying which authentication state
> - "Postconditions: record is created" without specifying any of: ID is generated, audit log is written, related entities are linked
> - "Throws: ValidationError" without specifying for what condition
> - A multi-write method without `Transaction: YES`
>
> **Pre/Postconditions are not comments — they're the contract.** A caller who violates a precondition gets undefined behavior. A service that violates a postcondition has a bug.
>
> **Ask triggers:**
> - A method's postconditions are unclear (success could mean two different end states) → ask before writing
> - Pre/postconditions reference a state not described in Tech Spec → State Machines → ask whether the state machine is missing the state
>
> **Cross-reference checklist:**
> - Every method signature in this contract matches the Detail Block's Exposed Methods table (no method here that's not there; no method there that's not here)
> - Every Input DTO and Output DTO referenced exists in DTO Registry
> - Every error referenced exists in Error Catalog
> - Transactional declaration matches the Detail Block's Transactional Operations table
>
> **Remove this block before delivering the filled doc.**

> Defines what each service exposes as a formal interface — method signatures, inputs, outputs, pre/postconditions. Language-agnostic. Think of this as the "what" a service promises to deliver, independent of how it's implemented.
>
> **Why this matters:** Contracts let you swap implementations, write mocks for tests, and verify callers and services stay in sync. If a caller depends on a method signature that changes, this is where you catch it.

### [ServiceName] Contract — S-[##]

```
[ServiceName]
│
├── [methodName](input: [InputDtoName]) → [OutputDtoName]
│     Preconditions: [What must be true before calling this]
│     Postconditions: [What is guaranteed to be true after success]
│     Throws: [ErrorName] if [condition]
│     Transaction: NO
│
├── [methodName](id: UUID) → [OutputDtoName] | null
│     Preconditions: [e.g., Caller is authenticated]
│     Postconditions: [e.g., Returns null if not found — never throws NOT_FOUND]
│     Throws: [ErrorName] if [condition]
│     Transaction: NO
│
└── [methodName](input: [InputDtoName]) → void
      Preconditions: [e.g., Record exists and belongs to caller]
      Postconditions: [e.g., Record is soft-deleted, audit log entry created]
      Throws: [ErrorName] if not found
      Throws: [ErrorName] if business rule prevents deletion
      Transaction: YES — wraps all writes
```

> **Pre/Postconditions:** These are the contract, not comments. A caller who violates a precondition gets undefined behavior. A service that violates a postcondition has a bug.

### [ServiceName] Contract — S-[##]

*(Repeat one block per service in Service Registry order)*

---

## Error Catalog

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define every named error the service layer can throw. Each error has a name, an owner service, an HTTP mapping (used by the controller layer), a meaning, and the specific condition that triggers it.
>
> **A complete Error Catalog covers:**
> - One row per named error
> - Every error referenced in any Service Detail Block or Service Contract appears here
> - Every error has an owner service (S-XX) — the service that throws it
> - Every error has an HTTP mapping for the controller layer
> - Every error has a "When Thrown" condition that's specific, not vague
> - Naming convention declared at the bottom (one pattern, consistent)
>
> **Incomplete looks like:**
> - An error referenced in a Service Contract but missing from this catalog
> - "When Thrown: when the operation fails" — not specific enough
> - Two errors with overlapping meanings (e.g., `UserNotFoundError` and `UserDoesNotExistException` — pick one)
> - HTTP mapping inconsistent with Tech Spec → Error Handling
>
> **Generic errors are not acceptable** as substitutes for named errors. `Error`, `Exception`, `null` returns — all violate Design Rules.
>
> **Ask triggers:**
> - Tech Spec → Error Handling uses a Result/Either pattern instead of throwing → reconcile with Design Rules choice made at Gate 1; ask if unclear
> - A condition could be modeled as multiple distinct errors or one generic error → ask before collapsing (specific errors are better — easier to handle)
>
> **Cross-reference checklist:**
> - Every error referenced in any Service Detail Block exists here
> - Every error referenced in any Service Contract exists here
> - Every error referenced in any Component Detail Block's Error Handling table exists here
> - HTTP mappings match Tech Spec → Error Handling
> - HTTP mappings match API Contract → Error Codes Reference
>
> **Remove this block before delivering the filled doc.**

> Every named error the service layer can throw. Errors are defined here, owned by a service, and caught by components or middleware.
>
> **Rules:**
> - Every error has a name, a meaning, and an owner
> - Components catch errors and decide how to display them — they don't define what the errors mean
> - Generic errors (`Error`, `Exception`, `null`) are not acceptable substitutes for named errors
> - HTTP mapping lives in the API layer, not the service — the service throws `UserNotFoundException`, the controller maps it to 404

| Error Name | Owner Service | HTTP Mapping | Meaning | When Thrown |
|------------|--------------|-------------|---------|-------------|
| `UserNotFoundException` | S-01 UserService | 404 | The requested user does not exist or is not visible to the caller | `getById()` when ID not found or soft-deleted |
| `DuplicateEmailError` | S-01 UserService | 409 | A user with this email already exists | `create()` when email uniqueness check fails |
| `InsufficientPermissionError` | S-02 AuthService | 403 | Caller is authenticated but not permitted to perform this operation | Any permission check failure |
| `BusinessRuleViolationError` | [Owning service] | 422 | The request is valid but violates a domain rule | [Specific rule — e.g., cannot delete a completed order] |

> **Naming convention:** [Choose one and stick to it — e.g., `[Domain][Reason]Error` or `[Reason]Exception`]. Define it here at fill time.

---

## Dependency Map

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first.** Dependency Map is filled AFTER Component Registry, Service Registry, and Service Contracts. It maps relationships those sections declare — it doesn't introduce new components or services.
>
> **Your job:** Visualize every cross-layer relationship. Component → Service (which UI calls which backend logic). Service → Service (which services depend on which). Service → Repository / External (which services touch which entities or external clients). The tree at the bottom is the blast-radius reference for "if I change this, what breaks?"
>
> **A complete Dependency Map covers:**
> - Component → Service table (every C-XX with at least one service call)
> - Service → Service table (every S-XX with a non-empty Depends On in Service Registry)
> - Service → Repository / External table (every S-XX with an Injected Dependency that's a Repository or External client)
> - Text dependency tree showing the full graph
> - Shared-dependency callouts (services depended on by many)
>
> **Incomplete looks like:**
> - A component listed in Component Registry but missing from Component → Service (every non-Layout component should appear)
> - A Service Registry Depends On entry not reflected in Service → Service
> - A Service Detail Block's Injected Dependency not reflected in Service → Repository / External
> - A "shared dependency" callout missing for a service used by 5+ components or services
>
> **DAG validation rules:**
> - No cycles in Service → Service (already validated at Service Registry, re-confirm here)
> - No orphan components (every non-Layout C-XX appears in Component → Service or has a documented reason)
> - No orphan services (every S-XX appears as a caller or callee somewhere in the map)
>
> **Ask triggers:**
> - A service appears as a dependency of 5+ services or components → ask whether to flag it in Module Breakdown Risk & Blockers
> - A component has 6+ service dependencies → ask whether it's doing too much (orchestration belongs in a service)
>
> **Cross-reference checklist:**
> - Every entry in Component → Service has a C-XX in Component Registry and an S-XX (or framework hook) in Service Registry
> - Every entry in Service → Service matches Service Registry Depends On column
> - Every entry in Service → Repository / External matches a Service Detail Block's Injected Dependencies table
> - External clients in Service → Repository / External match Tech Spec → Dependencies & Integrations
>
> **Remove this block before delivering the filled doc.**

> Shows which components call which services, and which services depend on other services. Use this to understand blast radius when a service changes.

### Component → Service

| Component | Calls Service(s) | Via |
|-----------|-----------------|-----|
| C-[##] [Name] | S-[##] [Name] | [Direct call / hook / store action] |
| C-[##] [Name] | S-[##] [Name], S-[##] [Name] | [Direct call] |

### Service → Service

| Service | Calls Service(s) | Why |
|---------|-----------------|-----|
| S-[##] [Name] | S-[##] [Name] | [e.g., Delegates permission check] |

### Service → Repository / External

| Service | Uses | Type | Notes |
|---------|------|------|-------|
| S-[##] [Name] | `[RepositoryName]` | Repository | [Which DB entity] |
| S-[##] [Name] | `[ClientName]` | External API | [e.g., Stripe, SendGrid — must match Tech Spec → Dependencies & Integrations] |

### Dependency Tree (Text)

```
[C-01: PageName]
└── [S-01: ServiceName]
    ├── [Repository: EntityRepository]
    └── [S-02: OtherService]
        └── [Repository: OtherEntityRepository]

[C-02: OtherPage]
└── [S-01: ServiceName] ← shared dependency
```

> ⚠️ If a component appears in many branches or a service has many dependents, changes there have broad blast radius. Flag those in the Risk section of the Module Breakdown.

### DAG Validation

- [ ] No cycles in Service → Service
- [ ] Every non-Layout component appears in Component → Service (or has a documented reason)
- [ ] Every service appears as a caller or callee somewhere in the map
- [ ] Shared dependencies (services depended on by 5+ components or services) are flagged

---

## Open Questions

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Track any open question that blocks a Detail Block from being marked Done. One row per question. Resolve before Gate 2.
>
> **A complete Open Questions table:**
> - Has zero unresolved rows when this doc reaches Gate 2
> - Each row names the specific C-XX, S-XX, or DTO affected
> - Each row has an owner (Ryan / Claude / TBD)
> - Each row has a "Needed By" — typically a phase or specific date
>
> **Remove this block before delivering the filled doc.**

| Question | Affects | Priority | Owner | Needed By | Resolution |
|----------|---------|----------|-------|-----------|------------|
| [Question] | [C-XX / S-XX / DtoName] | High / Med / Low | [Ryan / Claude / TBD] | [Phase or date] | [Empty until resolved] |

---

## 🚦 Gate 2 — Full Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Why this gate matters:** This is the last gate before this doc is consumed by Testing Strategy (every C-XX and S-XX becomes test scope), Build Decisions Log, Mid-Build Review, and direct coding agents. Errors here cascade into every downstream coding artifact.
>
> **Human sign-off is required.** Do not declare this doc Done without explicit human approval.
>
> **Gate procedure:**
> 1. Walk every checklist item. For each, open the relevant section and verify — do not check from memory.
> 2. Re-verify the bidirectional Module ID link by opening Module Breakdown for every Module ID referenced. Drift since Gate 1 is common.
> 3. Confirm three-way consistency for every service: Service Registry row ↔ Service Detail Block ↔ Service Contract. Method signatures, throws, transactional flags must agree across all three.
> 4. If any check fails, flag to the human and resolve before declaring Done.
>
> **Remove this instruction block before delivering. Keep the checklist and sign-off line.**

### Completeness

- [ ] Overview is filled — architecture, stacks, service pattern, DI approach, totals, source docs
- [ ] Design Rules locked (Gate 1 signed off)
- [ ] File & Naming Conventions locked (Gate 1 signed off)
- [ ] Component Registry has one row per component; every row has a Module ID and a Type
- [ ] Service Registry has one row per service; every row has a Module ID and a Domain
- [ ] DTO Registry has one row per DTO; every DTO has at least one Detail Block
- [ ] Every non-trivial component has a Detail Block (trivial Shared primitives are documented in the design system)
- [ ] Every service has a Detail Block
- [ ] Every service has a Service Contract block
- [ ] Error Catalog has one row per named error
- [ ] Dependency Map covers every relationship declared in Registries and Detail Blocks
- [ ] Open Questions has zero blocking entries

### Three-Way Service Consistency

- [ ] For every service: method signatures in Service Registry's Exposed Methods, the Service Detail Block's Exposed Methods table, and the Service Contract block AGREE on method name, input DTO, return type, and throws.
- [ ] For every service: Transactional Operations in the Detail Block match `Transaction: YES` declarations in the Service Contract.
- [ ] For every service: Injected Dependencies in the Detail Block match the dependency graph in Dependency Map → Service → Service and Service → Repository / External.

### Cross-Doc Consistency

- [ ] **Module ↔ Component/Service bidirectional link** — for every Module ID referenced here, opened `[AppName]_Module_Breakdown.md` and confirmed that Module's Detail Block "Domain / Class Entities" column lists the corresponding C-XX or S-XX. Reverse: every class entity named in any Module Detail Block has a C-XX or S-XX here.
- [ ] **UI/UX ↔ Component Registry** — every UI/UX Screen has a corresponding Page component (C-XX) here; every UI/UX Shared Component Inventory entry has a corresponding Shared component (C-XX) here.
- [ ] **API Contract ↔ Component/Service** — every endpoint in API Contract maps to a frontend hook or service method here AND to a backend service method here. No orphan endpoints; no orphan services without endpoints (unless service is internal).
- [ ] **Schema ↔ Service Registry** — every Schema entity has at least one service that owns CRUD operations for it (verified via Service Detail Block's DB Entities Accessed).
- [ ] **Tech Spec ↔ Error Catalog** — every error here has an HTTP mapping consistent with Tech Spec → Error Handling and API Contract → Error Codes Reference.
- [ ] **Tech Spec ↔ Service Detail Blocks** — every External client referenced in any Injected Dependencies table exists in Tech Spec → Dependencies & Integrations.

### Design Rules Discipline

- [ ] No Local State in any Component Detail Block contains domain data (only UI state)
- [ ] No Component Detail Block makes a raw `fetch()` call (every API call goes through a service or hook)
- [ ] No Service Detail Block returns an ORM entity or domain object (DTOs only)
- [ ] No Service Detail Block instantiates a dependency internally (all deps injected)
- [ ] Every multi-write service method appears in its Detail Block's Transactional Operations table
- [ ] Every error a service throws is a named error from Error Catalog (no generic `Error`)
- [ ] No Service Detail Block contains presentation logic (no string formatting for display, no HTML, no UI decisions)

### Cleanup Verification

- [ ] Searched the file for `🤖` — zero hits
- [ ] Searched the file for `❓ AGENT PAUSE` — zero hits
- [ ] Searched the file for "Remove this block" — zero hits
- [ ] Every `🚦 GATE` block contains only its checklist and sign-off line — no agent prose

### Sign-Off

- [ ] **Human sign-off** — Component/Service Layer Map is complete, internally consistent, cross-doc consistent, and ready to drive Testing Strategy and downstream coding agents.

Signed: _____________________ Date: ___________
