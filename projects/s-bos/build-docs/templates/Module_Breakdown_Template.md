# Module Breakdown: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized
> **Fill order:** First coding-phase doc. Begin after the design phase is fully signed off (PDD ✅ Schema ✅ Tech Spec ✅ UI/UX ✅ UI Strings ✅ Coding Kickoff Checklist ✅).
>
> **Source docs (every module traces upstream to these):**
> - `[AppName]_Product_Design_Doc.md` — Core Features, User Workflows, Out of Scope, Timeline / Phases
> - `[AppName]_DB_Schema.md` — Entities, Relationships (drives Data modules and DB Entities Involved in every Detail Block)
> - `[AppName]_Technical_Spec.md` — Tech Stack, API Endpoints, State Machines, Auth, Events & Side Effects, Deployment & Environments, Environment Variables (drives Service / Infrastructure / Integration / DevOps modules and most cross-references)
> - `[AppName]_UI_UX.md` — Shared Component Inventory, Screens (drives Component modules and UI Screens Involved in every Feature module)
> - `[AppName]_UI_Strings.md` — Per-Screen strings (referenced indirectly via UI/UX screen names)
>
> **Downstream docs that consume this one (write to feed them):**
> - `[AppName]_API_Contract.md` — Uses Module IDs to scope endpoint contracts. Each endpoint listed in a module's "API Endpoints Involved" must have a full contract in API Contract.
> - `[AppName]_Database_Migration_Checklist.md` — Each migration row references the Module ID(s) that introduce it. The Migration column in each Detail Block closes the loop.
> - `[AppName]_Component_Service_Layer_Map.md` — Components (`C-XX`) and Services (`S-XX`) live there. The "Domain / Class Entities" column in each Detail Block names the classes; Component/Service Map gives them their `C-XX` / `S-XX` IDs and contracts.
> - `[AppName]_Testing_Strategy.md` — Tests reference Module IDs in scope. The verify scripts defined here are referenced there.
> - `[AppName]_Deployment_Config.md` — DevOps modules drive its CI/CD steps.
> - `[AppName]_Progress_Checklist.md` — One row per module, status mirrors this doc.
> - `[AppName]_Mid_Build_Review.md` — Checks drift between built code and these module specs.
> - `[AppName]_Build_Decisions_Log.md` — BD entries reference Module IDs when a workaround / deviation / gap affects a specific module.
>
> **Agent role:** Decompose the app into the minimum set of buildable units that produce a working system in a valid order. Every module must trace to at least one upstream source (PDD Feature, Schema Entity, Tech Spec component, UI/UX Screen, or explicit human direction). No invented modules. No invented dependencies.
>
> **The three rules while filling this doc:**
> 1. Everything written traces to PDD + Schema + Tech Spec + UI/UX + confirmed human input. No invented modules. No invented dependencies. No invented acceptance criteria.
> 2. If a module's scope, dependency, or acceptance criterion is unclear, stop and ask. Do not proceed on a guess — Module Breakdown errors cascade into every coding-phase doc downstream.
> 3. Output must be specific enough that a coding agent can pick up a single module, read its Detail Block + upstream references, and start building without re-deriving anything from the design docs.
>
> **Two failure modes drive most of the design here — both are addressed by gates:**
> - **Invalid DAG.** A module depends on something that depends on it (cycle), or a module is scheduled in Phase N but its dependency is in Phase N+1. Either makes the build order impossible.
> - **Weak acceptance criteria.** "Module is done when it works" is not done. The three-category Functional / Environmental / Verification structure exists specifically to defeat the "unit tests pass but the installer is broken" failure mode that kills late-stage builds.
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
> 2. Module Type Reference (review only — adjust if project needs new types)
> 3. **Module Registry** — must come first; everything else references Module IDs from here
> 4. **Dependency Map** — must come after Registry; can't map dependencies that aren't inventoried
> 5. **Build Order / Phase Plan** — must come after Dependency Map; sequencing requires a known DAG
> 6. **🚦 Gate 1 — Module Inventory & Dependency Graph** (human sign-off before Detail Blocks)
> 7. **Module Detail Blocks** — must come after Build Order; phase assignment is a per-block field
> 8. Risk & Blockers (concurrent with Detail Blocks — flag as they surface)
> 9. Open Questions (concurrent — flag as they surface)
> 10. **🚦 Gate 2 — Full Module Breakdown Sign-Off** (final gate before downstream coding-phase docs consume this)

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Overview | 🔲 Not Started | — | — |
| Module Type Reference | 🔲 Not Started | — | Review and adjust per project |
| Module Registry | 🔲 Not Started | — | Strict ordering — fill before Dependency Map |
| Dependency Map | 🔲 Not Started | — | Strict ordering — fill after Registry |
| Build Order / Phase Plan | 🔲 Not Started | — | Strict ordering — fill after Dependency Map |
| 🚦 Gate 1 — Module Inventory & Dependency Graph | 🔲 Not Started | — | Human sign-off required before Detail Blocks |
| Module Detail Blocks | 🔲 Not Started | — | One block per module — strict ordering applies |
| Risk & Blockers | 🔲 Not Started | — | Populate concurrently with Detail Blocks |
| Open Questions | 🔲 Not Started | — | Populate concurrently with Detail Blocks |
| 🚦 Gate 2 — Full Module Breakdown Sign-Off | 🔲 Not Started | — | Final gate before downstream coding-phase docs |

**Coding Status values:** 🔲 Not Started / 🔄 In Progress / 👀 In Review / ✅ Done / 🚫 Blocked

---

## Overview

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Anchor this doc to the project. Pull the App name and phase directly from the PDD. Total Modules is filled in last (after the Registry is complete).
>
> **A complete Overview covers:**
> - App name (from PDD)
> - Phase (e.g., MVP, Phase 2 — from PDD Timeline / Phases)
> - Total Modules count (fill last)
> - Source Docs pinned to their exact filenames
>
> **Remove this block before delivering the filled doc.**

- **App:** [App Name]
- **Phase:** [MVP / Phase 2 / etc. — from PDD Timeline]
- **Total Modules:** [Fill last]
- **Source Docs:**
  - PDD: `[AppName]_Product_Design_Doc.md`
  - DB Schema: `[AppName]_DB_Schema.md`
  - Tech Spec: `[AppName]_Technical_Spec.md`
  - UI/UX: `[AppName]_UI_UX.md`
  - UI Strings: `[AppName]_UI_Strings.md`

---

## Module Type Reference

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Review this table against the project. The list below is a starting point — add or remove types if the project needs them.
>
> **A complete Module Type Reference covers:**
> - Every module in the Registry maps to exactly one Type from this table
> - If a module doesn't fit any type cleanly, add a new type here with its description — don't force-fit
>
> **Ask triggers:**
> - The app has a major architectural layer not covered by any type here (e.g., a separate analytics pipeline, a ML model serving layer, a hardware integration layer)
>
> **Remove this block before delivering the filled doc.**

| Type               | Description                                                    | Examples                                                 |
| ------------------ | -------------------------------------------------------------- | -------------------------------------------------------- |
| **Foundation**     | Project scaffolding, environment setup, base configuration     | Repo init, env config, folder structure, DB connection   |
| **Infrastructure** | Cross-cutting technical concerns used by all other modules     | Auth, logging, error handling, middleware, rate limiting |
| **Data**           | Migrations, schema creation, seeders, data pipelines           | Initial migration, seed scripts, data import             |
| **Service**        | Business logic layer — domain rules, orchestration, processing | NotificationService, PaymentProcessor, PermissionChecker |
| **Integration**    | Third-party connections and external API wrappers              | Stripe, Twilio, Google Calendar, SendGrid                |
| **Feature**        | A discrete user-facing capability                              | Scheduling, Billing, User Profile, Search                |
| **Component**      | Reusable UI element used across multiple screens               | DatePicker, Modal, DataTable, Toast, EmptyState          |
| **DevOps**         | CI/CD pipelines, deployment config, environment provisioning   | GitHub Actions workflow, Dockerfile, staging deploy      |

---

## Module Registry

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first:** This section MUST be filled before Dependency Map and Build Order. Module IDs are assigned here and referenced everywhere else. If the human tries to fill Dependency Map or Build Order before the Registry is complete, push back with:
> *"Dependency Map needs the full module inventory in place first — modules can't depend on modules that haven't been declared. Let's finish the Registry, then I'll build the Dependency Map from it."*
>
> **Your job:** Decompose the app into the smallest set of buildable modules that together produce the full system in scope. Every module is a unit a coding agent (or a human) can pick up, build, and verify in isolation given its dependencies are met.
>
> **A complete Registry covers — every row must have:**
> - **ID** — sequential `M-01`, `M-02`, ... Once assigned, an ID never changes (other docs reference it).
> - **Module Name** — short, specific, no overlap with another module's name. PascalCase or Title Case, consistent within the project.
> - **Type** — exactly one value from the Module Type Reference table.
> - **Description** — one line, specific. "User auth and session management" is good. "Auth stuff" is not.
> - **Phase** — which build phase this module belongs to (e.g., 1, 2, 3 — matches PDD Timeline phases).
> - **Complexity** — S (a few hours to a day) / M (2–5 days) / L (1–2 weeks). If a module would be XL, split it.
> - **Depends On** — comma-separated list of Module IDs this module requires. `—` if none.
> - **Status** — always 🔲 Not Started at the time of filling this doc.
>
> **What the inventory must cover (cross-check against upstream docs before declaring complete):**
> - **Every PDD Core Feature** → at least one `Feature` module (or split across multiple Feature modules if the feature is large)
> - **Every Tech Spec Tech Stack layer** → covered by a `Foundation` or `Infrastructure` module that initializes / configures it
> - **Every Tech Spec external service / integration** → its own `Integration` module
> - **Every UI/UX Shared Component** → its own `Component` module (or grouped logically — e.g., one module for "Form components" covering DatePicker, Select, etc., if they're built together)
> - **Every Tech Spec CI/CD step** → covered by a `DevOps` module
> - **The Schema as a whole** → at least one `Data` module for the initial migration set (more if migrations are phased)
> - **Tech Spec State Machines, Events & Side Effects** → owned by a `Service` module (typically the module owning the entity the state machine is on)
>
> **Incomplete looks like:**
> - A PDD Feature with no corresponding `Feature` module
> - A Tech Spec endpoint with no module owning it (every endpoint must be owned by exactly one module — usually a Feature or Service module)
> - A UI/UX Shared Component with no `Component` module
> - A module marked Complexity = L that on inspection is really multiple separable concerns — split it
> - Two modules with overlapping scope ("UserService" and "AuthService" both handling login) — clarify the boundary or merge
> - A module description that could fit five different things ("Backend logic") — be specific
>
> **Ask triggers — stop and ask the human if:**
> - The PDD lists a Feature that could be one module or three smaller ones — let the human decide the slicing
> - A PDD Out-of-Scope item looks like it might still need a stub — ask before adding a module for it
> - Two PDD Features share so much logic that they might be one module — propose the merge and ask
> - The Tech Spec Tech Stack includes a layer (e.g., a queue, a cache) that doesn't obviously map to either Foundation or Infrastructure — ask which it should be
> - The project uses an unusual architecture (event-driven, micro-frontends, plugin-based) that doesn't fit the standard Module Type set cleanly
>
> **Module sizing heuristics:**
> - **S** — One service class, one CRUD endpoint set, one small component. ~1 day of focused work.
> - **M** — A feature with 3–5 endpoints and a state machine, or a service with non-trivial logic and integrations. 2–5 days.
> - **L** — A feature with a full subsystem (e.g., notifications: queues, templates, multiple delivery methods, retry handling). 1–2 weeks.
> - **XL is not allowed.** If a module looks XL, split it into two or more M / L modules and document the dependency between them.
>
> **Cross-reference checklist (before declaring Registry complete):**
> - Open the PDD Core Features section. Every feature has a Module ID in the Registry that implements it.
> - Open the Tech Spec API Endpoints section. Every endpoint has a Module ID that owns it.
> - Open the Tech Spec State Machines section. Every entity with a state machine has a Module ID that owns its lifecycle logic.
> - Open the UI/UX Shared Component Inventory. Every shared component has a Module ID.
> - Open the Tech Spec Deployment & Environments section. Every CI/CD pipeline step has a Module ID.
>
> **Remove this block before delivering the filled doc.**

> One row per module. Use Module ID (e.g., `M-01`) to reference modules elsewhere and in detail blocks below. Search by ID to jump to any detail block.

| ID   | Module Name | Type   | Description            | Phase          | Complexity  | Depends On | Status         |
| ---- | ----------- | ------ | ---------------------- | -------------- | ----------- | ---------- | -------------- |
| M-01 | [Name]      | [Type] | [One-line description] | [1 / 2 / etc.] | [S / M / L] | —          | 🔲 Not Started |
| M-02 | [Name]      | [Type] | [One-line description] | [1]            | [S / M / L] | M-01       | 🔲 Not Started |
| M-03 | [Name]      | [Type] | [One-line description] | [1]            | [S / M / L] | M-01, M-02 | 🔲 Not Started |

> **Complexity guide:** S = a few hours to a day / M = 2–5 days / L = 1–2 weeks. If a module is XL, break it into smaller modules.

---

## Dependency Map

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first:** This section MUST be filled after the Module Registry is complete. If the human tries to draft this before Registry rows are stable, push back with:
> *"Let's lock the Registry first — if we add or remove modules later, this graph has to be redrawn. I'll build the Dependency Map straight from the Registry once it's complete."*
>
> **Your job:** Translate the "Depends On" column of the Registry into a visual + tabular form that humans can scan for cycles, gaps, and ordering problems. The map is derivative — every edge here comes from a "Depends On" value in the Registry. If the map and the Registry disagree, the Registry is the source of truth and the map gets fixed.
>
> **A complete Dependency Map covers:**
> - **Text tree** — every module appears as a node. Roots (no dependencies) at the top. Leaves (no dependents) at branch ends.
> - **Standalone modules** (no deps, no dependents) listed explicitly below the tree so they're not lost.
> - **Dependency List table** — one row per module, restates the Registry's Depends On with a Notes column explaining *why* the dependency exists ("Needs DB connection from M-01" — not just "depends on M-01").
>
> **DAG validation rules (the agent must check these and flag failures):**
> 1. **No cycles.** Walk the graph. If any path leads back to its starting node, flag the cycle and stop. Cycles must be resolved before this section can be marked complete.
> 2. **No orphan references.** Every Module ID listed in any "Depends On" value must exist in the Registry. A typo (`M-O3` instead of `M-03`) is a silent killer.
> 3. **No self-references.** A module cannot depend on itself.
> 4. **Every module appears exactly once in the tree or in the standalone list.**
>
> **Incomplete looks like:**
> - A "Notes" column that just repeats the Module ID — needs to say *what* the depending module gets from its dependency
> - A module from the Registry that doesn't appear anywhere in the tree or standalone list
> - A dependency edge in the tree that's not in any Registry row's "Depends On"
> - A cycle (any cycle — even self-reference)
>
> **Ask triggers — stop and ask the human if:**
> - The Registry's Depends On values produce a cycle. Flag the cycle explicitly: *"M-04 depends on M-07, M-07 depends on M-04. One of these edges has to be removed — which module actually owns the shared logic?"*
> - A "Depends On" edge is unclear in purpose — what does the dependent module actually need from its dependency?
> - Two modules have effectively the same dependency set and similar purpose — they might be one module
>
> **Remove this block before delivering the filled doc.**

> Shows build order at a glance. Two formats — use whichever fits the project. Keep both if helpful.

### Text Tree

```
[M-01: Foundation]
└── [M-02: Infrastructure]
    ├── [M-03: Feature A]
    │   └── [M-05: Feature C]
    └── [M-04: Feature B]
        └── [M-06: Feature D]

[M-07: Component A]  ← no dependencies, can build anytime
[M-08: Integration A] ← depends on M-02
```

### Dependency List

> For each module, what must be done first. The Notes column states *what specifically* the depending module needs from the dependency — not just that the dependency exists.

| Module | Depends On | Notes                                      |
| ------ | ---------- | ------------------------------------------ |
| M-01   | —          | Starting point — no dependencies           |
| M-02   | M-01       | Needs DB connection pool + env config from Foundation |
| M-03   | M-01, M-02 | Needs auth middleware from M-02; reads from User table created by M-01 |
| M-07   | —          | Standalone UI component, buildable anytime |

---

## Build Order / Phase Plan

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first:** This section MUST be filled after Dependency Map. Sequencing requires a validated DAG. If the human tries to set build order before the Dependency Map is complete, push back with:
> *"Phase ordering depends on the validated DAG. If we set build order now and then find a cycle or missing edge, we'll have to redo the phases. Let me finish the Dependency Map first."*
>
> **Your job:** Group modules into build phases that ship a working slice of the system at each phase boundary. Phases are not arbitrary sprint buckets — each phase has a goal that traces to the PDD Timeline, and a "Done when" condition that's checkable, not aspirational.
>
> **A complete Phase Plan covers:**
> - **Phase numbers match PDD Timeline phases** (Phase 1 here = MVP if the PDD calls it MVP).
> - **Goal** per phase — what does completing this phase enable? Trace to PDD ("Phase 1 ships the core scheduling workflow listed in PDD Feature F-03").
> - **Done when** per phase — specific, checkable condition. Not "core features complete" — "M-01 through M-08 all ✅ Done, staging deployment passes smoke test, demo workflow runs end-to-end."
> - **Order column per module within a phase** — within a phase, modules with met dependencies can build in parallel. The Order column is the recommended sequence (1 = start here), accounting for dependency depth.
>
> **The hard rule (flag any violation immediately):**
> ⚠️ **A module cannot appear in Phase N if any of its dependencies are in Phase N+1 or later.** The agent must check this before marking the section complete. Walk each phase: every dependency of every module in the phase must be in the same phase or an earlier phase.
>
> **Incomplete looks like:**
> - A phase Goal that's vague ("Build the foundation") — name what specifically gets built
> - A "Done when" that's not checkable ("Phase 1 features complete") — replace with explicit checklist
> - A module placed in Phase N with a dependency in Phase N+1 (impossible — must be fixed)
> - Two modules with the same Order number in the same phase (Order should be unique within a phase; modules that could go in parallel still get distinct Order numbers based on dependency depth)
> - A phase with no modules — either delete the phase or assign modules to it
>
> **Ask triggers — stop and ask the human if:**
> - The PDD Timeline has phases that don't cleanly correspond to module groupings — ask whether to follow PDD phases or restructure
> - A module's dependencies span multiple phases (e.g., it depends on M-02 in Phase 1 and M-09 in Phase 2) — the module must be in Phase 2 or later. Confirm with human if that pushes a critical feature past where they expected it.
> - The dependency graph forces a phase layout that conflicts with PDD's stated MVP scope — flag the conflict
>
> **Remove this block before delivering the filled doc.**

> Sequence for coding. Phases are project-defined — group by logical milestones tied to PDD Timeline phases, not arbitrary sprints.
> Modules within a phase can be built in parallel if their dependencies are met.

### Phase [1]: [Phase Name — e.g., "Foundation & Infrastructure"]

**Goal:** [What does completing this phase enable? Trace to PDD Timeline.]
**Done when:** [Specific, checkable condition — not "features complete". Example: "M-01 through M-08 all ✅ Done; staging deployment passes smoke test; verify-M-08.sh exits 0."]

| Order | Module ID | Module Name | Notes                                      |
| ----- | --------- | ----------- | ------------------------------------------ |
| 1     | M-01      | [Name]      | Starting point — no dependencies           |
| 2     | M-02      | [Name]      | Requires M-01 complete                     |
| 3     | M-03      | [Name]      | Can start after M-01; parallel with M-02   |

### Phase [2]: [Phase Name — e.g., "Core Features"]

**Goal:** [What does completing this phase enable?]
**Done when:** [Specific, checkable condition]

| Order | Module ID | Module Name | Notes |
| ----- | --------- | ----------- | ----- |
| 1     | M-04      | [Name]      | —     |
| 2     | M-05      | [Name]      | —     |

> ⚠️ A module must never appear in Phase N if any of its dependencies are in Phase N+1.

---

## 🚦 Gate 1 — Module Inventory & Dependency Graph

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off.**
>
> This gate matters because everything downstream — Detail Blocks, the Coding-Phase docs, the actual build — assumes the module inventory is complete and the dependency graph is valid. A missed module or an invalid DAG caught here costs minutes; caught after Detail Blocks are written, it costs hours; caught during coding, it costs days.
>
> Run every check carefully. Open the upstream docs and verify — do not check from memory.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**

*Registry completeness*
- [ ] Every PDD Core Feature has at least one `Feature` module in the Registry
- [ ] Every Tech Spec API endpoint is owned by exactly one module (no orphan endpoints, no double-owned endpoints)
- [ ] Every Tech Spec State Machine has a `Service` (or `Feature`) module that owns the entity's lifecycle logic
- [ ] Every Tech Spec external service / integration has its own `Integration` module
- [ ] Every UI/UX Shared Component has a `Component` module (or is explicitly grouped into one)
- [ ] Every Tech Spec Tech Stack layer is initialized by a `Foundation` or `Infrastructure` module
- [ ] Every Tech Spec CI/CD step is covered by a `DevOps` module
- [ ] At least one `Data` module covers the initial Schema migration set
- [ ] No module is marked Complexity = XL — all are S, M, or L
- [ ] Every Registry row has a non-empty Description that's specific, not generic

*Dependency Map validity*
- [ ] Every Module ID referenced in any "Depends On" cell exists in the Registry
- [ ] No self-references (no module depends on itself)
- [ ] No cycles — walking any dependency chain never returns to its starting module
- [ ] Every module from the Registry appears in the Text Tree or in the standalone-modules list
- [ ] Every dependency edge has a Notes value that states *what* the depending module needs

*Phase Plan validity*
- [ ] Every phase has a Goal that traces to PDD Timeline
- [ ] Every phase has a "Done when" that's a checkable condition, not aspirational
- [ ] No module appears in Phase N with a dependency in Phase N+1 or later (walk every phase)
- [ ] Order column is unique within each phase
- [ ] Total Modules in Overview matches the Registry row count

**Sign-off:**
> 🚦 **Gate 1** — Module inventory is complete, dependency graph is acyclic and traces to upstream docs, phase ordering is valid. Ready to write Detail Blocks.
>
> **Human sign-off:** ☐ Approved — proceed to Module Detail Blocks

---

## Module Detail Blocks

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first:** Detail Blocks come after Gate 1 sign-off. If a Detail Block is written before its module's place in the dependency graph and phase plan is locked, the Block may reference dependencies that get reshuffled later. Push back if the human wants to draft Detail Blocks before Gate 1:
> *"Detail Blocks reference upstream IDs and phase positions. Let me get Gate 1 signed off first — if the graph or phases change, we'd have to rewrite every block. After Gate 1, the Detail Blocks are stable to write."*
>
> **Your job — write one Detail Block per module in the Registry.** Each block is the complete spec a coding agent (or human) needs to build that module without re-reading the design docs. Pull every cross-reference from its source doc. If a cross-reference value is not in the source doc, stop and ask — do not invent.
>
> **Each Detail Block must cover all sub-sections.** The structure is fixed. Do not omit sub-sections; if a sub-section genuinely doesn't apply, write `N/A — justification: [reason]`. Silence is not an answer.
>
> **Per-sub-section guidance (read this once, apply to every block):**
>
> - **Purpose** — 2–4 sentences. Not a restatement of the Registry Description. Explain what this module does in the system, why it exists, and what breaks if it's missing or built wrong. Cross-reference the PDD Feature or Tech Spec component this module realizes.
>
> - **Depends On / Required By tables** — Pull "Depends On" from the Registry (no new info). The "What's needed from it" / "What they need from this" columns are the value-add — be specific ("Auth middleware exposing `requireAuth()` and `requireRole(role)`", not "Auth stuff"). Required By is derived from other modules' Depends On values — for each module that depends on this one, list what they pull.
>
> - **DB Entities Involved** — One row per entity this module reads or writes. Operations column lists the actual CRUD ops this module performs (not "may use" — what it actually does). Schema Doc Ref is the section/anchor in `[AppName]_DB_Schema.md`. Migrations column lists `DB-XX` IDs from `[AppName]_Database_Migration_Checklist.md` if any migration in that doc introduces or modifies entities this module owns — write `—` if none.
>
> - **Domain / Class Entities** — Business logic classes / services / domain models this module owns or imports. Not DB tables. Examples: `AuthService`, `TokenValidator`, `PaymentProcessor`, `useScheduling` hook. The Notes column says "Owned by this module" or "Imported from M-XX". These names will get `S-XX` / `C-XX` IDs assigned in the downstream Component/Service Map — for this doc, the class name + ownership is enough.
>
> - **API Endpoints Involved** — Pull from Tech Spec API Endpoints. Every endpoint this module owns gets a row. Method + Path + Purpose + Tech Spec section anchor. If an endpoint isn't owned by exactly one module, flag it — endpoints are not co-owned.
>
> - **UI Screens Involved** — Pull from UI/UX Screens. For `Feature` modules this is the user-facing screens that exercise this module's logic. For `Service` / `Integration` / `Infrastructure` modules, this is typically `N/A`. For `Component` modules, the column lists the screens that use this component (from UI/UX Shared Component Inventory's "Used on" column).
>
> - **Key Logic & Behavior** — The non-obvious stuff. The information that, if omitted, would cause a coding agent to build the module wrong. Pull from: Tech Spec State Machine transitions (for modules owning state machines), Tech Spec Events & Side Effects rows (for modules firing or handling events), PDD User Workflow edge cases, Tech Spec Auth rules (for modules enforcing them). Concrete examples, not platitudes. "If the user submits twice in <500ms, deduplicate by client-side idempotency key" beats "handle duplicate submissions."
>
> - **Acceptance Criteria — three categories.** This is the most important section. The "Functional alone" failure mode is the #1 cause of "we shipped but it's broken in production" outcomes. Every block has Functional + Environmental + Verification. If Environmental genuinely doesn't apply (pure utility module with no deployed surface), write `N/A — justification: [why]`. **Never leave a category blank.** Verification is always required — every module must have a verify script.
>
> - **Done when** — One sentence. Must reference all three categories. Not "module is complete" — name the specific observable outcome that proves all three categories are satisfied.
>
> - **Notes / Risks** — Anything that could slow this module down, require a decision, or create problems for dependent modules. If a Risk is significant, also add it to the Risk & Blockers section below.
>
> **Cross-reference checklist (per block):**
> - Every DB Entity in the table appears in the DB Schema doc
> - Every Migration ID (DB-XX) appears in the Migration Checklist (note: Migration Checklist is filled in parallel — Migration column may temporarily say `[TBD-MigrationChecklist]` if that doc isn't done yet, and gets resolved during Cross-Doc Validation)
> - Every API endpoint appears in Tech Spec API Endpoints
> - Every UI Screen appears in UI/UX Screens (or `N/A` if not a UI-facing module)
> - Every State Machine reference appears in Tech Spec State Machines
> - Every Side Effect reference appears in Tech Spec Events & Side Effects
>
> **Ask triggers — stop and ask the human if:**
> - A module's scope is fuzzy enough that two coding agents would build it differently
> - An endpoint or screen is referenced in the design docs but unclear which module owns it
> - A side effect in Tech Spec is described but the human decision on retry/failure handling is missing
> - Environmental acceptance criteria for a `Foundation` or `Infrastructure` module aren't obvious — what does "the deployed shape" mean for this layer?
>
> **Remove this block before delivering the filled doc.**

> One block per module. Full specification for building that module.
> Find any module by searching its ID (e.g., M-01).

---

### M-01 · [Module Name] · [Type]

**Status:** 🔲 Not Started
**Phase:** [#]
**Complexity:** [S / M / L] — [One-line reason: e.g., "Straightforward CRUD, no complex logic"]

---

**Purpose**

[2–4 sentences. What does this module do, and why does it matter to the system? How does it fit into the larger app? What breaks if this isn't built correctly? Trace to the PDD Feature or Tech Spec component this realizes.]

---

**Depends On**
| Module ID | Module Name | What's needed from it |
|-----------|-------------|----------------------|
| — | — | — |

**Required By**
| Module ID | Module Name | What they need from this |
|-----------|-------------|--------------------------|
| M-02 | [Name] | [e.g., DB connection pool, base env config] |

---

**DB Entities Involved**

> From DB Schema Doc. **If migrations are tracked in `[AppName]_Database_Migration_Checklist.md`, list the relevant `DB-XX` IDs in the Migration column.** This makes the link bidirectional — the Migration Checklist already references modules; this column closes the loop the other way.

| Entity   | Operations                      | Schema Doc Ref    | Migrations |
| -------- | ------------------------------- | ----------------- | ---------- |
| [Entity] | Create / Read / Update / Delete | [Section or link] | [DB-XX, DB-YY, or —] |

---

**Domain / Class Entities**

> Business logic classes, service objects, or domain models this module owns or depends on. Not DB tables — the code-level abstractions. These will be given `S-XX` / `C-XX` IDs in the downstream Component/Service Map.

| Class / Service        | Responsibility | Notes                                       |
| ---------------------- | -------------- | ------------------------------------------- |
| [e.g., AuthService]    | [What it does] | [Owned by this module / imported from M-XX] |
| [e.g., TokenValidator] | [What it does] | —                                           |

---

**API Endpoints Involved**

> From Tech Spec. Every endpoint listed here must appear in the Tech Spec API Endpoints section.

| Method | Path                | Purpose        | Tech Spec Ref |
| ------ | ------------------- | -------------- | ------------- |
| POST   | /api/[resource]     | [What it does] | [Section]     |
| GET    | /api/[resource]/:id | [What it does] | [Section]     |

---

**UI Screens Involved**

> From UI/UX Doc. For non-UI modules, write `N/A — justification: this is a backend service module`.

| Screen        | Route    | Interaction               | UI/UX Doc Ref |
| ------------- | -------- | ------------------------- | ------------- |
| [Screen Name] | /[route] | [What the user does here] | [Section]     |

---

**Key Logic & Behavior**

> The non-obvious stuff that, if omitted, would cause a coding agent to build the module wrong. Pull from Tech Spec State Machines, Events & Side Effects, Auth rules, and PDD edge cases. Concrete, specific, with examples where helpful.

- [Rule or behavior — e.g., "Token must be validated before any protected route is accessed via the `requireAuth` middleware"]
- [Edge case — e.g., "If access token is expired, attempt silent refresh using refresh token; if refresh token is also invalid, return 401 with `code: TOKEN_EXPIRED` and force re-login"]
- [State transition — e.g., "User status: `pending` → `active` on email confirm endpoint; `active` → `suspended` by admin only via `PATCH /api/users/:id/suspend`"]
- [Side effect — e.g., "On `user.signed_up` event, fire welcome email via `EmailService.sendWelcome` (retry 3× with exponential backoff per Tech Spec Event Map row 1)"]

---

**Acceptance Criteria**

> Every module has criteria in **three categories**. "Functional alone" is the failure mode that ships broken installers and phantom features — it's why this template enforces all three. If a category genuinely doesn't apply, write `N/A — justification: [reason]`. Don't leave a section blank — silence is not an answer.

#### 🧪 Functional — does the code do the thing?

> The behavior the module is supposed to produce, verifiable by unit or integration tests in-process. The code-level claims.

- [ ] [Observable behavior — e.g., "`AuthService.login(email, password)` returns a valid JWT when credentials match; throws `InvalidCredentials` otherwise"]
- [ ] [Observable behavior — e.g., "`requireAuth` middleware rejects requests with no Authorization header with 401 and `code: UNAUTHORIZED`"]
- [ ] [Observable behavior — e.g., "Token refresh issues a new access token and invalidates the old refresh token; previously-used refresh tokens are rejected"]

#### 🌍 Environmental — does the artifact behave correctly in its deployed context?

> Behavior that **only manifests outside unit tests** — in the deployed shape of the artifact. This is the category that catches "unit tests pass but the installer is broken" and "works on my machine but not in staging."
>
> For web/API/RN modules: deployed build behavior, network failure handling, auth-expired flows, real API integration, environment-specific config.
> For desktop/CLI modules: clean-machine install behavior, first-run state, DB migrations on a fresh user profile.
> For pure infrastructure / utility modules: write `N/A — justification: [reason]`.

- [ ] [Observable behavior — e.g., "Deployed to staging, login flow succeeds against the staging DB and returns a token usable on subsequent protected endpoints"]
- [ ] [Observable behavior — e.g., "Expired token in production triggers silent refresh; if refresh fails, client redirects to /login"]
- [ ] Or: `N/A — justification: [why this module has no environmental surface]`

#### ✅ Verification — single-command, under-2-minute proof

> A script or command that **a tired human can execute in under 2 minutes** at the end of a long build to confirm the module is real. This is the fatigue defense — if verification takes longer, it gets skipped at exactly the moment skipping is most dangerous.
>
> **The verify script must exist as a committed artifact.** Visibility-through-absence: no script in the repo = module not done. Default location: `/scripts/verify/verify-{module-id}.{ext}` (e.g., `verify-M-24.ps1`, `verify-M-14.sh`, `verify-M-07.bat`). Adjust folder per project conventions (defined in Tech Stack), but the convention must be uniform within a project.
>
> Stack-agnostic forms — use whatever fits the project:
> - Shell scripts: `verify-M-24.ps1`, `verify-M-14.sh`, `verify-M-07.bat`
> - npm/yarn scripts: `npm run verify:M-14`
> - Python: `pytest tests/verify/test_m14.py`
> - HTTP probes: `curl -fS https://staging.example.com/health/m14`
> - Playwright/Cypress E2E: `npx playwright test verify/m14.spec.ts`
>
> The script's output is the gate: exit code 0 = pass, anything else = fail. Human-readable success line is encouraged.

- [ ] `verify-M-01.{ext}` exists at the project's verify-scripts location
- [ ] Script runs in under 2 minutes on a clean checkout
- [ ] Script exits 0 on success and non-zero on failure
- [ ] [What the script proves — one sentence, e.g., "Hits the staging login endpoint with a known test user, asserts 200 + valid JWT, hits a protected endpoint with that JWT, asserts 200. Exits 0."]

---

**Done when:** [One sentence — the clear, unambiguous condition that declares this module complete. Must reference all three categories. Example: "User can register, log in, and access a protected route; all functional unit tests green; staging deployment serves the auth flow correctly; `verify-M-01.sh` exits 0 demonstrating the full round-trip."]

---

**Notes / Risks**

> Anything that could slow this down, require a decision, or create problems for dependent modules. Significant risks should also appear in the Risk & Blockers section.

- [Risk or note]

---

### M-02 · [Module Name] · [Type]

**Status:** 🔲 Not Started
**Phase:** [#]
**Complexity:** [S / M / L] — [Reason]

---

**Purpose**
[2–4 sentences.]

---

**Depends On**
| Module ID | Module Name | What's needed from it |
|-----------|-------------|----------------------|
| M-01 | [Name] | [e.g., DB connection, env config] |

**Required By**
| Module ID | Module Name | What they need from this |
|-----------|-------------|--------------------------|
| M-03 | [Name] | [e.g., Auth middleware] |

---

**DB Entities Involved**
| Entity | Operations | Schema Doc Ref | Migrations |
|--------|------------|---------------|------------|
| — | — | — | — |

---

**Domain / Class Entities**
| Class / Service | Responsibility | Notes |
|-----------------|---------------|-------|
| — | — | — |

---

**API Endpoints Involved**
| Method | Path | Purpose | Tech Spec Ref |
|--------|------|---------|--------------|
| — | — | — | — |

---

**UI Screens Involved**
| Screen | Route | Interaction | UI/UX Doc Ref |
|--------|-------|-------------|--------------|
| — | — | — | — |

---

**Key Logic & Behavior**

- [Rule or behavior]

---

**Acceptance Criteria**

> Same three-category structure as M-01. Don't leave categories blank — use `N/A — justification: ...` if a category genuinely doesn't apply. Verification is always required.

#### 🧪 Functional

- [ ] [Condition]
- [ ] [Condition]

#### 🌍 Environmental

- [ ] [Condition]
- [ ] Or: `N/A — justification: [reason]`

#### ✅ Verification

- [ ] `verify-M-02.{ext}` exists at the project's verify-scripts location
- [ ] Script runs in under 2 minutes
- [ ] Script exits 0 on success
- [ ] [What the script proves — one sentence]

**Done when:** [One sentence referencing all three categories.]

---

**Notes / Risks**

- [Risk or note]

---

*(Continue pattern for M-03, M-04, etc. — one full Detail Block per module in the Registry.)*

---

## Risk & Blockers

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Surface modules with external dependencies, unresolved decisions, high complexity, or known unknowns. Populated concurrently with Detail Blocks — when a Risk surfaces while writing a block, log it here as well as in the block's Notes / Risks section.
>
> **A complete Risk row covers:**
> - Module ID — the affected module
> - Risk / Blocker — what the risk is, concretely
> - Type — External dep / Unknown / Complexity / Decision needed
> - Impact — High / Med / Low (rough — what happens to the build if this risk fires)
> - Mitigation — what reduces or handles it
>
> **Incomplete looks like:**
> - "Risk: complexity" — name what specifically is complex
> - Mitigation column blank — every risk needs at least a planned response
>
> **Ask triggers:**
> - A risk's mitigation requires a human decision (e.g., "depends on whether we use Stripe or Adyen") — flag it as a row in Open Questions too
>
> **Remove this block before delivering the filled doc.**

| Module ID | Risk / Blocker     | Type                                                    | Impact             | Mitigation                   |
| --------- | ------------------ | ------------------------------------------------------- | ------------------ | ---------------------------- |
| [M-XX]    | [What the risk is] | [External dep / Unknown / Complexity / Decision needed] | [High / Med / Low] | [How to reduce or handle it] |

---

## Open Questions

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Track unresolved decisions that affect module scope, dependencies, or build order. Populated concurrently with Detail Blocks. Every Open Question must be resolved before the affected module enters its build phase — note this in the "Needed By" column.
>
> **A complete Open Question row covers:**
> - Question — specific, decidable (not "what should we do about X")
> - Affects Module(s) — Module IDs
> - Priority — High / Med / Low
> - Owner — who answers (Ryan / Claude / TBD)
> - Needed By — the phase or specific date the answer must land
>
> **Remove this block before delivering the filled doc.**

| Question   | Affects Module(s) | Priority         | Owner                 | Needed By       |
| ---------- | ----------------- | ---------------- | --------------------- | --------------- |
| [Question] | [M-XX, M-YY]      | High / Med / Low | [Ryan / Claude / TBD] | [Phase or date] |

---

## 🚦 Gate 2 — Full Module Breakdown Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off. This is the final gate before downstream coding-phase docs (API Contract, Database Migration Checklist, Component/Service Map, Testing Strategy, Deployment Config) consume this doc.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**

*Document completeness*
- [ ] Status & Next Steps table shows no 🔲 or 🔄 — every section is ✅ Done
- [ ] Gate 1 is signed off
- [ ] Overview lists Total Modules matching the Registry row count
- [ ] Every module in the Registry has a corresponding Detail Block

*Per Detail Block — every block must satisfy these*
- [ ] Purpose has 2–4 sentences and traces to PDD Feature or Tech Spec component
- [ ] Depends On / Required By tables match the Registry's dependency graph
- [ ] DB Entities Involved rows trace to DB Schema doc (every entity exists there)
- [ ] Domain / Class Entities listed with ownership clarified
- [ ] API Endpoints Involved rows trace to Tech Spec API Endpoints (every endpoint exists there and is owned by exactly one module across the doc)
- [ ] UI Screens Involved rows trace to UI/UX Screens (or `N/A` with justification)
- [ ] Key Logic & Behavior includes the non-obvious rules — state transitions, side effects, edge cases — not generic descriptions
- [ ] Acceptance Criteria has all three categories filled (Functional, Environmental, Verification) — or `N/A` with justification for Environmental
- [ ] Verification sub-section names the verify script with file extension and one-sentence description of what it proves
- [ ] "Done when" sentence references all three acceptance criteria categories

*Cross-doc traceability*
- [ ] Every PDD Core Feature is owned by at least one Feature module (re-verify after Detail Blocks)
- [ ] Every Tech Spec API endpoint is owned by exactly one module
- [ ] Every Tech Spec State Machine has a module that owns its lifecycle logic
- [ ] Every Tech Spec Side Effect in the Event Map has a module that handles it
- [ ] Every UI/UX Shared Component has a Component module
- [ ] Every UI/UX Screen appears in at least one module's UI Screens Involved table

*Open items*
- [ ] Every Risk in Risk & Blockers has a Mitigation
- [ ] Every Open Question has a Priority, Owner, and Needed By
- [ ] No "TBD" left in any Detail Block (Open items are captured in Risk & Blockers or Open Questions, not as inline TBDs)

**Sign-off:**
> 🚦 **Gate 2** — Module Breakdown complete, internally consistent, traces fully to all upstream design docs. DAG is valid, acceptance criteria are airtight, every module is buildable from its Detail Block alone. Ready for downstream coding-phase docs.
>
> **Human sign-off:** ☐ Approved — Module Breakdown complete. Proceed to API Contract, Database Migration Checklist, and Component/Service Map (these three can be filled in parallel).
