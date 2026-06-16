# API Contract: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized
> **Fill order:** Coding-phase doc. Begin after Module Breakdown is signed off. Can be filled in parallel with Database Migration Checklist and Component/Service Layer Map.
>
> **Source docs (every contract traces upstream to these):**
> - `[AppName]_Technical_Spec.md` — API Endpoints (the canonical endpoint list — every endpoint here gets a full contract), Authentication & Authorization (drives the Auth resource block, the global Auth convention, and per-endpoint Auth value), Error Handling (drives the Error Codes Reference and per-endpoint error rows), Events & Side Effects → Real-Time Events (drives WebSocket Contracts), Events & Side Effects → Delivery Methods → Outbound Webhook (drives Webhook Contracts), State Machines (drives per-endpoint state-transition error scenarios — e.g., `409 Conflict` when a guard fails)
> - `[AppName]_DB_Schema.md` — Entities and Data Dictionary (drives response field types, nullability, and example values), Constraints / Validation Rules (drives request field validation)
> - `[AppName]_Module_Breakdown.md` — Module Registry and Detail Blocks (every endpoint has an owning Module ID — the bidirectional link to "API Endpoints Involved" in each Module Detail Block)
> - `[AppName]_Product_Design_Doc.md` — Indirect — referenced via Tech Spec for feature → endpoint traceability when an error scenario is unclear
>
> **Downstream docs that consume this one (write to feed them):**
> - `[AppName]_Component_Service_Layer_Map.md` — Frontend hooks/services and backend route handlers are built from these contracts. Every field type, nullability, and error scenario here becomes a TypeScript type, a validator, or an error-handling branch there. Precision deficit here → guessing there.
> - `[AppName]_Testing_Strategy.md` — API Contract Tests (`AC-XX`) are written **directly against** these contracts. Every endpoint contract becomes one or more `AC-XX` tests covering happy path + every error row. If an error row is missing here, the test is missing there.
> - `[AppName]_Build_Decisions_Log.md` — BD entries reference endpoint paths when a workaround / deviation affects a specific endpoint. The contract is the baseline the deviation is measured against.
> - Frontend & backend coding agents — read this doc directly to write typed clients, route handlers, request validators, and error handlers. **The downstream-precision standard for this doc is: a frontend coding agent can generate a fully typed client, and a backend coding agent can generate a route handler + validator, from a single endpoint contract — without re-reading Tech Spec or Schema.**
>
> **Agent role:** Translate the Tech Spec's endpoint surface into complete, field-level request/response contracts with realistic example payloads. Every field type, every validation rule, every error scenario must trace to Tech Spec + Schema + confirmed human input. No invented fields. No invented error scenarios. No invented response shapes.
>
> **The three rules while filling this doc:**
> 1. Everything written traces to Tech Spec + Schema + Module Breakdown + confirmed human input. No invented fields. No invented validation rules. No invented error codes. If a field shape isn't in Schema and isn't in Tech Spec, stop and ask — do not invent.
> 2. If a Tech Spec endpoint is unclear about request shape, response shape, or error behavior, stop and ask before writing the contract. Vague contracts produce vague code.
> 3. Output must be specific enough that a frontend coding agent can write a typed client (request type, response type, error union) and a backend coding agent can write a route handler (validator, success response, every error branch) from a single endpoint contract block — without re-reading Tech Spec or Schema.
>
> **Two failure modes drive most of the design here:**
> - **Incomplete error rows.** If the contract lists only the happy path and validation errors but skips the state-machine guard failures (`409`), permission failures (`403`), or business-rule failures (`422`), the downstream code will silently handle some errors and crash on others. Every error scenario from Tech Spec must appear.
> - **Hand-waved request/response shapes.** "Returns the resource" or "accepts the standard fields" is not a contract — it's a hope. Every field gets a row with type, nullability, and example. Every example payload is realistic, not `"string"`.
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
> 2. **API Conventions** — must come first; every endpoint contract assumes these (base URL, auth header, content type, date format, pagination, response envelope)
> 3. **Error Codes Reference** — must come before any Endpoint Contract; endpoint contracts *reference* error codes, they don't re-define them
> 4. **🚦 Gate 1 — Foundation Lock** (human sign-off before per-endpoint contracts begin — Conventions + Error Codes are the contract foundation; mistakes here cascade into every endpoint below)
> 5. **Endpoint Contracts** — per resource, per endpoint. Authentication resource first (every other resource depends on it). Then resources in Tech Spec order.
> 6. **WebSocket Contracts** — only if Tech Spec → Events & Side Effects → Real-Time Events is non-empty. Otherwise mark N/A.
> 7. **Webhook Contracts** — only if Tech Spec → Events & Side Effects → Delivery Methods → Outbound Webhook is non-empty. Otherwise mark N/A.
> 8. **Versioning & Deprecation**
> 9. **🚦 Gate 2 — Full API Contract Sign-Off** (final gate before downstream coding-phase docs consume this)

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Overview | 🔲 Not Started | — | — |
| API Conventions | 🔲 Not Started | — | Strict ordering — fill before Endpoint Contracts |
| Error Codes Reference | 🔲 Not Started | — | Strict ordering — fill before Endpoint Contracts |
| 🚦 Gate 1 — Foundation Lock | 🔲 Not Started | — | Human sign-off required before Endpoint Contracts |
| Endpoint Contracts | 🔲 Not Started | — | One contract block per Tech Spec endpoint |
| WebSocket Contracts | 🔲 Not Started | — | N/A unless Tech Spec → Real-Time Events is non-empty |
| Webhook Contracts | 🔲 Not Started | — | N/A unless Tech Spec → Outbound Webhook is used |
| Versioning & Deprecation | 🔲 Not Started | — | — |
| 🚦 Gate 2 — Full API Contract Sign-Off | 🔲 Not Started | — | Final gate before downstream coding-phase docs |

**Coding Status values:** 🔲 Not Started / 🔄 In Progress / 👀 In Review / ✅ Done / 🚫 Blocked

---

## Overview

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Anchor this doc to the project and pin its source docs. Two-to-three sentences total. The content of every section below is driven by Tech Spec + Schema + Module Breakdown — name them explicitly so the reader knows where every claim in this doc comes from.
>
> **A complete Overview covers:**
> - What this doc is (one sentence)
> - How it relates to Tech Spec (one sentence — Tech Spec defines *what exists*, this doc defines *exactly what to send and expect*)
> - Source docs pinned to their exact filenames
>
> **Remove this block before delivering the filled doc.**

**What this doc is:** The full contract for every API surface in [App Name] — REST endpoints, WebSocket events (if any), and outbound webhooks (if any). Defines exactly what each surface accepts and returns: field-level detail, example payloads, and per-endpoint error behavior.

**How it relates to the Tech Spec:** The Technical Spec is the authority on *what exists and why* — the endpoint list, validation rules, auth requirements, state machine guards, and success codes. This doc is the authority on *exactly what to send and expect* — full request/response schemas, every error scenario, and realistic example payloads. Downstream coding agents read this doc directly.

**Source docs:**
- Tech Spec: `[AppName]_Technical_Spec.md` — API Endpoints, Auth, Error Handling, State Machines, Events & Side Effects
- DB Schema: `[AppName]_DB_Schema.md` — Entities (field types and nullability), Constraints (request validation rules)
- Module Breakdown: `[AppName]_Module_Breakdown.md` — every endpoint contract links to its owning Module ID

---

## API Conventions

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first:** This section MUST be filled before any Endpoint Contract. Every contract below assumes these conventions and references them ("Auth: Required" = the convention defined here; date fields are ISO 8601 as defined here; pagination shape is the envelope defined here). If the human tries to draft endpoint contracts before Conventions are locked, push back with:
> *"Every endpoint contract assumes the conventions are settled — auth header format, date format, pagination shape, response envelope. If we change a convention after writing 20 contracts, we rewrite 20 contracts. Let me lock Conventions first."*
>
> **Your job:** Lock the project-wide conventions every endpoint inherits. Pull from Tech Spec: Base URL from Deployment & Environments, Versioning strategy from API Endpoints / Versioning, Auth from Authentication & Authorization, Pagination from Performance Considerations. Date format defaults to ISO 8601 UTC — only deviate if Tech Spec specifies otherwise.
>
> **A complete API Conventions section covers:**
> - **Base URL** per environment (Dev, Staging, Production) — pull from Tech Spec Deployment & Environments. If a value is not yet known, write `[TBD — confirm at deployment]`, do not invent.
> - **Versioning strategy** — URL path, header, or none — pulled directly from Tech Spec. Include an example.
> - **Authentication** — method, header format, what happens on missing vs. invalid token — pulled from Tech Spec Auth section. Per-endpoint contracts will say `Auth: Required` or `Auth: Public`; the meaning of those values is defined here.
> - **Content Type** — both request and response. Note any deviations (e.g., file upload endpoints use `multipart/form-data`).
> - **Date & Time format** — pin the format explicitly. Default ISO 8601 UTC for timestamps, `YYYY-MM-DD` for date-only. Never Unix timestamps unless Tech Spec demands it.
> - **Pagination** — strategy (cursor / offset / none), request params, and the response envelope shape. Every list endpoint inherits this — they don't redefine it.
> - **Standard response envelope** — single resource shape, list shape, empty success (204). Every endpoint contract uses this shape — they don't redefine it.
>
> **Incomplete looks like:**
> - "Authentication: token-based" — name the specific method (JWT Bearer, session cookie, API key)
> - Pagination "as needed" — pick a strategy; if some endpoints paginate and some don't, that's fine, but the strategy and shape are defined here once
> - Date format "ISO" — specify the precise format including timezone handling
> - Base URL "TBD" without an `[TBD]` marker and a Needed By note in Open Questions
>
> **Ask triggers — stop and ask the human if:**
> - Tech Spec is ambiguous on versioning (says "v1" but unclear if it's URL or header)
> - Pagination shape isn't fully specified in Tech Spec (e.g., cursor format is unclear — opaque string? base64-encoded?)
> - Multiple auth methods are needed (e.g., JWT for user endpoints + API key for service-to-service) — clarify how each is identified in headers and which endpoints use which
> - Response envelope is contested ("do we use `{ data: ... }` or return the resource at the top level?") — this is the single most important convention; lock it before moving on
>
> **Cross-reference checklist:**
> - Authentication convention matches Tech Spec Auth section exactly
> - Pagination matches Tech Spec Performance Considerations → Pagination
> - Every Base URL matches Tech Spec Deployment & Environments
> - Versioning strategy matches Tech Spec API Endpoints → Versioning
>
> **Remove this block before delivering the filled doc.**

> Applies to all endpoints unless explicitly overridden in a specific contract.

### Base URL

| Environment | Base URL |
|-------------|----------|
| Development | `http://localhost:[PORT]/api` |
| Staging | `https://staging.[domain]/api` |
| Production | `https://[domain]/api` |

### Versioning

- **Strategy:** [URL path versioning (`/v1/`) / Header versioning (`API-Version: 1`) / No versioning yet]
- **Current version:** `v1`
- **Example:** `POST https://[domain]/api/v1/[resource]`

> See Versioning & Deprecation section for how version changes are managed.

### Authentication

- **Method:** [Pulled from Tech Spec Auth — JWT Bearer / Session cookie / API key / OAuth / etc.]
- **Header:** [Exact header format — e.g., `Authorization: Bearer <token>`]
- **Applies to:** All endpoints marked `Auth: Required`
- **Missing token:** Returns `401 Unauthorized` with `error: UNAUTHORIZED`
- **Invalid / expired token:** Returns `401 Unauthorized` with `error: UNAUTHORIZED` (do not differentiate — never confirm whether a token is well-formed)

### Content Type

- **Requests:** `Content-Type: application/json` (default)
- **Responses:** `Content-Type: application/json` (default)
- **Deviations:** [e.g., "File upload endpoints use `multipart/form-data` — noted per-endpoint where applicable"]

### Date & Time Format

- **Timestamps:** ISO 8601 UTC — `2025-01-15T10:30:00Z`
- **Date-only fields:** `YYYY-MM-DD` — `2025-01-15`
- **Never:** Unix timestamps or locale-formatted strings (unless Tech Spec explicitly requires otherwise — document the exception here if so)

### Pagination

- **Strategy:** [Cursor-based / Offset-based / Per-endpoint (only if pagination strategy varies — usually it shouldn't)]
- **Request params:**

| Param | Type | Default | Description |
|-------|------|---------|-------------|
| `limit` | Integer | 20 | Max records to return. Max allowed: 100. |
| `cursor` | String | — | Opaque cursor from previous response (cursor-based only) |
| `page` | Integer | 1 | Page number (offset-based only) |
| `offset` | Integer | 0 | Record offset (offset-based only) |

- **Response envelope (paginated lists):**

```json
{
  "data": [ ... ],
  "pagination": {
    "total": 84,
    "limit": 20,
    "cursor_next": "eyJpZCI6IjUwIn0=",
    "has_more": true
  }
}
```

> If cursor-based: include `cursor_next` and `has_more`, omit `page` and `offset`. If offset-based: include `page`, `offset`, and `total`, omit `cursor_next`. `total` may be omitted from cursor-based responses if exact counts are expensive — document the decision here.

### Standard Response Envelope

**Single resource (GET by ID, POST, PATCH):**
```json
{
  "data": { ... }
}
```

**List (GET collection):**
```json
{
  "data": [ ... ],
  "pagination": { ... }
}
```

**Empty success (DELETE, some POST/PATCH):** No body. HTTP 204.

> Every endpoint contract below inherits this envelope. Response Field tables document `data.field` paths — the envelope itself is not re-described per endpoint.

---

## Error Codes Reference

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first:** This section MUST be filled before any Endpoint Contract. Every contract's "Error Responses" table references `error` codes from this catalog — contracts do NOT define new error codes inline. If the human tries to write endpoint contracts first, push back with:
> *"Endpoint contracts reference error codes from a shared catalog. If we define codes ad-hoc per endpoint, we'll get inconsistent error handling and the frontend will need a separate handler for every endpoint. Let me build the Error Codes Reference first, then every contract references it."*
>
> **Your job:** Build the canonical catalog of every error code the API can return. Pull from Tech Spec Error Handling (the standard status code table) and Tech Spec State Machines (every guard failure becomes a `409` or `422` error scenario — the codes are named here). Every code listed here is referenced from one or more endpoint contracts below; every error scenario in any endpoint contract maps to a code in this catalog.
>
> **A complete Error Codes Reference covers:**
> - **Error Response Format** — the JSON shape every error returns. Locked here, referenced everywhere.
> - **HTTP Status Code Reference** — the table of HTTP codes used by this app, with semantic meaning. Pull from Tech Spec Error Handling. Add codes Tech Spec didn't list but State Machines / business rules require (e.g., `422` for guard failures).
> - **Application Error Codes table** — every machine-readable `error` value the API can return. One row per code. Include HTTP status, description, and common causes.
>
> **Incomplete looks like:**
> - An error code referenced in any endpoint contract but missing from this catalog
> - A `409` or `422` listed without explaining what triggers it (state machine guard? unique constraint? business rule?)
> - A code with a vague description ("Something went wrong") — name the category
> - Missing `details` field documentation — `details` is an array of `{ field, issue }` objects for validation; document its shape explicitly
> - Codes from training data that aren't actually used by this app — only include codes the app emits
>
> **Ask triggers — stop and ask the human if:**
> - Tech Spec doesn't specify whether to use `400` vs. `422` for business rule violations (both are valid — pick one and document)
> - Whether to differentiate "not found" from "forbidden" (best practice: return `404` for both to avoid leaking resource existence — confirm this is the policy)
> - Tech Spec lists rate limiting but doesn't specify the response shape — confirm `429` with `Retry-After` header
> - The app needs codes for partial success (`207 Multi-Status`) — rare; confirm before adding
>
> **Cross-reference checklist:**
> - Every Tech Spec Error Handling status code appears in the HTTP table
> - Every Tech Spec State Machine guard failure has a corresponding application error code (typically `CONFLICT` for state conflicts, `BUSINESS_RULE_VIOLATION` for guard failures on business rules)
> - Every code in the table is referenced by at least one endpoint contract below (verify at Gate 2)
>
> **Remove this block before delivering the filled doc.**

> Global catalog. Every error this API can return is defined here. Endpoint contracts reference these codes — they don't re-define them.

### Error Response Format

Every error response uses this exact shape:

```json
{
  "status": 400,
  "error": "VALIDATION_ERROR",
  "message": "One or more fields are invalid.",
  "details": [
    { "field": "email", "issue": "already in use" },
    { "field": "name", "issue": "is required" }
  ]
}
```

**Field semantics:**
- `status` — HTTP status code as an integer (matches the response's HTTP status; redundant but useful for clients that lose status info)
- `error` — Machine-readable error code from the Application Error Codes table below. Client code branches on this value.
- `message` — Human-readable, user-safe. Suitable for display in the UI without translation by the client.
- `details` — Always an array of `{ field, issue }` objects. Empty array `[]` for non-validation errors. Populated for `VALIDATION_ERROR` and `BUSINESS_RULE_VIOLATION` with field-specific issues.

### HTTP Status Code Reference

| Status | Meaning | When to use |
|--------|---------|-------------|
| 200 | OK | Successful GET, PATCH |
| 201 | Created | Successful POST that creates a resource |
| 204 | No Content | Successful DELETE — no body returned |
| 400 | Bad Request | Validation failed, malformed request body |
| 401 | Unauthorized | Not authenticated — missing, invalid, or expired credential |
| 403 | Forbidden | Authenticated but not permitted to perform this action |
| 404 | Not Found | Resource doesn't exist (or isn't visible to this user — see policy below) |
| 409 | Conflict | Duplicate unique field, state conflict, optimistic lock failure |
| 422 | Unprocessable Entity | Request is valid JSON but fails a business rule or state machine guard |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Unexpected server error — never expose internal details |

> **404 vs. 403 policy:** When a resource exists but belongs to a different user, return `404` (not `403`). This avoids leaking resource existence to unauthorized users. Document any deviation per-endpoint.

### Application Error Codes

> Machine-readable `error` field values. Every endpoint contract's error rows reference one of these codes.

| Error Code | HTTP Status | Description | Common Causes |
|------------|-------------|-------------|---------------|
| `VALIDATION_ERROR` | 400 | One or more request fields failed validation | Missing required field, wrong type, exceeds max length, invalid format |
| `MALFORMED_REQUEST` | 400 | Request body is not valid JSON | Syntax error, wrong Content-Type, empty body when required |
| `UNAUTHORIZED` | 401 | Authentication required or credential invalid | Missing Authorization header, expired JWT, revoked token |
| `FORBIDDEN` | 403 | Authenticated but action not permitted | Wrong role, attempting privileged action without permission |
| `NOT_FOUND` | 404 | Resource does not exist or is not visible to this user | Wrong ID, deleted resource, resource belongs to different user |
| `CONFLICT` | 409 | Unique constraint violation or state conflict | Duplicate email, duplicate name within scope, optimistic lock version mismatch |
| `BUSINESS_RULE_VIOLATION` | 422 | Request is valid but breaks a business rule or state machine guard | Deleting a completed task, exceeding plan limits, transition not allowed from current state |
| `RATE_LIMITED` | 429 | Too many requests within the rate-limit window | Exceeds per-IP or per-user rate limit |
| `INTERNAL_ERROR` | 500 | Unexpected server error | Unhandled exception — details never exposed to client |

> Add rows only when introducing a new error type the app actually emits. Do not include speculative codes.

---

## 🚦 Gate 1 — Foundation Lock

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off.**
>
> This gate matters because every endpoint contract below assumes the foundation is settled: the response envelope, the auth convention, the date format, the pagination shape, and the error code catalog. A change to any of these after endpoint contracts are written triggers a rewrite of every contract. Get this right once.
>
> Run every check carefully. Open Tech Spec + Schema and verify — do not check from memory.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**

*Conventions completeness*
- [ ] Every environment in Tech Spec Deployment & Environments has a Base URL here (or an `[TBD]` marker with an Open Question)
- [ ] Versioning strategy is locked and matches Tech Spec
- [ ] Authentication method, header format, and missing/invalid behavior match Tech Spec Auth section exactly
- [ ] Date format is explicit (ISO 8601 UTC for timestamps, `YYYY-MM-DD` for date-only) — no "ISO" without specifics
- [ ] Pagination strategy is chosen (cursor or offset) and the response envelope is fully specified
- [ ] Standard response envelope is locked — single resource, list with pagination, and 204 empty
- [ ] Content-Type defaults and any deviations (e.g., multipart for uploads) are documented

*Error Codes Reference completeness*
- [ ] Error Response Format is locked with field semantics for `status`, `error`, `message`, `details`
- [ ] HTTP Status Code Reference covers every status the app will return — at minimum 200, 201, 204, 400, 401, 403, 404, 409, 422, 429, 500 — with semantic meaning
- [ ] Application Error Codes table includes one row for every machine-readable `error` value the app emits
- [ ] Every Tech Spec State Machine guard failure has a corresponding error code in the table (typically `CONFLICT` or `BUSINESS_RULE_VIOLATION`)
- [ ] 404 vs. 403 policy is documented (default: return 404 to avoid leaking resource existence)

**Sign-off:**
> 🚦 **Gate 1** — API conventions and error catalog are locked. Every endpoint contract below inherits these without redefinition. Ready to write per-endpoint contracts.
>
> **Human sign-off:** ☐ Approved — proceed to Endpoint Contracts

---

## Endpoint Contracts

> 🤖 **AGENT INSTRUCTIONS**
>
> **Strict ordering rule — read first:** Endpoint contracts come after Gate 1 sign-off. They are written per resource. Within this section: Authentication resource is first (every other resource depends on it). Then other resources in the order Tech Spec lists them. If the human tries to draft endpoint contracts before Gate 1 sign-off, push back with:
> *"Endpoint contracts assume the conventions and error codes are locked. Let me get Gate 1 signed off first — otherwise we may have to rewrite every contract if a convention changes."*
>
> **Your job — write one full contract block per endpoint listed in Tech Spec API Endpoints.** Each contract is the complete, field-level spec a coding agent needs to build the route handler, the request validator, and the typed client — without re-reading Tech Spec or Schema.
>
> **Per-resource structure (every resource):**
> 1. Resource header with one-sentence description
> 2. Summary Table — pulled from Tech Spec, lists every endpoint in this resource. Quick scan reference.
> 3. Owning Module — the Module ID from Module Breakdown that owns this resource's endpoints. If endpoints in this resource are owned by multiple modules, list each owner per-endpoint in the contract block instead.
> 4. One full contract block per endpoint
>
> **Per-endpoint contract block (the structure is fixed — do not omit sub-sections):**
>
> 1. **Endpoint heading** — `### METHOD /path` (e.g., `### POST /api/v1/projects`)
> 2. **Owning Module** — `M-XX` from Module Breakdown. Bidirectional link — the module's Detail Block must list this endpoint in its API Endpoints Involved table.
> 3. **Auth + Success Code** — `Auth: Required / Public` + the success HTTP status from Tech Spec
> 4. **Path Parameters** table (if any path params) — name, type, description, example
> 5. **Query Parameters** table (if applicable — GETs with filtering/pagination) — name, type, required, default, description
> 6. **Request Fields** table (POST, PATCH, PUT) — field, type, required, validation rules, example. Validation column is the actual rules from Tech Spec, not generic.
> 7. **Example Request** — fenced code block with HTTP verb, path, headers, body. Realistic data.
> 8. **Response Fields** table — `data.field` paths with type, nullability, description, example. Pull types and nullability from Schema's Data Dictionary. List endpoints add `data[].field` rows plus pagination rows.
> 9. **Example Response** — fenced code block with HTTP status line and JSON body. Realistic data.
> 10. **Error Responses** table — every error scenario this endpoint can return. Columns: Scenario, Status, Error Code, Details. Pull from: Tech Spec validation rules (`VALIDATION_ERROR` rows), Tech Spec auth requirements (`UNAUTHORIZED`, `FORBIDDEN`), Tech Spec State Machine guards (`CONFLICT` / `BUSINESS_RULE_VIOLATION`), Tech Spec business rules. **Every error scenario from Tech Spec for this endpoint must appear.**
> 11. **Notes** — non-obvious behavior, side effects, idempotency, cascade rules, system-set fields, anything that affects client behavior. Reference Tech Spec sections where applicable (e.g., "Triggers `[Entity] → submitted` transition per Tech Spec State Machines § [Entity]" — and the side effects of that transition fire per Tech Spec Event Map).
>
> **Realistic example data — what "realistic" means:**
> - UUIDs look like UUIDs (`550e8400-e29b-41d4-a716-446655440000`), not `"id-123"`
> - Emails look like real emails (`ryan@example.com`), not `"string"`
> - Names look like real names for the domain (`"Q4 Planning"` for a project, `"Patient Intake Form"` for a clinical app)
> - Timestamps are ISO 8601 UTC with realistic values, not `"now"`
> - Enum values are actual values from Schema, not `"some_status"`
> - If the resource has an obvious domain example from the PDD or Schema, use it
>
> **Validation rule precision — what "actual rules" means:**
> - For strings: `required` (yes/no), `max length` if constrained, `format` (email, URL, slug, etc.), `pattern` if regex-constrained
> - For enums: list all valid values explicitly — `must be one of: active, archived, pending`
> - For numbers: `min` / `max` if constrained, `integer` vs. float
> - For dates: format (ISO 8601 vs. date-only), range if constrained
> - For relationships: FK target entity, whether the FK must exist
> - For uniqueness: scope of uniqueness (`unique`, `unique per user`, `unique per project`)
>
> **Error Responses — what must appear (cross-check against Tech Spec for every endpoint):**
> - Every validation rule above generates at least one `VALIDATION_ERROR` row
> - If `Auth: Required`, an `UNAUTHORIZED` row for missing/invalid credential
> - If the endpoint is permission-gated (not just authenticated), a `FORBIDDEN` row for wrong role/ownership
> - If the endpoint targets a resource by ID, a `NOT_FOUND` row
> - If the endpoint has a unique constraint that can fail, a `CONFLICT` row
> - If the endpoint triggers a State Machine transition, a `CONFLICT` or `BUSINESS_RULE_VIOLATION` row for each guard failure
> - If the endpoint has business rules (Tech Spec State Machines or PDD User Workflows error paths), a `BUSINESS_RULE_VIOLATION` row per rule
> - If the endpoint is rate-limited, a `RATE_LIMITED` row
>
> **Cross-reference checklist (per contract block):**
> - Endpoint method + path appears verbatim in Tech Spec API Endpoints
> - Owning Module ID appears in Module Breakdown Registry
> - The module's Detail Block lists this endpoint in API Endpoints Involved (bidirectional)
> - Every request field's validation rules trace to Tech Spec validation rules + Schema constraints
> - Every response field's type and nullability trace to Schema Data Dictionary
> - Every State Machine transition referenced in Notes appears in Tech Spec State Machines
> - Every side effect referenced in Notes appears in Tech Spec Event Map
>
> **Ask triggers — stop and ask the human if:**
> - Tech Spec lists an endpoint but doesn't specify the request/response shape clearly enough to write the contract
> - An endpoint implies a State Machine transition but the guard or side effects aren't fully specified
> - The same endpoint appears with conflicting specs across Tech Spec sections (e.g., listed in API Endpoints with one validation, referenced in State Machines with a different one) — flag the contradiction
> - A field's nullability isn't determinable from Schema (some Schema docs are incomplete on this) — ask
> - Two endpoints overlap (e.g., `POST /projects/:id/archive` and `PATCH /projects/:id` both can set `status: archived`) — confirm both should exist and document the canonical path
> - The endpoint requires PATCH vs. PUT semantics and Tech Spec isn't explicit
>
> **Remove this block before delivering the filled doc.**

> Grouped by resource. Each resource has a summary table (pulled from Tech Spec) followed by a full contract block per endpoint.
>
> **How to use:** The summary table is the quick reference. The contract blocks are the implementation spec. If they conflict, the contract block wins — update the summary table to match.
>
> **Start here:** Authentication is always the first resource. Every other resource depends on it.

---

### Resource: Authentication

> 🤖 **AGENT INSTRUCTIONS — READ BEFORE WRITING THIS BLOCK**
>
> **The Authentication block in this template is illustrative.** It assumes a JWT-with-optional-refresh-token model and email + password login — common but not universal. **Do not ship the example as-is.** Open Tech Spec → Authentication & Authorization and rewrite this entire block to match the project's actual auth scheme.
>
> **Decision tree:**
> - **Email + password + JWT (with or without refresh tokens):** The example below is a reasonable starting point. Verify every field and error against Tech Spec, then keep what matches and rewrite what doesn't.
> - **OAuth / SSO / magic links / SAML:** Delete the example endpoints below. The auth flow has different endpoints — typically `/auth/initiate`, `/auth/callback`, possibly `/auth/token-exchange`. Rewrite based on Tech Spec's login flow steps.
> - **Session cookies (no JWT):** Replace the `access_token` response shape with a `Set-Cookie` header and document session lifetime. The login flow remains but the response shape changes.
> - **API keys only (no user login):** Delete this entire resource. There is no `/auth/*` surface. Document API key issuance in the Versioning section or under operations.
> - **No auth at all (rare — internal tool, fully public):** Delete this entire resource. Mark Auth in API Conventions as "None — every endpoint is Public."
>
> **What to keep regardless of auth scheme:**
> - The principle that login and logout (or their equivalents) are explicitly contracted, not implied
> - The principle that token/credential responses never reveal information about user existence (same error for "no such email" and "wrong password")
> - The principle that `Auth: Required` endpoints fail with `UNAUTHORIZED` on missing/invalid credentials
>
> **Remove this entire 🤖 block before delivering. Also remove the human-facing ⚠️ illustrative-warning block below if the project's auth scheme is known and the example has been adapted or replaced.**

> Handles credential issuance, validation, and invalidation. All other protected endpoints depend on a valid credential obtained here.

> ⚠️ **The Authentication block below is illustrative.** Replace with the project's actual auth scheme per Tech Spec Authentication & Authorization. Do not ship the example as-is.

**Owning Module:** [M-XX from Module Breakdown — typically the Auth or Infrastructure module]

#### Summary Table

| Method | Path | Auth | Success Code | Description |
|--------|------|------|--------------|-------------|
| POST | `/api/v1/auth/login` | Public | 200 | Authenticate and receive a token |
| POST | `/api/v1/auth/logout` | Required | 204 | Invalidate the current token |
| POST | `/api/v1/auth/refresh` | Public | 200 | Exchange a refresh token for a new access token |

> Remove `POST /auth/refresh` if the app does not use refresh tokens — force re-login on expiry instead.

---

#### `POST /api/v1/auth/login`

**Owning Module:** [M-XX]
**Auth:** Public
**Success:** 200 OK

**Request Fields**

| Field | Type | Required | Validation | Example |
|-------|------|----------|------------|---------|
| `email` | String | Yes | Valid email format, max 255 chars | `"ryan@example.com"` |
| `password` | String | Yes | Non-empty, min 8 chars | `"hunter2"` |

**Example Request**
```json
POST /api/v1/auth/login
Content-Type: application/json

{
  "email": "ryan@example.com",
  "password": "hunter2"
}
```

**Response Fields**

| Field | Type | Nullable | Description | Example |
|-------|------|----------|-------------|---------|
| `data.access_token` | String | No | JWT to be used in `Authorization: Bearer <token>` header on subsequent requests | `"eyJhbGciOiJIUzI1NiIs..."` |
| `data.refresh_token` | String | Yes | Token used to obtain a new access token without re-login. Omitted if app doesn't use refresh tokens. | `"dGhpcyBpcyBhIHJlZnJlc2..."` |
| `data.expires_in` | Integer | No | Access token lifetime in seconds | `3600` |
| `data.token_type` | String | No | Always `"Bearer"` | `"Bearer"` |

**Example Response — 200 OK**
```json
{
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIs...",
    "refresh_token": "dGhpcyBpcyBhIHJlZnJlc2...",
    "expires_in": 3600,
    "token_type": "Bearer"
  }
}
```

**Error Responses**

| Scenario | Status | Error Code | Details |
|----------|--------|------------|---------|
| `email` missing or empty | 400 | `VALIDATION_ERROR` | `[{ "field": "email", "issue": "is required" }]` |
| `email` not a valid email format | 400 | `VALIDATION_ERROR` | `[{ "field": "email", "issue": "must be a valid email" }]` |
| `password` missing or empty | 400 | `VALIDATION_ERROR` | `[{ "field": "password", "issue": "is required" }]` |
| Email not found OR password incorrect | 401 | `UNAUTHORIZED` | `[]` |
| Account locked / suspended | 403 | `FORBIDDEN` | `[]` (only if Tech Spec defines account states; otherwise omit) |
| Rate limit exceeded | 429 | `RATE_LIMITED` | `[]` (only if Tech Spec defines rate limiting on this endpoint) |

> ⚠️ Return the same 401 for both "email not found" and "wrong password" — never confirm whether an email exists.

**Notes**
- Password is never logged, stored in plaintext, or returned in any response.
- On success, the client stores the `access_token` for `Authorization: Bearer <token>` use. Store `refresh_token` securely — httpOnly cookie is preferred over localStorage to mitigate XSS.
- `refresh_token` is omitted from the response if the app does not use refresh tokens.
- Triggers the `user.signed_in` analytics event per Tech Spec Analytics & Instrumentation (if defined there).

---

#### `POST /api/v1/auth/logout`

**Owning Module:** [M-XX]
**Auth:** Required
**Success:** 204 No Content

> Invalidates the current credential server-side. Client should also discard stored tokens regardless of response.

**Request Fields**

> No request body. The credential to invalidate is identified from the `Authorization` header.

**Example Request**
```
POST /api/v1/auth/logout
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

**Example Response — 204 No Content**
```
(no body)
```

**Error Responses**

| Scenario | Status | Error Code | Details |
|----------|--------|------------|---------|
| Missing or invalid token | 401 | `UNAUTHORIZED` | `[]` |

**Notes**
- If using JWTs without a token blocklist, logout is effectively client-side only — the JWT remains technically valid until expiry. Document this explicitly here if so.
- If using a token blocklist or session store, this endpoint invalidates the server-side record.
- Triggers the `user.signed_out` analytics event per Tech Spec Analytics & Instrumentation (if defined there).

---

#### `POST /api/v1/auth/refresh`

**Owning Module:** [M-XX]
**Auth:** Public (uses refresh token in body, not access token in header)
**Success:** 200 OK

> Remove this endpoint entirely if the app forces re-login on access token expiry.

**Request Fields**

| Field | Type | Required | Validation | Example |
|-------|------|----------|------------|---------|
| `refresh_token` | String | Yes | Non-empty | `"dGhpcyBpcyBhIHJlZnJlc2..."` |

**Example Request**
```json
POST /api/v1/auth/refresh
Content-Type: application/json

{
  "refresh_token": "dGhpcyBpcyBhIHJlZnJlc2..."
}
```

**Response Fields**

| Field | Type | Nullable | Description | Example |
|-------|------|----------|-------------|---------|
| `data.access_token` | String | No | New JWT access token | `"eyJhbGciOiJIUzI1NiIs..."` |
| `data.refresh_token` | String | Yes | New refresh token (if single-use refresh policy is enforced) | `"bmV3IHJlZnJlc2ggdG9rZW4..."` |
| `data.expires_in` | Integer | No | Access token lifetime in seconds | `3600` |
| `data.token_type` | String | No | Always `"Bearer"` | `"Bearer"` |

**Example Response — 200 OK**
```json
{
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIs...",
    "refresh_token": "bmV3IHJlZnJlc2ggdG9rZW4...",
    "expires_in": 3600,
    "token_type": "Bearer"
  }
}
```

**Error Responses**

| Scenario | Status | Error Code | Details |
|----------|--------|------------|---------|
| `refresh_token` missing | 400 | `VALIDATION_ERROR` | `[{ "field": "refresh_token", "issue": "is required" }]` |
| Refresh token invalid, expired, or already used | 401 | `UNAUTHORIZED` | `[]` |

**Notes**
- Refresh tokens should be single-use if possible — issue a new refresh token alongside each new access token and invalidate the old one. Document the policy in Tech Spec Authentication & Authorization.
- On refresh token failure, the client must redirect to login.

---

### Resource: [Resource Name]

> One sentence describing what this resource represents. Example: "A `Project` belongs to a `User` and contains `Tasks`. Projects have a lifecycle: `active` → `archived`."

**Owning Module:** [M-XX from Module Breakdown — if all endpoints in this resource share an owner; otherwise note "Per-endpoint — see contract blocks below"]

#### Summary Table

> Pulled from Tech Spec API Endpoints. Update here if a contract block changes a method, path, auth, or success code — keep summary and contract blocks in sync. Contract blocks are authoritative if they conflict.

| Method | Path | Auth | Success Code | Description |
|--------|------|------|--------------|-------------|
| POST | `/api/v1/[resource]` | Required | 201 | Create a [resource] |
| GET | `/api/v1/[resource]` | Required | 200 | List [resource]s for the authenticated user |
| GET | `/api/v1/[resource]/:id` | Required | 200 | Get a single [resource] by ID |
| PATCH | `/api/v1/[resource]/:id` | Required | 200 | Update a [resource] |
| DELETE | `/api/v1/[resource]/:id` | Required | 204 | Delete a [resource] |

---

#### `POST /api/v1/[resource]`

**Owning Module:** [M-XX]
**Auth:** Required
**Success:** 201 Created

**Request Fields**

| Field | Type | Required | Validation | Example |
|-------|------|----------|------------|---------|
| `name` | String | Yes | Non-empty, max 255 chars, unique per user | `"Q4 Planning"` |
| `description` | String | No | Max 1000 chars | `"Planning session for Q4 goals"` |
| `status` | Enum | No | One of: `active`, `archived`. Default: `active`. | `"active"` |

**Example Request**
```json
POST /api/v1/[resource]
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
Content-Type: application/json

{
  "name": "Q4 Planning",
  "description": "Planning session for Q4 goals",
  "status": "active"
}
```

**Response Fields**

| Field | Type | Nullable | Description | Example |
|-------|------|----------|-------------|---------|
| `data.id` | UUID | No | Server-assigned unique identifier | `"550e8400-e29b-41d4-a716-446655440000"` |
| `data.name` | String | No | As submitted | `"Q4 Planning"` |
| `data.description` | String | Yes | As submitted (or null if not provided) | `"Planning session for Q4 goals"` |
| `data.status` | Enum | No | As submitted (or default `"active"`) | `"active"` |
| `data.created_at` | Timestamp | No | Server-set on creation, ISO 8601 UTC | `"2025-01-15T10:30:00Z"` |
| `data.updated_at` | Timestamp | No | Server-set on creation, equal to `created_at` | `"2025-01-15T10:30:00Z"` |

**Example Response — 201 Created**
```json
{
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Q4 Planning",
    "description": "Planning session for Q4 goals",
    "status": "active",
    "created_at": "2025-01-15T10:30:00Z",
    "updated_at": "2025-01-15T10:30:00Z"
  }
}
```

**Error Responses**

| Scenario | Status | Error Code | Details |
|----------|--------|------------|---------|
| `name` missing or empty | 400 | `VALIDATION_ERROR` | `[{ "field": "name", "issue": "is required" }]` |
| `name` exceeds max length | 400 | `VALIDATION_ERROR` | `[{ "field": "name", "issue": "must be 255 characters or fewer" }]` |
| `name` already exists for this user | 409 | `CONFLICT` | `[{ "field": "name", "issue": "already in use" }]` |
| `description` exceeds max length | 400 | `VALIDATION_ERROR` | `[{ "field": "description", "issue": "must be 1000 characters or fewer" }]` |
| `status` is not a valid enum value | 400 | `VALIDATION_ERROR` | `[{ "field": "status", "issue": "must be one of: active, archived" }]` |
| Not authenticated | 401 | `UNAUTHORIZED` | `[]` |
| User has reached plan limit for resources | 422 | `BUSINESS_RULE_VIOLATION` | `[{ "field": "—", "issue": "plan limit of 10 resources reached" }]` (only if Tech Spec defines plan limits) |

**Notes**
- `id`, `created_at`, and `updated_at` are server-set and ignored if included in the request body.
- The created resource is associated with the authenticated user (`user_id` is server-set, not in the request).
- [If this creation triggers a State Machine transition or fires a side effect, document it here referencing Tech Spec State Machines § [Entity] and Tech Spec Event Map row [X].]

---

#### `GET /api/v1/[resource]`

**Owning Module:** [M-XX]
**Auth:** Required
**Success:** 200 OK

**Query Parameters**

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `status` | Enum | No | — (no filter) | Filter by status. One of: `active`, `archived` |
| `q` | String | No | — | Free-text search across `name` and `description` |
| `limit` | Integer | No | 20 | Max records per page. Max allowed: 100. |
| `cursor` | String | No | — | Pagination cursor from a previous response |

**Example Request**
```
GET /api/v1/[resource]?status=active&limit=20
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

**Response Fields**

| Field | Type | Nullable | Description | Example |
|-------|------|----------|-------------|---------|
| `data[].id` | UUID | No | Resource ID | `"550e8400-e29b-41d4-a716-446655440000"` |
| `data[].name` | String | No | Resource name | `"Q4 Planning"` |
| `data[].description` | String | Yes | Resource description | `"Planning session for Q4 goals"` |
| `data[].status` | Enum | No | Current status | `"active"` |
| `data[].created_at` | Timestamp | No | ISO 8601 UTC | `"2025-01-15T10:30:00Z"` |
| `data[].updated_at` | Timestamp | No | ISO 8601 UTC | `"2025-01-15T10:30:00Z"` |
| `pagination.total` | Integer | No | Total matching records | `42` |
| `pagination.limit` | Integer | No | Applied limit | `20` |
| `pagination.has_more` | Boolean | No | True if more pages exist | `true` |
| `pagination.cursor_next` | String | Yes | Cursor for next page. Null if no more pages. | `"eyJpZCI6IjUwIn0="` |

**Example Response — 200 OK**
```json
{
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Q4 Planning",
      "description": "Planning session for Q4 goals",
      "status": "active",
      "created_at": "2025-01-15T10:30:00Z",
      "updated_at": "2025-01-15T10:30:00Z"
    }
  ],
  "pagination": {
    "total": 42,
    "limit": 20,
    "has_more": true,
    "cursor_next": "eyJpZCI6IjUwIn0="
  }
}
```

**Error Responses**

| Scenario | Status | Error Code | Details |
|----------|--------|------------|---------|
| Not authenticated | 401 | `UNAUTHORIZED` | `[]` |
| `status` is not a valid enum value | 400 | `VALIDATION_ERROR` | `[{ "field": "status", "issue": "must be one of: active, archived" }]` |
| `limit` exceeds max | 400 | `VALIDATION_ERROR` | `[{ "field": "limit", "issue": "must be 100 or fewer" }]` |
| `cursor` malformed or expired | 400 | `VALIDATION_ERROR` | `[{ "field": "cursor", "issue": "invalid pagination cursor" }]` |

**Notes**
- Returns only records belonging to the authenticated user (server-side filter; no `user_id` query param needed or accepted).
- Soft-deleted records (if soft delete is used — see DELETE contract) are excluded.
- Sort order: [pulled from Tech Spec — typically `created_at DESC` unless specified otherwise].
- Empty result returns `200` with `data: []` and `pagination.total: 0` — not `404`.

---

#### `GET /api/v1/[resource]/:id`

**Owning Module:** [M-XX]
**Auth:** Required
**Success:** 200 OK

**Path Parameters**

| Param | Type | Description |
|-------|------|-------------|
| `id` | UUID | ID of the resource to retrieve |

**Example Request**
```
GET /api/v1/[resource]/550e8400-e29b-41d4-a716-446655440000
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

**Response Fields**

| Field | Type | Nullable | Description | Example |
|-------|------|----------|-------------|---------|
| `data.id` | UUID | No | Resource ID | `"550e8400-e29b-41d4-a716-446655440000"` |
| `data.name` | String | No | Resource name | `"Q4 Planning"` |
| `data.description` | String | Yes | Resource description | `"Planning session for Q4 goals"` |
| `data.status` | Enum | No | Current status | `"active"` |
| `data.created_at` | Timestamp | No | ISO 8601 UTC | `"2025-01-15T10:30:00Z"` |
| `data.updated_at` | Timestamp | No | ISO 8601 UTC | `"2025-01-15T10:30:00Z"` |

**Example Response — 200 OK**
```json
{
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Q4 Planning",
    "description": "Planning session for Q4 goals",
    "status": "active",
    "created_at": "2025-01-15T10:30:00Z",
    "updated_at": "2025-01-15T10:30:00Z"
  }
}
```

**Error Responses**

| Scenario | Status | Error Code | Details |
|----------|--------|------------|---------|
| `id` not a valid UUID | 400 | `VALIDATION_ERROR` | `[{ "field": "id", "issue": "must be a valid UUID" }]` |
| Not authenticated | 401 | `UNAUTHORIZED` | `[]` |
| Resource not found OR belongs to a different user | 404 | `NOT_FOUND` | `[]` |

**Notes**
- Returns 404 (not 403) when the resource exists but belongs to a different user. This is the global policy per Error Codes Reference § 404 vs. 403.

---

#### `PATCH /api/v1/[resource]/:id`

**Owning Module:** [M-XX]
**Auth:** Required
**Success:** 200 OK

> Partial update. Only send fields you want to change. Omitted fields are unchanged. Send `null` to explicitly clear a nullable field.

**Path Parameters**

| Param | Type | Description |
|-------|------|-------------|
| `id` | UUID | ID of the resource to update |

**Request Fields**

| Field | Type | Required | Validation | Example |
|-------|------|----------|------------|---------|
| `name` | String | No | Non-empty if provided, max 255 chars, unique per user | `"Updated Name"` |
| `description` | String / null | No | Max 1000 chars. Send `null` to clear. | `"Updated description"` |
| `status` | Enum | No | One of: `active`, `archived`. State transitions per Tech Spec State Machines § [Entity]. | `"archived"` |

**Example Request**
```json
PATCH /api/v1/[resource]/550e8400-e29b-41d4-a716-446655440000
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
Content-Type: application/json

{
  "status": "archived"
}
```

**Response Fields**

> Returns the full updated resource (same shape as GET by ID).

**Example Response — 200 OK**
```json
{
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Q4 Planning",
    "description": "Planning session for Q4 goals",
    "status": "archived",
    "created_at": "2025-01-15T10:30:00Z",
    "updated_at": "2025-01-20T14:22:15Z"
  }
}
```

**Error Responses**

| Scenario | Status | Error Code | Details |
|----------|--------|------------|---------|
| `id` not a valid UUID | 400 | `VALIDATION_ERROR` | `[{ "field": "id", "issue": "must be a valid UUID" }]` |
| `name` provided but empty | 400 | `VALIDATION_ERROR` | `[{ "field": "name", "issue": "cannot be empty" }]` |
| `name` exceeds max length | 400 | `VALIDATION_ERROR` | `[{ "field": "name", "issue": "must be 255 characters or fewer" }]` |
| `name` conflicts with another resource owned by this user | 409 | `CONFLICT` | `[{ "field": "name", "issue": "already in use" }]` |
| Invalid `status` value | 400 | `VALIDATION_ERROR` | `[{ "field": "status", "issue": "must be one of: active, archived" }]` |
| Status transition not allowed from current state | 422 | `BUSINESS_RULE_VIOLATION` | `[{ "field": "status", "issue": "cannot transition from archived to active" }]` (per Tech Spec State Machines § [Entity]) |
| Not authenticated | 401 | `UNAUTHORIZED` | `[]` |
| Resource not found OR belongs to a different user | 404 | `NOT_FOUND` | `[]` |

**Notes**
- `updated_at` is refreshed on every successful PATCH.
- `id`, `created_at`, and `user_id` cannot be changed. Including them in the request body is ignored (not an error — but document any exception here).
- Status changes trigger the corresponding State Machine transition per Tech Spec State Machines § [Entity]. Side effects from that transition (notifications, audit log, etc.) fire per Tech Spec Event Map.
- [If the resource uses optimistic locking (`version` field), document the lock-version field and its `CONFLICT` behavior here.]

---

#### `DELETE /api/v1/[resource]/:id`

**Owning Module:** [M-XX]
**Auth:** Required
**Success:** 204 No Content

**Path Parameters**

| Param | Type | Description |
|-------|------|-------------|
| `id` | UUID | ID of the resource to delete |

**Example Request**
```
DELETE /api/v1/[resource]/550e8400-e29b-41d4-a716-446655440000
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

**Example Response — 204 No Content**
```
(no body)
```

**Error Responses**

| Scenario | Status | Error Code | Details |
|----------|--------|------------|---------|
| `id` not a valid UUID | 400 | `VALIDATION_ERROR` | `[{ "field": "id", "issue": "must be a valid UUID" }]` |
| Not authenticated | 401 | `UNAUTHORIZED` | `[]` |
| Resource not found OR belongs to a different user | 404 | `NOT_FOUND` | `[]` |
| Business rule prevents deletion (e.g., resource has dependents) | 422 | `BUSINESS_RULE_VIOLATION` | `[{ "field": "—", "issue": "cannot delete resource with active children" }]` (per Tech Spec — replace with the actual rule) |

**Notes**
- **Delete strategy:** [Soft delete — sets `deleted_at` timestamp / Hard delete — row removed] — pulled from Tech Spec or DB Schema.
- **Cascade behavior:** [What happens to child records — cascade-delete, set-null, or block deletion]. Reference DB Schema Relationships § [Entity].
- **Idempotency:** Deleting an already-deleted resource returns `204` (idempotent) OR `404` (strict) — pick one and document the choice consistently across all DELETE endpoints.
- [If deletion triggers a State Machine transition (e.g., `→ cancelled`) instead of removing the row, document it here referencing Tech Spec State Machines.]

---

### Resource: [Resource Name]

*(Copy the full resource block above — Resource header + Owning Module + Summary Table + one contract block per endpoint. Repeat for every resource in Tech Spec.)*

---

## WebSocket Contracts

> 🤖 **AGENT INSTRUCTIONS**
>
> **Skip this entire section unless Tech Spec → Events & Side Effects → Real-Time Events is non-empty.** If the app has no real-time features, mark this section as N/A in the Status table and add this note: `N/A — Tech Spec Real-Time Events section is empty (no WebSocket/SSE in this app)`. Do not delete the section heading — the absence of the section is meaningful (signals "this was considered and intentionally not used").
>
> **Your job (if WebSocket Contracts apply):** Translate the Tech Spec Real-Time Events table into full event contracts. Every row in Tech Spec Real-Time Events becomes one event contract here.
>
> **A complete WebSocket Contracts section covers:**
> - **Overview** — connection URL, auth mechanism (matches Tech Spec Real-Time Events → Auth), event envelope shape, heartbeat behavior
> - **One contract block per event** — direction (Client→Server, Server→Client, both), payload fields with types and nullability, example, error handling
> - **Connection Error Events** — system-level events (auth failure, disconnect, error)
> - **Reconnect behavior** — what happens on disconnect/reconnect, whether missed events are replayed
>
> **Per-event contract block must cover:**
> - Direction
> - When the event fires (server-side trigger) or when the client sends it
> - Payload field table — same precision as REST endpoint fields
> - Example payload — realistic data, not `"field": "value"`
> - Error handling — what happens if payload is invalid, what server returns on error
>
> **Cross-reference checklist:**
> - Every event listed here appears in Tech Spec Real-Time Events
> - Connection auth mechanism matches Tech Spec Real-Time Events → Auth
> - Server-emitted events fire from State Machine transitions or Event Map rows — name the trigger
>
> **Ask triggers — stop and ask the human if:**
> - Tech Spec lists an event but doesn't specify the payload shape
> - Auth mechanism is unclear (token in query param? auth message on connect? cookie?)
> - Whether missed events are replayed on reconnect is unspecified — this is a critical UX decision
>
> **Remove this block before delivering the filled doc.**

> N/A unless Tech Spec → Events & Side Effects → Real-Time Events is non-empty. If N/A, leave only the N/A note below and remove the rest of this section's content.

### Overview

- **Protocol:** WebSocket (`wss://` in production, `ws://` in dev only)
- **Endpoint:** `wss://[domain]/ws`
- **Auth:** [Pulled from Tech Spec Real-Time Events → Auth — e.g., "Token passed as query param `?token=<jwt>`" or "Auth message sent immediately after connect"]
- **Connection lifecycle:** [How connections are established, maintained, and closed. Specify timeout behavior.]
- **Heartbeat / ping-pong:** [Interval and behavior — e.g., "Server sends ping every 30s; client must respond with pong within 10s or be disconnected" — or "None"]

### Event Envelope

All messages are JSON with this envelope:

**Client → Server:**
```json
{
  "event": "event_name",
  "payload": { ... }
}
```

**Server → Client:**
```json
{
  "event": "event_name",
  "payload": { ... },
  "timestamp": "2025-01-15T10:30:00Z"
}
```

> `event` is the event name (snake_case, namespaced by domain — e.g., `task:updated`). `payload` is event-specific. Server messages include `timestamp` (ISO 8601 UTC).

---

### Event: `[event:name]`

**Direction:** [Client → Server / Server → Client / Both]
**Description:** [What this event means and when it fires — reference Tech Spec Real-Time Events row]

**Payload Fields**

| Field | Type | Nullable | Description | Example |
|-------|------|----------|-------------|---------|
| `[field]` | String | No | [What it is] | `"value"` |
| `[field]` | UUID | No | [What it is] | `"550e8400-..."` |

**Example**
```json
{
  "event": "[event:name]",
  "payload": {
    "[field]": "value"
  },
  "timestamp": "2025-01-15T10:30:00Z"
}
```

**Trigger (for Server → Client events):** [What server-side action causes this — typically a State Machine transition or an Event Map row]
**Recipient (for Server → Client events):** [Who receives it — specific user, room, or broadcast]
**Error Handling:** [What happens if the client sends an invalid payload — does the server send an `error` event back? Disconnect?]

---

### Event: `[event:name]`

*(Repeat block for each event in Tech Spec Real-Time Events)*

---

### Connection Error Events

> System-level events that aren't tied to a specific business event. The server emits these to communicate connection state.

| Event | Trigger | Payload | Example |
|-------|---------|---------|---------|
| `error` | Invalid payload, auth failure, server error | `{ "code": "...", "message": "..." }` | `{ "code": "INVALID_PAYLOAD", "message": "payload.task_id is required" }` |
| `disconnect` | Server-initiated close | `{ "reason": "..." }` | `{ "reason": "auth_expired" }` |

### Reconnect Behavior

- **Disconnect detection:** [Client-side — heartbeat timeout / explicit disconnect event]
- **Reconnect strategy:** [Exponential backoff — starting interval and max]
- **Missed event replay:** [Yes — server replays events since `last_event_id` / No — client polls the relevant REST endpoint to resync state]

---

## Webhook Contracts

> 🤖 **AGENT INSTRUCTIONS**
>
> **Skip this entire section unless Tech Spec → Events & Side Effects → Delivery Methods includes Outbound Webhook.** If the app does not send outbound webhooks, mark this section as N/A in the Status table and add this note: `N/A — Tech Spec does not list Outbound Webhook as a delivery method`. Do not delete the section heading — the absence is meaningful.
>
> **Your job (if Webhook Contracts apply):** Define the outbound webhook system — overall delivery contract, signature verification, retry policy, payload envelope — and one event block per webhook event the app sends.
>
> **A complete Webhook Contracts section covers:**
> - **Overview** — what webhooks are in this system, how consumers register, auth/signature scheme, delivery guarantee, retry policy, timeout, dedup
> - **Payload envelope** — standard shape for every webhook payload (event_id, event, timestamp, data)
> - **Signature verification** — exact algorithm and header name, with verification pseudocode
> - **One event block per webhook event** — trigger, payload, expected consumer response
>
> **Cross-reference checklist:**
> - Every webhook event listed here appears in Tech Spec Event Map with Delivery Method = "Outbound Webhook"
> - Retry policy matches Tech Spec Event Map "On Failure" for webhook rows
> - Signature scheme matches Tech Spec Security Considerations (if specified there)
>
> **Ask triggers — stop and ask the human if:**
> - Retry policy is unspecified in Tech Spec — confirm max attempts and backoff
> - Signature algorithm isn't specified — HMAC-SHA256 is the standard default; confirm
> - Whether consumers can configure which events they receive (event filtering on subscription) — this affects the registration flow
>
> **Remove this block before delivering the filled doc.**

> N/A unless Tech Spec → Events & Side Effects → Delivery Methods includes Outbound Webhook. If N/A, leave only the N/A note below and remove the rest of this section's content.

### Overview

- **What they are:** HTTP POST requests sent by [App Name] to a registered URL when certain events occur in the system.
- **Registration:** [How consumers register a webhook URL — API endpoint like `POST /api/v1/webhooks`, dashboard setting, etc.]
- **Auth / Verification:** HMAC-SHA256 signature in `X-Webhook-Signature` header. See Signature Verification below.
- **Delivery guarantee:** [At-least-once — same event may be delivered multiple times; consumers must deduplicate on `event_id` or resource state]
- **Retry policy:** [e.g., "3 retries with exponential backoff: 1 min, 5 min, 30 min. After final failure, delivery is logged and not retried."] — matches Tech Spec Event Map "On Failure"
- **Timeout:** Server waits [X seconds] for a `2xx` response. Anything else (timeout, 4xx, 5xx) is treated as failure.
- **Deduplication:** Every delivery includes an `event_id`. Same event retried = same `event_id`. Consumers should deduplicate on `event_id` to handle retries safely.

### Webhook Payload Envelope

```json
{
  "event_id": "evt_550e8400-e29b-41d4-a716-446655440000",
  "event": "resource.created",
  "timestamp": "2025-01-15T10:30:00Z",
  "data": { ... }
}
```

**Field semantics:**
- `event_id` — Unique per event occurrence. Same event retried = same `event_id`. Use for deduplication.
- `event` — Event type. Format: `[noun].[verb]` (e.g., `task.completed`, `user.deleted`). Matches Tech Spec Event Map row.
- `timestamp` — When the event occurred server-side, ISO 8601 UTC.
- `data` — Event-specific payload, defined per event below.

### Signature Verification

Every webhook includes this header:

```
X-Webhook-Signature: sha256=<HMAC-SHA256(secret, raw_body)>
```

**Verification (consumer side):**
```
expected = "sha256=" + HMAC_SHA256(consumer_secret, raw_request_body)
actual   = request.headers["X-Webhook-Signature"]
if expected != actual: reject with 401
```

> Always verify the signature **before** parsing the body. Reject any webhook that fails verification. The secret is provided to the consumer at registration time and never transmitted afterward.

---

### Event: `[resource].[action]`

> Example event names: `project.created`, `task.status_changed`, `user.deleted`

**Trigger:** [When this fires — typically a State Machine transition or an Event Map row. Reference Tech Spec State Machines § [Entity] or Event Map row.]
**Description:** [What this event means to the consumer]

**Payload (the `data` field)**

| Field | Type | Nullable | Description | Example |
|-------|------|----------|-------------|---------|
| `id` | UUID | No | Resource ID | `"550e8400-e29b-41d4-a716-446655440000"` |
| `[field]` | [Type] | [Y/N] | [Description] | `[example]` |

**Example Full Payload**
```json
{
  "event_id": "evt_550e8400-e29b-41d4-a716-446655440000",
  "event": "[resource].[action]",
  "timestamp": "2025-01-15T10:30:00Z",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "[field]": "[value]"
  }
}
```

**Expected Consumer Response**
- Return `2xx` within [X] seconds to acknowledge delivery
- Any non-2xx response or timeout triggers the retry policy
- Response body is ignored

---

### Event: `[resource].[action]`

*(Repeat block for each webhook event in Tech Spec Event Map where Delivery Method = "Outbound Webhook")*

---

## Versioning & Deprecation

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Lock the rules for evolving this API over time — what counts as a breaking change, how deprecations are communicated, and the version history. The strategy itself was decided in API Conventions; this section codifies the *rules*.
>
> **A complete Versioning & Deprecation section covers:**
> - **Versioning strategy** — restated for clarity (the rule is defined here)
> - **Breaking-change taxonomy** — table classifying each change type as breaking or non-breaking. This is the authority — if a change is on this table as breaking, it requires a new version.
> - **Deprecation process** — explicit steps from "decision to deprecate" to "removal"
> - **Version history** — running log of versions and their status (Active, Deprecated, Sunset)
>
> **Why this is non-optional:** Without an explicit breaking-change taxonomy, the team will accidentally ship a breaking change disguised as a "small fix" and break every API consumer. This table is the gate.
>
> **Ask triggers:**
> - Tech Spec doesn't specify the minimum notice period before a sunset — confirm with human (90 days is common; some apps need longer)
> - Whether the app supports multiple versions in parallel — affects the deprecation process
>
> **Remove this block before delivering the filled doc.**

### Versioning Strategy

- **Current version:** `v1`
- **Version location:** [URL path — `/api/v1/` / Request header — `API-Version: 1`] — locked in API Conventions § Versioning
- **Backward compatibility rule:** Additive changes (new optional fields, new endpoints, new error codes that are subtypes of existing categories) do not require a version bump. Breaking changes (per the table below) require a new major version.

### What Constitutes a Breaking Change

| Change Type | Breaking? | Notes |
|-------------|-----------|-------|
| Adding a new optional request field | No | — |
| Adding a new response field | No | Clients must tolerate unknown fields |
| Adding a new endpoint | No | — |
| Adding a new error code that's a subtype of an existing one | No | Clients should branch on the existing parent code |
| Removing a request field | **Yes** | — |
| Removing a response field | **Yes** | — |
| Changing a field type | **Yes** | E.g., String to Integer |
| Changing a field from optional to required | **Yes** | — |
| Changing a field from nullable to non-nullable | **Yes** | — |
| Changing a default value | **Yes** | Hidden behavior change for clients that omit the field |
| Removing an endpoint | **Yes** | — |
| Changing a success status code | **Yes** | E.g., 200 to 201 |
| Renaming an error code | **Yes** | Clients branch on `error` value |
| Removing an error scenario | No | Clients should handle "no error" gracefully |
| Adding a new required header | **Yes** | — |
| Changing pagination shape | **Yes** | Affects every list endpoint client |
| Changing the response envelope shape | **Yes** | Affects every endpoint client |

### Deprecation Process

1. **Decision to deprecate** — documented in Build Decisions Log with rationale and target sunset date
2. **Flag in this doc** — the deprecated endpoint or field is marked `⚠️ DEPRECATED — sunset [date]`
3. **Response headers** — deprecated endpoints include `Deprecation: true` and `Sunset: <RFC 9745 date>` in every response
4. **Minimum notice period** — [X days/months] between deprecation flagging and sunset
5. **Removal** — happens in the next major version. The old version remains available until the sunset date.
6. **Sunset** — endpoint returns `410 Gone` after the sunset date passes. Version removed from this doc.

### Version History

| Version | Released | Status | Notes |
|---------|----------|--------|-------|
| v1 | [Date] | Active | Initial release |

---

## 🚦 Gate 2 — Full API Contract Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off. This is the final gate before downstream coding-phase docs (Component/Service Layer Map, Testing Strategy) consume this doc, and before backend/frontend coding agents read it directly to generate code.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off. Open Tech Spec, Schema, and Module Breakdown and verify cross-references — do not check from memory.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**

*Document completeness*
- [ ] Status & Next Steps table shows no 🔲 or 🔄 — every section is ✅ Done
- [ ] Gate 1 is signed off
- [ ] Every endpoint in Tech Spec API Endpoints has a full contract block in this doc
- [ ] Every endpoint has an Owning Module ID that exists in Module Breakdown Registry
- [ ] Every Owning Module's Detail Block lists the corresponding endpoint(s) in API Endpoints Involved (bidirectional link verified)

*Per Endpoint Contract — every block must satisfy these*
- [ ] Method + path matches Tech Spec API Endpoints verbatim
- [ ] Auth value (Required / Public) matches Tech Spec
- [ ] Success status code matches Tech Spec
- [ ] Request Fields table lists every field with type, required flag, validation rules, and example
- [ ] Every validation rule traces to Tech Spec validation rules or Schema constraints
- [ ] Response Fields table lists every field with `data.field` path, type, nullability, description, and example
- [ ] Every response field's type and nullability trace to Schema Data Dictionary
- [ ] Example Request is realistic (not `"string"`, not `"field": "value"`)
- [ ] Example Response is realistic and matches the Response Fields shape
- [ ] Error Responses table covers every expected scenario — validation, auth, ownership, state machine guards, business rules, rate limiting (where applicable)
- [ ] Every error scenario from Tech Spec for this endpoint appears in the Error Responses table
- [ ] Every error code referenced exists in the Error Codes Reference catalog
- [ ] Notes section covers non-obvious behavior — side effects, system-set fields, cascade rules, state transitions

*Conventions & Error Catalog consistency*
- [ ] Every endpoint inherits the standard response envelope from API Conventions (no inline redefinition)
- [ ] Every error code used in any contract appears in the Error Codes Reference catalog
- [ ] Every code in the Error Codes Reference catalog is used by at least one endpoint (or explicitly marked as reserved for future use)
- [ ] Date/time fields use ISO 8601 UTC format throughout (or explicit deviation documented per-field)
- [ ] Pagination shape in any list endpoint matches the envelope defined in API Conventions

*WebSocket & Webhook sections*
- [ ] WebSocket Contracts section is either fully filled OR marked N/A with reference to Tech Spec Real-Time Events being empty
- [ ] Webhook Contracts section is either fully filled OR marked N/A with reference to Tech Spec not using Outbound Webhook
- [ ] If filled: every event in Tech Spec Real-Time Events has a contract block here
- [ ] If filled: every webhook event in Tech Spec Event Map (Delivery Method = Outbound Webhook) has a contract block here

*Versioning*
- [ ] Versioning strategy matches API Conventions
- [ ] Breaking-change taxonomy table is filled
- [ ] Deprecation process has explicit steps and notice period
- [ ] Version History has at least the initial version row

*Authentication block sanity check*
- [ ] The Authentication resource block has been adapted to match Tech Spec's actual auth scheme (not shipped as the JWT-and-refresh-token example)
- [ ] The illustrative ⚠️ warning has been removed (assuming the block has been adapted)

*Cross-doc traceability*
- [ ] Every endpoint contract's Owning Module ID maps back to Module Breakdown
- [ ] Every State Machine transition referenced in any contract's Notes appears in Tech Spec State Machines
- [ ] Every side effect referenced in any contract's Notes appears in Tech Spec Event Map
- [ ] Every Tech Spec API endpoint is covered exactly once (no orphan endpoints, no double-contracted endpoints)

*Cleanup*
- [ ] Zero `🤖` in the file
- [ ] Zero `❓ AGENT PAUSE` in the file
- [ ] Zero "Remove this block" in the file
- [ ] Every `🚦 GATE` block contains only the checklist and sign-off — no agent prose
- [ ] No "TBD" left in any contract — open items are resolved or surfaced explicitly

**Sign-off:**
> 🚦 **Gate 2** — API Contract is complete, internally consistent, and fully traces to Tech Spec, Schema, and Module Breakdown. Every endpoint has a contract precise enough for frontend and backend coding agents to build from directly. Ready for downstream Component/Service Layer Map and Testing Strategy.
>
> **Human sign-off:** ☐ Approved — API Contract complete. Proceed to Component/Service Layer Map and Testing Strategy (these can be filled in parallel after this gate).
