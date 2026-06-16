# Context: [App Name]

> **Purpose:** Orients Claude to this specific project. Read alongside `memory.md` and `restart.md` at the start of every session.
> **Update:** When the app's scope, stack, or working rules change. This is a living document.
> **Copy from:** `App_Design_Context_Template.md` in the Design Templates project. Rename to `[AppName]_Design_Context.md` at session 1 cleanup.

---

## When to Read This File

This is the project's orientation document. Claude reads it at the start of every session, alongside `memory.md` and `restart.md`. It captures what the app is, where it is in the pipeline, and how Claude works on this specific project.

> **Working agreement source:** The Project Instructions panel (loaded automatically by Claude Desktop) holds the working agreement — collaboration model, behavioral rules, working rules, common requests, red flags. That panel is loaded every session before any files are read. **This file does not duplicate that content.** This file captures project-specific orientation only — what *this app* is and what *this project's* current state looks like.

> **First-time project setup:** This file starts as a scaffold and gets populated at the end of session 1, after the first PDD pass surfaces the app name and core decisions. The Session 1 Bootstrap section in the Project Instructions panel walks through that process. For Ryan's pre-project thinking aid, see `New_Project_Kickoff_Template.md` in the Design Templates folder (Ryan-only — Claude does not read it during sessions).

---

## The Collaboration Model — Read This First

> **This is the foundational principle for all human-AI work on this project. Every session, every doc, every interaction runs on this model.**

**Human leads. Agent executes with precision. Nothing gets invented. Ambiguity gets killed before it compounds.**

- The **human is the designer and design leader.** All design decisions belong to the human. The agent never makes a design decision — it asks.
- The **agent is the scribe, depth-adder, and consistency enforcer.** The human describes the design in natural language. The agent translates it into precise, unambiguous documentation that downstream coding agents can execute from without guessing.
- The **agent writes the docs** because it holds the full doc structure in memory simultaneously, knows exactly what each downstream doc needs from the current one, and can enforce consistency across sections in real time. The human can't write at that depth fast enough — and shouldn't have to. Human judgment belongs in design decisions, not documentation precision.

**The agent's three rules — no exceptions:**
1. Everything written traces back to something the human said. No invented design decisions.
2. If something is unclear, stop and ask mid-section. Do not proceed on a guess.
3. Output must be specific enough that the next agent in the pipeline can work from it without ambiguity.

**The failure mode this model prevents:** the agent filling gaps with reasonable guesses that go unchallenged until they're baked three layers deep into the design. Gaps compound. A vague entity in the PDD becomes a missing field in the Schema becomes an undefined API behavior in the Tech Spec becomes a bug in the code. Kill the ambiguity at the source.

---

## What This App Is

**Name:** [App Name]

**One-sentence description:** [What does it do and for whom? E.g., "A scheduling platform for dental clinics to manage appointments across multiple providers."]

**Problem it solves:** [The specific pain — not a product pitch, the actual problem users have today.]

**Primary user:** [One person, one role — the person who uses this most.]

**Secondary user(s):** [Admin, manager, customer — whoever else interacts with the system.]

**Core value:** [The one thing this app must do well to be worth using. Everything else is secondary.]

---

## Current State

**Phase:** [Pre-Design / Design / Coding / MVP Live / Iterating]

> File-level status lives in the **File Inventory** at the bottom of this doc — single source of truth for which files exist, their phase, and their state. Don't duplicate it here.

---

## Tech Stack

> Fill in as decisions are made. Leave `—` until decided — don't pre-fill guesses.

| Layer        | Technology | Version | Rationale |
| ------------ | ---------- | ------- | --------- |
| Frontend     | —          | —       | —         |
| Backend      | —          | —       | —         |
| Database     | —          | —       | —         |
| Auth         | —          | —       | —         |
| ORM          | —          | —       | —         |
| Hosting      | —          | —       | —         |
| Queue / Jobs | —          | —       | —         |
| Email        | —          | —       | —         |
| Other        | —          | —       | —         |

---

## Key Constraints & Non-Negotiables

> Things that are decided and not up for discussion. Stack locks, compliance requirements, platform constraints, budget limits.

- [Constraint — e.g., "Must use React Native — mobile app required"]
- [Constraint — e.g., "HIPAA compliance required — PHI handling rules apply"]
- [Constraint — e.g., "Must integrate with existing Salesforce CRM"]
- [Constraint — e.g., "Solo developer — no microservices, keep it simple"]

---

## Third-Party Integrations

> Services this app will use. Fill in before starting the DB Schema Doc — integrations can shape the data model.

| Service          | Purpose               | Data model impact                       | Status                   |
| ---------------- | --------------------- | --------------------------------------- | ------------------------ |
| [e.g., Stripe]   | [Payments]            | [Requires `stripe_customer_id` on User] | [API key obtained / TBD] |
| [e.g., SendGrid] | [Transactional email] | [None]                                  | [TBD]                    |
| [e.g., Auth0]    | [Authentication]      | [Requires `auth0_user_id` on User]      | [TBD]                    |
| [Service]        | [Purpose]             | [Impact]                                | [Status]                 |

> Cross-reference: Each service here should appear in the Tech Spec's Dependencies & Integrations section and the Events & Side Effects Delivery Methods table.

---

## What We're NOT Building (Scope Boundaries)

> Explicit out-of-scope decisions. Write these down — "out of scope" is a design decision, not an oversight.

| Feature / Capability | Why excluded | Reconsider when                    |
| -------------------- | ------------ | ---------------------------------- |
| [Feature]            | [Reason]     | [Condition — or "Never / Phase 2"] |
| [Feature]            | [Reason]     | [—]                                |

---

## Open Questions

> Decisions that haven't been made yet. These gate design work — resolve them before the section that depends on them.

| Question   | Blocks              | Priority         | Owner        |
| ---------- | ------------------- | ---------------- | ------------ |
| [Question] | [Which doc/section] | High / Med / Low | [Ryan / TBD] |

---

## Project-Specific Working Rules

> Project-specific rules that extend or override the generic ones in the Project Instructions panel. Examples: "This app handles PHI — flag PHI exposure risks immediately", "This is a solo project — prioritize simplicity over scalability", "This is a complex multi-tenant app — explain non-obvious architectural patterns whenever introduced."
>
> If a rule belongs in every project, it goes in the Project Instructions panel. If it's specific to *this* app, it goes here.

- [Add project-specific rules during session 1 cleanup, or as they emerge.]

---

## File Inventory

> Single source of truth for every file in this project. Status updates as files move through the pipeline. Sub-header rows mark phase transitions — read top-to-bottom to see exactly where the project is.

| File | Purpose | Status |
|------|---------|--------|
| **— Continuity (read at session start) —** | | |
| `[AppName]_Design_Context.md` | This file — project orientation | ✅ Active |
| `memory.md` | Running facts, decisions, context | ✅ Active |
| `restart.md` | Where we stopped, next steps | ✅ Active |
| **— Design —** | | |
| `[AppName]_Pre-Design_Thought_List.md` | Pre-design thinking aid (used optionally in session 1) | ⏳ Not Started |
| `[AppName]_Product_Design_Doc.md` | Problem, users, entities, features, workflows | ⏳ Not Started |
| `[AppName]_DB_Schema.md` | Data model, entities, relationships, data dictionary | ⏳ Not Started |
| `[AppName]_Technical_Spec.md` | Architecture, stack, services, state machines, events | ⏳ Not Started |
| `[AppName]_UI_UX.md` | Design system, screens, shared components, navigation | ⏳ Not Started |
| `[AppName]_UI_Strings.md` | All user-visible copy — labels, errors, toasts, empty states | ⏳ Not Started |
| `[AppName]_Sample_Data.md` | Canonical sample rows for demos, tests, screenshots | ⏳ Not Started |
| `[AppName]_Decisions_Log.md` | Design-phase architecture decisions (AD-XX) | ⏳ Not Started |
| `[AppName]_Cross_Doc_Validation_Checklist.md` | Cross-doc consistency check — signed off before coding | ⏳ Not Started |
| **— Coding Prep —** | | |
| `[AppName]_Coding_Kickoff_Checklist.md` | Pre-coding gate — env, packages, folder structure | ⏳ Not Started |
| `[AppName]_Module_Breakdown.md` | Modules, dependencies, build order, acceptance criteria | ⏳ Not Started |
| `[AppName]_API_Contract.md` | Request/response schemas, example payloads | ⏳ Not Started |
| `[AppName]_Database_Migration_Checklist.md` | Migration registry, patterns, rollback | ⏳ Not Started |
| `[AppName]_Component_Service_Layer_Map.md` | Frontend components and backend services | ⏳ Not Started |
| `[AppName]_Testing_Strategy.md` | Test types, coverage plans, CI integration | ⏳ Not Started |
| `[AppName]_Deployment_Config.md` | Environments, build, rollback procedure | ⏳ Not Started |
| `[AppName]_Pre_Build_Validation_Checklist.md` | Cross-mesh validation across coding-phase docs — signed off before coding | ⏳ Not Started |
| **— Build —** | | |
| `[AppName]_Build_Decisions_Log.md` | Typed log of build-phase decisions (BD-XX) | ⏳ Not Started |
| `[AppName]_Progress_Checklist.md` | Per-module build progress | ⏳ Not Started |
| `[AppName]_Mid_Build_Review_1.md` | Mid-build drift check between code and design docs (first instance — additional `_2`, `_3`, etc. created as needed) | ⏳ Not Started |
| **— Closeout —** | | |
| `[AppName]_Phase_Closeout_1.md` | Per-phase closeout (first instance — additional `_2`, `_3`, etc. as needed) | ⏳ Not Started |
| `[AppName]_Project_Closeout.md` | Final closeout — generates the worklog file below | ⏳ Not Started |
| `[AppName]_Template_Update_Worklog.md` | Generated by Project Closeout — feeds template improvements back upstream | ⏳ Not Started |
| **— As needed —** | | |
| `[AppName]_Discussion_[topic-slug].md` | One file per topic when a Concern is promoted to discussion (created on demand from `Discussion_File_Template.md`) | ⏳ Not Started |

---

## How to Start a Session

1. Project Instructions panel loads automatically (working agreement always present).
2. Claude reads this file, `memory.md`, and `restart.md`.
3. Claude confirms what we're working on before taking any action.
4. If any continuity file is missing or stale, Claude flags it immediately.
5. Say "let's go" or name the task — Claude picks up from `restart.md`'s Next Steps.
