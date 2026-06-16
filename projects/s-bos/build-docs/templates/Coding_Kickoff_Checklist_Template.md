# Coding Kickoff Checklist: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized
> **Fill order:** Last design-phase doc — runs after all four core design docs are signed off and the Cross-Doc Validation Checklist is signed off. This is the **final gate before coding begins**.
>
> **Purpose of this doc:** Single-page final verification that design is complete and coding-ready. This doc does NOT generate new content — it verifies that content already exists in the source docs. If a box can't be checked, the gap is fixed in the source doc first, not patched here.
>
> **Source docs:**
> - `[AppName]_Product_Design_Doc.md`
> - `[AppName]_DB_Schema.md`
> - `[AppName]_Technical_Spec.md`
> - `[AppName]_UI_UX.md`
> - `[AppName]_UI_Strings.md`
> - `[AppName]_Cross_Doc_Validation_Checklist.md`
>
> **Agent role:** Verifier, not generator. For every checkbox, the agent reads the referenced source section and confirms the content exists and is complete. The agent never checks a box on memory — it opens the source doc, reads, then checks. If the verification fails, the agent does NOT silently move on and does NOT attempt to fix the gap here. The agent flags the specific gap with the specific doc + section that needs work, and the human resolves it in the source doc.
>
> **The three rules while running this checklist:**
> 1. No box gets checked without reading the source doc. No answers from memory.
> 2. If a box fails verification, name the specific gap and the specific doc + section — don't just say "section is incomplete."
> 3. The Cross-Doc Validation Checklist is the authoritative consistency check. This doc does NOT re-verify cross-doc consistency item by item — that's already been done. This doc verifies the Cross-Doc Validation Checklist itself is signed off, then confirms the no-red-flags list.
>
> **Why this division of labor:**
> - Each design doc's own gates verify its internal consistency.
> - The Cross-Doc Validation Checklist verifies consistency across docs.
> - This Coding Kickoff Checklist verifies that all of the above are done and no red flags remain.
>
> **When this doc is fully filled and approved:** Remove every `🤖 AGENT INSTRUCTIONS` block, every `❓ AGENT PAUSE` prompt, and the agent-facing instruction prose inside `🚦 GATE` blocks. Keep the checklist items, status table, and sign-off line.
>
> **Cleanup verification (before declaring the doc done):**
> - Search the file for `🤖` — zero hits
> - Search the file for `❓ AGENT PAUSE` — zero hits
> - Search the file for "Remove this block" — zero hits

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| 1. Foundation | ⏳ Not Started | — | Verify against Tech Spec |
| 2. Data Model | ⏳ Not Started | — | Verify against DB Schema |
| 3. API | ⏳ Not Started | — | Verify against Tech Spec |
| 4. UI / Screens | ⏳ Not Started | — | Verify against UI/UX |
| 5. Design System | ⏳ Not Started | — | Verify against UI/UX + UI Strings |
| 6. Cross-Doc Validation | ⏳ Not Started | — | Verify Cross-Doc Validation Checklist is signed off |
| 7. No Red Flags | ⏳ Not Started | — | Full inventory of common skip-points |
| 🚦 Final Gate — Ready to Code | ⏳ Not Started | — | — |

**Status scheme:** ⏳ Not Started → 🔄 In Progress → ❓ Needs Discussion → ✅ Done

---

## 1. Foundation

> 🤖 **AGENT INSTRUCTIONS**
>
> **Source doc for this section:** `[AppName]_Technical_Spec.md`
>
> **How to verify each item:**
> - Open Tech Spec → "Tech Stack" section. Confirm every layer (Frontend, Backend, Database, Auth, ORM, Hosting) has a specific technology + version + rationale. No "TBD" cells.
> - Open Tech Spec → "Architecture Overview" section. Confirm the architecture pattern is named (monolith / API+SPA / microservices / etc.) and the System Diagram + Components table are consistent.
> - Open Tech Spec → "Deployment & Environments" section. Confirm Dev, Staging, Production are each defined with setup, database, and deploy trigger.
> - Open Tech Spec → "Deployment & Environments → CI/CD Pipeline." Confirm trigger, steps, and tool are specified.
> - Open Tech Spec → "Environment Variables" section. Confirm every variable has Required (Yes/No), Example Value (placeholder), and Description. Cross-check: every secret named in Security Considerations appears here.
>
> **If any box fails:** Do not check it. Name the specific item and the Tech Spec section where the gap exists.
>
> **Remove this block before delivering the filled doc.**

- [ ] Tech stack confirmed (Frontend, Backend, Database, Auth, ORM, Hosting) — verified in Tech Spec → Tech Stack
- [ ] Architecture pattern decided (monolith / API + SPA / microservices / etc.) — verified in Tech Spec → Architecture Overview
- [ ] All environments defined (Dev, Staging, Production) — verified in Tech Spec → Deployment & Environments
- [ ] CI/CD pipeline planned (trigger, steps, tool) — verified in Tech Spec → Deployment & Environments
- [ ] All environment variables listed with example values — verified in Tech Spec → Environment Variables

---

## 2. Data Model

> 🤖 **AGENT INSTRUCTIONS**
>
> **Source doc for this section:** `[AppName]_DB_Schema.md`
>
> **How to verify each item:**
> - Open DB Schema → "Entities" section. For every entity defined: confirm fields with types, constraints (NOT NULL, UNIQUE, CHECK), and descriptions are filled — no "TBD."
> - Open DB Schema → "Relationships" section. For every relationship: confirm FK nullability is stated with a reason, and on-delete behavior (CASCADE / SET NULL / RESTRICT / no action) is decided.
> - Open DB Schema → "Migration Notes" or equivalent. Confirm soft-delete strategy is declared per entity (soft via `deleted_at` / hard delete / mixed — and which entities use which).
> - Open DB Schema → "Migration Notes" or "Seed Order." Confirm parent → child seed/migration order is documented for any data that requires it.
> - Open Tech Spec → "Tech Stack" → ORM/Migration tool row. Confirm a specific migration tool is chosen (Prisma migrate / Flyway / Alembic / etc.).
>
> **If any box fails:** Do not check it. Name the specific entity, relationship, or section with the gap.
>
> **Remove this block before delivering the filled doc.**

- [ ] All core entities defined with fields, types, and constraints — verified in DB Schema → Entities
- [ ] All relationships documented with FK nullability + reason + on-delete behavior — verified in DB Schema → Relationships
- [ ] Soft-delete strategy decided per entity (soft / hard / mixed) — verified in DB Schema
- [ ] Seed / migration order documented (parent tables before child tables) — verified in DB Schema
- [ ] Migration tool chosen — verified in Tech Spec → Tech Stack

---

## 3. API

> 🤖 **AGENT INSTRUCTIONS**
>
> **Source doc for this section:** `[AppName]_Technical_Spec.md`
>
> **How to verify each item:**
> - Open Tech Spec → "API Endpoints / Services" section. For every endpoint: confirm method, path, auth requirement (Required / Public), validation rules per field, and success status code. No "standard validation" — actual rules.
> - Open Tech Spec → "Error Handling" section. Confirm the standard error response format is defined (JSON shape, where errors are caught, full status code table).
> - Open Tech Spec → "Authentication & Authorization" section. Confirm method, login flow, token expiration, refresh strategy, token storage, authorization model, enforcement layer, and roles + permissions table are all filled.
> - Cross-check: every endpoint that fires a state transition (per Tech Spec State Machines) appears here. If a State Machine references an endpoint not listed, flag it.
>
> **If any box fails:** Do not check it. Name the specific endpoint or section.
>
> **Remove this block before delivering the filled doc.**

- [ ] All endpoints listed (method, path, auth requirement) — verified in Tech Spec → API Endpoints
- [ ] Every endpoint has validation rules documented per field — verified in Tech Spec → API Endpoints
- [ ] Every endpoint has a success status code — verified in Tech Spec → API Endpoints
- [ ] Error response format standardized — verified in Tech Spec → Error Handling
- [ ] Auth method decided (JWT / session / OAuth / etc.) — verified in Tech Spec → Authentication & Authorization
- [ ] Roles and permissions table complete — verified in Tech Spec → Authentication & Authorization

---

## 4. UI / Screens

> 🤖 **AGENT INSTRUCTIONS**
>
> **Source doc for this section:** `[AppName]_UI_UX.md`
>
> **How to verify each item:**
> - Open UI/UX → "Screens / Pages" section. For every Screen block: confirm route, purpose, permissions, key elements with specific entity fields, primary actions, all 4 states (Loading / Empty / Error / Populated), re-fetch triggers, and responsive behavior are filled.
> - Pay specific attention to: Every Screen must have Empty and Error states filled — these are the most-skipped.
> - Pay specific attention to: Every Screen must have at least one Re-fetch trigger row — even "on mount, never again" counts but must be stated.
> - Open UI/UX → "Screen → API Map" section. Confirm every Screen has at least one row, and every endpoint listed exists in Tech Spec → API Endpoints.
> - Open UI/UX → "Navigation / IA → Screen Tree." Confirm every Screen appears in the tree.
> - Open UI/UX → "Responsive Design" section. Confirm breakpoints are defined and per-component behavior is described for at least Navigation, Data tables, Forms, and Modals.
>
> **If any box fails:** Do not check it. Name the specific Screen and the missing element (e.g., "Screen `/projects/:id` has no Empty state defined").
>
> **Remove this block before delivering the filled doc.**

- [ ] All screens listed with routes — verified in UI/UX → Screens / Pages
- [ ] Every screen has all 4 states defined (Loading, Empty, Error, Populated) — verified per Screen in UI/UX
- [ ] Every screen has re-fetch triggers documented — verified per Screen in UI/UX
- [ ] Screen → API Map complete (every screen knows which endpoints it calls) — verified in UI/UX → Screen → API Map
- [ ] Navigation / Screen Tree complete (every screen appears in the tree) — verified in UI/UX → Navigation / IA
- [ ] Responsive behavior defined for key components — verified in UI/UX → Responsive Design

---

## 5. Design System

> 🤖 **AGENT INSTRUCTIONS**
>
> **Source docs for this section:** `[AppName]_UI_UX.md` and `[AppName]_UI_Strings.md`
>
> **How to verify each item:**
> - Open UI/UX → "Design System / Brand Guidelines" section. Confirm every color, typography, and spacing token has a name AND a value — no blanks.
> - Open UI/UX → "Design System / Brand Guidelines → Core components." Confirm every primitive (Button, Input, Card, Badge, Modal, Toast, Empty state) has its variants enumerated.
> - Open UI/UX → "Accessibility" section. Confirm WCAG level is declared (2.1 AA minimum) and every requirement row has a specific implementation + enforcement layer — not "best practices."
> - Open UI/UX → "User Interactions / State Management → Error States." Confirm the Error States table has rows covering: auth failures (login + session expiry), validation failures, conflict (409), server error (500), network failures. Plus rows for every app-specific failure mode.
> - Open UI Strings → "Validation Errors master list." Confirm every form field with NOT NULL or UNIQUE in the Schema has a corresponding error row.
> - Open UI Strings → "Toast Messages." Confirm every UI/UX Error State row with "Toast" pattern has a copy row here.
>
> **If any box fails:** Do not check it. Name the specific gap.
>
> **Remove this block before delivering the filled doc.**

- [ ] Color tokens defined with values — verified in UI/UX → Design System
- [ ] Typography tokens defined — verified in UI/UX → Design System
- [ ] Spacing scale defined — verified in UI/UX → Design System
- [ ] Core component variants listed (Button, Input, Card, Badge, Modal, Toast, Empty state) — verified in UI/UX → Design System
- [ ] Accessibility standard declared with specific implementations — verified in UI/UX → Accessibility
- [ ] Error states defined for every user-initiated action that can fail — verified in UI/UX → User Interactions → Error States
- [ ] Validation error copy exists for every required / unique field — verified in UI Strings → Validation Errors Master List
- [ ] Toast copy exists for every UI/UX "Toast" error pattern — verified in UI Strings → Toast Messages

---

## 6. Cross-Doc Validation

> 🤖 **AGENT INSTRUCTIONS**
>
> **Source doc:** `[AppName]_Cross_Doc_Validation_Checklist.md`
>
> **How to verify:**
> - Open the Cross-Doc Validation Checklist. Confirm the final sign-off line shows ✅ Approved.
> - Do NOT attempt to re-verify the individual cross-doc consistency items here. That's the Cross-Doc Validation Checklist's job. If it's signed off, those checks have already been performed.
> - If the Cross-Doc Validation Checklist is not yet signed off: STOP. This Coding Kickoff cannot proceed. Return to the Cross-Doc Validation Checklist and complete it.
>
> **This is a hard gate.** If the Cross-Doc Validation Checklist is not signed off, the rest of this Coding Kickoff is moot — coding cannot begin regardless of the other sections' status.
>
> **Remove this block before delivering the filled doc.**

- [ ] Cross-Doc Validation Checklist signed off — see `[AppName]_Cross_Doc_Validation_Checklist.md`

> **This is a hard gate.** The Cross-Doc Validation Checklist is the authoritative consistency check across all design docs. If it is not signed off, coding cannot begin regardless of the status of sections 1–5 above.

---

## 7. No Red Flags

> 🤖 **AGENT INSTRUCTIONS**
>
> **What this section is:** Final scan for the patterns that have caused mid-coding rework in past projects. Each item maps to a category of skip-point that happens even after individual doc gates pass.
>
> **How to verify each item:**
> - **Undefined entities:** Cross-check PDD Features and Workflows against DB Schema Entities. If an entity is named in a feature or workflow step but not in the Schema, flag.
> - **Ambiguous workflows:** Re-read every PDD Workflow step. If any step says "system does X" without X being specific enough to derive code from, flag.
> - **Missing API contracts:** Cross-check PDD Features against Tech Spec endpoints. If a feature implies an endpoint that doesn't exist, flag.
> - **Undefined validation:** Open Tech Spec API Endpoints. For every field accepted by an endpoint, confirm validation rules are specified.
> - **Features without success criteria:** Cross-check PDD Features against PDD Success Outcomes. Every feature must have a defined success outcome.
> - **Success Metric → Analytics mapping:** Cross-check PDD Success Metrics against Tech Spec → Analytics → Event Inventory. Every metric must map to at least one event (or analytics is explicitly deferred with rationale).
> - **Feature flags:** Open Tech Spec or Decisions Log. Confirm an explicit "using flags / not using flags" decision exists.
> - **Build Decisions Log file existence:** Confirm `[AppName]_Build_Decisions_Log.md` file exists (even if empty). It must exist before coding starts so entries can be logged in real time.
> - **State Machines complete:** Cross-check DB Schema status enums (3+ values) against Tech Spec → State Machines. Every such enum must have a state machine.
> - **Side Effects in Event Map:** Cross-check every State Machine side effect against the Event Map. Every side effect must have an Event Map row.
>
> **If any flag is found:** Do not check the corresponding box. State the specific finding (e.g., "Workflow `Submit Project` step 4 says 'system processes the submission' without specifying what processing means").
>
> **Remove this block before delivering the filled doc.**

- [ ] No undefined entities (mentioned in PDD Features or Workflows but not defined in Schema)
- [ ] No ambiguous workflows (no step says "system does X" without X being specific enough to derive code from)
- [ ] No missing API contracts (every PDD Feature that needs an endpoint has one in Tech Spec)
- [ ] No fields with undefined validation rules — verified against Tech Spec API Endpoints
- [ ] Every PDD Feature has a defined success outcome
- [ ] Every PDD Success Metric maps to at least one Analytics event (or analytics explicitly deferred with rationale) — verified in Tech Spec → Analytics
- [ ] Feature flag strategy declared (using flags / not using flags — not left blank) — verified in Tech Spec or Decisions Log
- [ ] Build Decisions Log file created (`[AppName]_Build_Decisions_Log.md`) — empty is fine, but the file exists
- [ ] Every Schema status enum with 3+ values has a State Machine in Tech Spec
- [ ] Every State Machine side effect appears as a row in Tech Spec → Events & Side Effects → Event Map

---

## 🚦 Final Gate — Ready to Code

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off. This is the final gate. Coding begins after this sign-off and not before.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> The agent's role at this gate is to confirm — verbally to the human — that every section above is checked and the source docs were actually read (not answered from memory). If the agent has been answering from memory, this gate is the moment to flag it: re-verify by opening the docs.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] All 4 core design docs marked ✅ Done in their Status & Next Steps tables (PDD, Schema, Tech Spec, UI/UX)
- [ ] UI Strings marked ✅ Done
- [ ] Cross-Doc Validation Checklist signed off
- [ ] Sections 1–7 above all complete with every box checked
- [ ] No outstanding "TBD" items in any source doc
- [ ] No "Open Questions" in the PDD that block coding
- [ ] Every box on this checklist was verified by reading the source doc, not from memory
- [ ] Build Decisions Log file exists (`[AppName]_Build_Decisions_Log.md`)

**Sign-off status:**

| Item | Status | Notes |
|------|--------|-------|
| All design docs complete | ⏳ | — |
| Cross-Doc Validation signed off | ⏳ | — |
| Coding Kickoff Checklist complete | ⏳ | — |
| Design reviewed by | — | — |
| Ready to code | ⏳ | — |

**Sign-off:**
> 🚦 **Final Gate** — Design phase is complete. All source docs verified internally consistent (per their gates). Cross-doc consistency verified (per Cross-Doc Validation Checklist). No red flags remain. Coding may begin.
>
> **Human sign-off:** ☐ Approved — **Design is ready for coding.**
>
> Any ambiguity found during coding goes back to the source doc first — not resolved ad-hoc in code. Any such gap should also produce an entry in the Build Decisions Log.
