# Technical Spec Doc: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized
> **Fill order:** Third design doc. Begin after DB Schema is signed off. Can be filled in parallel with UI/UX after Schema is done.
>
> **Source docs:**
> - `[AppName]_Product_Design_Doc.md` — Core Features, User Workflows, Technical Constraints, Success Metrics
> - `[AppName]_DB_Schema.md` — Entities, Relationships, Constraints
>
> **Agent role:** Translate the PDD's features and the Schema's entities into a complete, machine-executable architecture spec. Every endpoint, transition, event, and config value must trace to either the PDD, the Schema, or a confirmed answer from the human.
>
> **The three rules while filling this doc:**
> 1. Everything written traces to PDD + Schema + confirmed human input. No invented endpoints. No invented transitions. No invented side effects.
> 2. If a feature implies an endpoint but the PDD doesn't specify behavior (e.g., what fields are required? what's the failure mode?), stop and ask.
> 3. Output must be specific enough that the API Contract agent can write the full request/response schema, the Migration Checklist agent can write the SQL, and the Component/Service Map agent can build the service contracts — all without guessing.
>
> **Two sections cause the most mid-coding rework when skipped or rushed:**
> - **State Machines** — Any entity with a status enum in the Schema needs a state machine here. Skipping it produces invalid state bugs.
> - **Events & Side Effects** — Every notification, email, job, and webhook must be inventoried here. Skipping it produces missing notifications and lost workflows.
>
> Both have dedicated gates in this doc. Do not skip them.
>
> **When this doc is fully filled and approved:** Remove every `🤖 AGENT INSTRUCTIONS` block, every `❓ AGENT PAUSE` prompt, and the agent-facing instruction prose inside `🚦 GATE` blocks. Keep the gate checklists, sign-off lines, and all filled content. The finished doc reads clean for humans.
>
> **Cleanup verification (before declaring the doc done):**
> - Search the file for `🤖` — zero hits
> - Search the file for `❓ AGENT PAUSE` — zero hits
> - Search the file for "Remove this block" — zero hits
> - Every `🚦 GATE` block contains only its checklist and sign-off line — no agent prose

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Architecture Overview | ⏳ Not Started | — | — |
| Tech Stack | ⏳ Not Started | — | — |
| API Endpoints / Services | ⏳ Not Started | — | — |
| 🚦 Gate 1 — Architecture & API Surface | ⏳ Not Started | — | — |
| State Machines | ⏳ Not Started | — | Required if any entity has a status enum with 3+ values |
| Authentication & Authorization | ⏳ Not Started | — | — |
| Error Handling | ⏳ Not Started | — | — |
| Performance Considerations | ⏳ Not Started | — | — |
| Security Considerations | ⏳ Not Started | — | — |
| Deployment & Environments | ⏳ Not Started | — | — |
| Environment Variables | ⏳ Not Started | — | — |
| Monitoring & Logging | ⏳ Not Started | — | — |
| Dependencies & Integrations | ⏳ Not Started | — | — |
| Events & Side Effects | ⏳ Not Started | — | Required if app has any async behavior |
| 🚦 Gate 2 — State Machines & Events Complete | ⏳ Not Started | — | — |
| Analytics & Instrumentation | ⏳ Not Started | — | If deferred, mark explicitly — never leave blank |
| Known Constraints / Trade-offs | ⏳ Not Started | — | — |
| 🚦 Gate 3 — Full Tech Spec Sign-Off | ⏳ Not Started | — | — |

---

## Overview

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Anchor the spec to the PDD's vision and Schema's data model. One paragraph each, no more.
>
> **A complete Overview covers:**
> - What we're building, one sentence — pulled from PDD Problem Statement
> - How it's structured at a high level — the architectural pattern decision
> - Primary constraints — pulled from PDD Technical Constraints
>
> **Remove this block before delivering the filled doc.**

**What we're building:** [One sentence — the system and its purpose]
**How it's structured:** [Key architectural pattern — e.g., REST API + SPA, monolith, microservices]
**Primary constraints:** [The non-negotiables that shape every technical decision — pulled from PDD]

---

## Architecture Overview

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Show how data and requests flow through the system. The diagram and the Components table must match — if a box appears in the diagram, it has a row in the table; if a row exists, it appears in the diagram.
>
> **A complete Architecture Overview covers:**
> - A text diagram showing client → server → data flow, plus any async paths (job queue, external services)
> - Every component named with its specific technology (not "a database" — `PostgreSQL 15`)
>
> **Incomplete looks like:**
> - A generic diagram that could apply to any web app — be specific to this app
> - Component listed as "TBD" — if it's TBD, the decision needs to happen now or be flagged as an Open Question
> - A component in the diagram with no entry in the table (or vice versa)
>
> **Ask triggers:**
> - PDD Technical Constraints implies a tech choice (e.g., "must run on AWS") but the specific service isn't determined — ask
>
> **Remove this block before delivering the filled doc.**

### System Diagram

```
[Client] → [API Gateway / Load Balancer] → [App Server]
                                                 ↓
                                   [Service Layer / Business Logic]
                                         ↓              ↓
                                     [Database]      [Cache]

[App Server] → [Job Queue] → [Worker]
[App Server] → [External Services / APIs]
[Client] ← [CDN] ← [Static Assets]
```

**Components (include what applies, delete what doesn't):**

| Component | Technology | Notes |
|-----------|-----------|-------|
| Frontend | — | — |
| API Layer | — | — |
| App Server | — | — |
| Database | — | — |
| Cache | — | — |
| Job Queue | — | — |
| CDN | — | — |
| External Services | — | — |

---

## Tech Stack

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Lock in every layer with a specific technology, specific version, and a rationale. Downstream the Deployment Config, Testing Strategy, and Module Breakdown all assume these choices are firm.
>
> **A complete Tech Stack entry covers:**
> - Specific technology (not "a JS framework" — `React 18`)
> - Specific version (or "latest LTS" with the date locked)
> - Rationale — one sentence on why this over the alternative
>
> **Incomplete looks like:**
> - "TBD" — choose, or flag as an Open Question with a deadline
> - "JavaScript" instead of `Node.js 20 + TypeScript 5`
> - Missing rationale — if you can't defend the choice, the human probably can't either
>
> **Ask triggers:**
> - PDD constraints reduce the choice set (e.g., "must integrate with existing Postgres") but don't pin the specific option (server-side language? framework?) — ask the human, don't guess
>
> **Remove this block before delivering the filled doc.**

| Layer | Technology | Version | Rationale |
|-------|-----------|---------|-----------|
| Frontend | — | — | — |
| Backend | — | — | — |
| Database | — | — | — |
| Auth | — | — | — |
| ORM | — | — | — |
| Hosting / Deployment | — | — | — |

---

## API Endpoints / Services

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define every endpoint required by every PDD Feature and every UI/UX Screen. This section defines the API surface; the downstream API Contract doc defines the full request/response schemas. Both are needed, but this one must be complete and accurate first.
>
> **A complete API section covers:**
> - **Every PDD Feature** maps to one or more endpoints. Cross-check: if a Feature's "Entities affected" includes Create on `Project`, there must be a `POST /api/projects` here.
> - **Every UI/UX Screen's read needs** map to a GET endpoint. (Note: UI/UX is filled in parallel with this doc, so this cross-check happens at the Cross-Doc Validation step. For now, anticipate from the PDD's Workflows what reads will be needed.)
> - **Validation rules** per endpoint — not generic "validate input" but the actual rules.
> - **Success codes** explicit per endpoint.
> - **Auth requirement** per endpoint — `Required` or `Public`.
>
> **Incomplete looks like:**
> - An endpoint with "standard validation" — that's not validation
> - A POST without listing what fields are required vs optional
> - A search/filter endpoint without listing the filter params
> - A PDD Feature that needs CRUD on an entity but only some operations are listed
>
> **Ask triggers — stop and ask the human if:**
> - The PDD says "users can manage X" but doesn't specify what "manage" means (Create only? CRUD? Read-only with admin-only writes?)
> - Permission rules are unclear (any authenticated user can do this, or owner only, or admin only?)
> - A feature implies pagination but the PDD doesn't specify the pagination shape
> - Whether a write should be PATCH (partial) or PUT (replace) is unclear
>
> **Validation rules — what they must specify:**
> - For strings: required? max length? format (email, URL, etc.)?
> - For enums: list all valid values
> - For numbers: min/max if constrained
> - For relationships: which FK is required vs optional
>
> **Remove this block before delivering the filled doc.**

**Pattern:** [REST / GraphQL / tRPC / Internal services only / etc.]

### REST Endpoints

> For each endpoint: method, path, auth requirement, request shape, validation rules, success response, and success status code.
> Error responses follow the standard format defined in the Error Handling section.

```
POST /api/[resource]
├── Auth: [Required / Public]
├── Request: { field, field }
├── Validation:
│   ├── field: [required, max 255, etc.]
│   └── field: [required, must be valid email, etc.]
├── Response: { field, field }
└── Success: 201

GET /api/[resource]/:id
├── Auth: [Required / Public]
├── Request: (none)
├── Response: { field, field }
└── Success: 200

GET /api/[resource]?filter=:value
├── Auth: [Required / Public]
├── Request: (none)
├── Response: [ { field, field }, ... ]
└── Success: 200

PATCH /api/[resource]/:id
├── Auth: [Required / Public]
├── Request: { field }
├── Validation:
│   └── field: [optional, max 255, etc.]
├── Response: { field, field }
└── Success: 200

DELETE /api/[resource]/:id
├── Auth: [Required / Public]
├── Request: (none)
├── Response: (none)
└── Success: 204
```

> **Why validation lives here:** The DB Schema doc defines what fields exist and their types. This section defines what the API accepts — which may be stricter. A field can be DB-nullable but API-required. Document the API contract here; don't make the reader cross-reference two docs.

### Service Methods (if internal services / non-REST)

```
[Service].[methodName](param, param) → ReturnType
[Service].[methodName](param) → ReturnType | null
```

---

## 🚦 Gate 1 — Architecture & API Surface

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off.**
>
> This gate matters because everything downstream — State Machines, Events, Auth, Deployment — depends on the architecture and API surface being firm.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] Architecture diagram and Components table are consistent
- [ ] Every Tech Stack layer has a specific technology, version, and rationale — no "TBD"
- [ ] Every PDD Core Feature has at least one corresponding endpoint
- [ ] Every endpoint declares Auth (Required / Public)
- [ ] Every endpoint with a request body declares validation rules per field
- [ ] Every endpoint declares its success status code

**Sign-off:**
> 🚦 **Gate 1** — Architecture, tech stack, and API surface are complete and traceable. Ready to define state machines, auth, and async behavior.
>
> **Human sign-off:** ☐ Approved — proceed to State Machines

---

## State Machines

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** For every entity in the Schema that has a status enum (3+ values), produce a complete state machine. The Schema lists the values; this section defines *which transitions are legal, who can trigger them, what guards apply, and what side effects fire*.
>
> **Why this matters:** State machines are the most-skipped section in this template and the #1 cause of mid-coding rework. Missing transitions cause invalid-state bugs. Missing side effects cause missed notifications. Missing guards cause permission holes.
>
> **A complete State Machine block covers:**
> - **States table** — every status value from the Schema appears here, with meaning and "who can be in this state"
> - **Transitions table** — every legal transition with Trigger, Guard, and Side Effects columns filled
> - **Terminal states** — explicitly named
> - **Invalid transitions** — explicitly named for any non-obvious "cannot move from X to Y" rule
> - **API endpoints that trigger transitions** — every transition has a corresponding endpoint in the API Endpoints section
>
> **Incomplete looks like:**
> - A status value in the Schema not listed here
> - A transition with empty Guard column (no guard means "always allowed" — say so explicitly with `—` if so)
> - A transition with empty Side Effects column (no side effects? say `none` explicitly)
> - Side Effects listed but not also appearing in the Events & Side Effects section
> - A transition that triggers a side effect but no API endpoint listed to fire that transition
>
> **Ask triggers — stop and ask the human if:**
> - The PDD's User Workflows describe a status change but the trigger isn't specified (user action? scheduled? webhook?)
> - The Schema has a status value but it's unclear when an entity enters that state
> - Two states seem to overlap — is this really two states or one with different sub-properties?
> - A side effect is implied by a Feature ("notify the user") but the channel isn't specified (email? in-app? both?)
>
> **Cross-reference (every block):**
> - Every state appears in Schema's entity definition
> - Every transition's API endpoint appears in API Endpoints section above
> - Every side effect listed appears as a row in the Events & Side Effects → Event Map section
>
> **Skip this entire section only if:** No entities have lifecycle states. Rare. If in doubt, fill it in.
>
> **Remove this block before delivering the filled doc.**

### [Entity Name] — State Machine

> One block per entity with lifecycle states. Copy this block for each.

**States:**

| State | Meaning | Who can be in this state |
|-------|---------|--------------------------|
| `draft` | [What this state means for the entity] | [Which user roles / system actors] |
| `submitted` | — | — |
| `approved` | — | — |
| `[state]` | — | — |

> ⚠️ Every state in the DB Schema's `status` enum must appear in this table. If a state is here but not in the schema, add it. If it's in the schema but not here, define its transitions.

**Transitions:**

| From | To | Trigger | Guard (must be true) | Side Effects |
|------|----|---------|----------------------|--------------|
| `draft` | `submitted` | User clicks Submit | [Condition — e.g., all required fields filled] | [What fires — e.g., send confirmation email, create audit log entry] |
| `submitted` | `approved` | Admin approves | [Condition — e.g., actor has `admin` role] | [e.g., notify user, update `approved_at` timestamp] |
| `submitted` | `rejected` | Admin rejects | [Condition] | [e.g., notify user with reason, log rejection] |
| `approved` | `cancelled` | User or admin cancels | [Condition — e.g., within 24hr window, or admin only] | [e.g., trigger refund, release held resources] |
| `[any]` | `[any]` | — | — | — |

> **Columns:**
> - **Trigger:** What action initiates this — an API call, a background job, a webhook, a scheduled event, or an admin action.
> - **Guard:** The condition that must be true for the transition to be allowed. If the guard fails, return a `409 Conflict` with a clear message.
> - **Side Effects:** Every downstream action this transition fires. This is where missing notifications, missed audit log writes, and incomplete cleanup get caught.

**Terminal states:** `[state]`, `[state]` — no transitions out.

**Invalid transitions:**
> List transitions that are explicitly disallowed and need a clear error (not just "not in the table").

- Cannot move from `approved` → `draft` (no un-approving)
- Cannot move from `cancelled` → any state (cancellation is final)

**API endpoints that trigger transitions:**

| Endpoint | Transition | Notes |
|----------|-----------|-------|
| `PATCH /api/[resource]/:id/submit` | `draft` → `submitted` | — |
| `PATCH /api/[resource]/:id/approve` | `submitted` → `approved` | Admin only |
| `DELETE /api/[resource]/:id` | `[any]` → `cancelled` | Soft delete if entity is in a terminal state |

> Cross-reference: Every endpoint listed here must appear in the API Endpoints section above. If it doesn't, add it.

---

### [Entity Name] — State Machine

*(Copy block above for each additional entity with lifecycle states)*

---

## Authentication & Authorization

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define both authentication (who are you?) and authorization (what can you do?). These are separate concerns — many designs conflate them and produce auth holes.
>
> **A complete Auth section covers:**
> - **Method** — specific (JWT, session cookies, OAuth) not generic ("token-based")
> - **Login flow** — numbered steps
> - **Token expiration** with rationale
> - **Refresh strategy** — explicit, including "None, force re-login" as a valid choice
> - **Token storage** — httpOnly cookie or localStorage, with security rationale
> - **Authorization model** — RBAC, ABAC, row-level, etc.
> - **Enforcement layer** — middleware, service layer, DB
> - **Roles and permissions table** — every role from the PDD with explicit allowed actions
>
> **Incomplete looks like:**
> - "JWT" with no expiration time
> - "Token-based" with no storage decision
> - Roles listed without permissions
> - Permissions described as "standard user permissions"
>
> **Ask triggers:**
> - PDD personas imply distinct roles but PDD doesn't enumerate the roles — ask
> - Whether unauthenticated users can do anything (e.g., view public content) is unclear
> - Whether refresh tokens are in scope
>
> **Remove this block before delivering the filled doc.**

### Authentication — Who are you?

- **Method:** [JWT / OAuth / Session-based / Magic Link / etc.]
- **Login flow:**
  1. [Step 1]
  2. [Step 2]
  3. [Step 3]
- **Token expiration:** [Duration]
- **Refresh strategy:** [How tokens are refreshed — or "None, force re-login"]
- **Token storage:** [httpOnly cookie / localStorage] — [Rationale — this is a security decision]

### Authorization — What can you do?

- **Model:** [Role-based (RBAC) / Attribute-based (ABAC) / Row-level / etc.]
- **Enforcement layer:** [Middleware / Service layer / Database / etc.]
- **401 vs. 403:** 401 = not authenticated. 403 = authenticated but not permitted.

**Roles and permissions:**

| Role | Permissions |
|------|------------|
| [Role] | [What they can do] |
| [Role] | [What they can do] |

---

## Error Handling

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define the standard error response format. This locks the contract for both the API Contract doc (which uses this format on every endpoint) and the UI/UX doc (which builds error UI around this shape).
>
> **A complete Error Handling section covers:**
> - The JSON error format with all fields explained
> - Where errors are caught (middleware, per-route, etc.)
> - The full HTTP status code table for this app
>
> **The format below is the recommended baseline.** Only deviate if there's a specific reason (e.g., conforming to an existing API consumer).
>
> **Remove this block before delivering the filled doc.**

**Standard error response format:**

```json
{
  "status": 400,
  "error": "BadRequest",
  "message": "User-friendly error message",
  "details": [
    { "field": "email", "issue": "already exists" },
    { "field": "name", "issue": "is required" }
  ]
}
```

> `details` is an array to support multiple validation errors in a single response.

**Where errors are caught:** [Middleware / Per-route / Service layer — be explicit]

**Standard status codes:**

| Code | Meaning | When to use |
|------|---------|-------------|
| 200 | OK | Successful GET, PATCH |
| 201 | Created | Successful POST |
| 204 | No Content | Successful DELETE |
| 400 | Bad Request | Validation failed |
| 401 | Unauthorized | Not authenticated |
| 403 | Forbidden | Authenticated but not permitted |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Duplicate / state conflict |
| 500 | Internal Server Error | Unexpected error |

---

## Performance Considerations

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Document the performance design — indexing, caching, pagination, N+1 prevention. This must trace to PDD Technical Constraints (any performance targets specified there) and Schema (indexes already defined).
>
> **A complete Performance section covers:**
> - **Indexes** beyond PK/FK with rationale — must match the Schema's Entities and Query Patterns sections
> - **Caching strategy** — what's cached, where, with what TTL and invalidation
> - **Pagination strategy** — cursor or offset, default page size
> - **N+1 prevention** — how the team will avoid the most common ORM performance trap
>
> **Ask triggers:**
> - PDD specifies a performance target ("API response ≤ 500ms at p95") but the implementation path isn't designed
> - Caching is implied by a constraint but the layer (in-memory vs Redis) isn't chosen
>
> **Remove this block before delivering the filled doc.**

**Database indexing:**
> List indexes beyond PKs and FKs — explain why each one exists.

| Table | Field(s) | Index Type | Reason |
|-------|---------|-----------|--------|
| [table] | [field] | Standard | [Range queries / search / sort] |
| [table] | [field1, field2] | Compound | [Filters that always appear together] |

**Caching strategy:**

| What | Cache Layer | TTL | Invalidation Strategy |
|------|------------|-----|-----------------------|
| [Query / resource] | [Redis / In-memory / None] | [Duration] | [On write / TTL expiry / manual] |

**Other:**
- Connection pooling: [Pool size and rationale]
- Pagination: [Strategy — cursor / offset, default page size]
- N+1 prevention: [JOINs / eager loading / dataloader — where applicable]

---

## Security Considerations

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Document security decisions and enforcement. PDD Technical Constraints may name compliance requirements (HIPAA, GDPR, etc.) — every named requirement must map to one or more rows here.
>
> **A complete Security section covers:**
> - Secrets management approach (cross-reference Environment Variables section)
> - Application security row for each concern (password hashing, HTTPS, CORS, rate limiting, input validation, SQL injection, dependency scanning, auth token security)
>
> **Incomplete looks like:**
> - "Best practices" — name the specific algorithm, library, or config
> - Rate limiting "where needed" — name the endpoints rate-limited
> - "Input validation" with no layer named
>
> **Ask triggers:**
> - PDD names a compliance requirement (e.g., HIPAA) but the implementation isn't designed (where's PHI? who has access? audit logging?)
> - Sensitive data fields exist in the Schema but no row addresses how they're protected
>
> **Remove this block before delivering the filled doc.**

**Secrets management:**
- Environment variables: [How stored — .env, secrets manager, etc.]
- Never committed to version control: [List what's secret — reference Environment Variables section]

**Application security:**

| Concern | Approach |
|---------|---------|
| Password hashing | [Algorithm + config — e.g., bcrypt, 12 rounds] |
| HTTPS | [Enforced / Redirect HTTP → HTTPS] |
| CORS | [Allowed origins — be specific] |
| Rate limiting | [Requests per window per IP — and what's rate limited] |
| Input validation | [Where validated — API layer, service layer, or both] |
| SQL injection | [Parameterized queries / ORM — never raw string concatenation] |
| Dependency vulnerabilities | [How monitored — npm audit, Dependabot, etc.] |
| Auth token security | [Storage, transmission, expiry — reference Auth section] |

---

## Deployment & Environments

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define the environment layout and CI/CD pipeline at a high level. The Deployment Config doc downstream will go into operational detail; this section establishes the foundation.
>
> **A complete Deployment section covers:**
> - CI/CD pipeline trigger, steps, and tool
> - Each environment (Dev, Staging, Production) with setup, DB, deploy trigger
>
> **Ask triggers:**
> - Hosting platform not chosen — flag as Open Question
> - Whether staging exists — some MVPs skip staging; that's valid but must be explicit
>
> **Remove this block before delivering the filled doc.**

**CI/CD Pipeline:**
- Trigger: [Push to branch / PR merge / manual]
- Steps: [Lint → Test → Build → Deploy]
- Tool: [GitHub Actions / CircleCI / etc.]

### Development

- Setup: [Commands to get running locally]
- Database: [Local / Docker]
- Environment variables: `.env.local` — [Where the template lives]

### Staging

- Platform: [—]
- Database: [Staging instance]
- URL: [—]
- Deployed on: [Every merge to main / manual / etc.]

### Production

- Platform: [—]
- Database: [Production instance]
- URL: [—]
- Deployed on: [Tagged release / manual approval / etc.]
- Backups: [Frequency + destination]

---

## Environment Variables

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Build the canonical, exhaustive list of every env var the app requires. This list is the source of truth — `.env.example` is a derivative, the Deployment Config is a derivative, this is the master.
>
> **A complete Environment Variables section covers:**
> - Every secret named in Security Considerations
> - Every external service connection (DATABASE_URL, REDIS_URL, third-party API keys)
> - Every config knob (NODE_ENV, PORT, log level)
> - Required column (Yes/No)
> - Example value (placeholder — never a real secret)
> - Description of what it does
>
> **Incomplete looks like:**
> - Vars used in code but not listed here
> - Real production secrets visible in the Example column — never
> - No Required column — every var must declare this
>
> **Ask triggers:**
> - The PDD or Tech Stack implies a service (Stripe, SendGrid) but no API key var is listed
>
> **Remove this block before delivering the filled doc.**

> **Complete list of all environment variables the app requires.** This is the canonical reference — not scattered across the codebase.
> Never commit real values. Commit a `.env.example` with placeholder values only.

| Variable | Required? | Example Value | Description |
|----------|-----------|---------------|-------------|
| `DATABASE_URL` | Yes | `postgresql://user:pass@localhost:5432/mydb` | Primary database connection string |
| `JWT_SECRET` | Yes | `some-long-random-string` | Secret used to sign JWT tokens |
| `PORT` | No | `3000` | Port the server listens on. Defaults to 3000. |
| `NODE_ENV` | Yes | `development` / `staging` / `production` | Controls log level, error detail, etc. |
| `[VAR_NAME]` | [Yes / No] | `[example]` | [What it does, where it's used] |

> When adding a new env var: add it here, add it to `.env.example`, and document which environments need it.

---

## Monitoring & Logging

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define how the app is observed in production. PDD Success Metrics may imply metrics that need instrumentation — every such metric maps to either a metric here or an analytics event in the Analytics section below.
>
> **A complete Monitoring section covers:**
> - Log format (structured JSON preferred)
> - Log levels by environment
> - Log destination (specific tool)
> - Health check endpoint with the checks it performs
> - Metrics tracked with alert thresholds
>
> **Ask triggers:**
> - PDD metric requires data the system doesn't currently collect — flag it
> - No error tracking tool chosen (Sentry, Bugsnag, etc.) — production without error tracking is reckless
>
> **Remove this block before delivering the filled doc.**

**Log format:** [Structured JSON / plaintext] — structured preferred for querying

**Log levels:**

| Level | Environment | What gets logged |
|-------|------------|-----------------|
| DEBUG | Development | Everything |
| INFO | Staging / Production | Requests, key events |
| ERROR | All | Exceptions, failures |

**Log destination:** [Console / CloudWatch / Datadog / etc.]

**Health check endpoint:** `GET /health` — [What it checks]

**Metrics tracked:**

| Metric | Tool | Alert threshold |
|--------|------|----------------|
| Error rate | [—] | [—] |
| Request latency (p95) | [—] | [—] |
| Uptime | [—] | [—] |

---

## Dependencies & Integrations

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Inventory every external service the app depends on and every key library. PDD Pre-Design Thought List may have surfaced third-party dependencies — every one of those should appear here.
>
> **A complete Dependencies section covers:**
> - Every external service with its purpose and fallback behavior
> - Every key library with version and rationale
>
> **Ask triggers:**
> - PDD implies an integration (e.g., "send emails") but the specific service isn't chosen
> - A service's failure mode isn't documented — what does the app do when SendGrid is down?
>
> **Remove this block before delivering the filled doc.**

**External services:**

| Service | Purpose | Fallback if unavailable |
|---------|---------|------------------------|
| [Service] | [What it does] | [Degrade gracefully / block / queue] |

**Key libraries:**

| Library | Version | Purpose |
|---------|---------|---------|
| [Library] | [—] | [Why it's here, not an alternative] |

> When adding a new dependency: document the version, the reason, and what you evaluated and rejected.

---

## Events & Side Effects

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Inventory every async behavior in the system. This is the second most-skipped section after State Machines and the second-largest source of mid-coding rework. Every side effect listed in any State Machine transition must appear here. Every notification, email, push, webhook, background job, and scheduled task must be defined.
>
> **A complete Events & Side Effects section covers:**
> - **Delivery Methods table** — every channel the app uses (email, push, in-app, queue, cron, WebSocket, webhook)
> - **Event Map** — one row per trigger→action pair. Cross-references State Machines and Dependencies.
> - **Background Jobs** — detail block per non-trivial job
> - **Scheduled Tasks** — cron schedule for every recurring task
> - **Real-Time Events** — only if WebSocket/SSE is used
>
> **Incomplete looks like:**
> - A side effect in a State Machine transition with no row in the Event Map
> - A delivery method named without an entry in Dependencies & Integrations
> - A background job listed in Event Map with non-trivial logic but no detail block
> - A scheduled task with cron in a non-UTC timezone but no fan-out logic
> - On-failure column empty for any row
>
> **Ask triggers — stop and ask the human if:**
> - A workflow in the PDD says "notify the user" but the channel isn't specified
> - A workflow implies a job (e.g., "we calculate this nightly") but the schedule isn't specified
> - It's unclear whether a job is idempotent — required for retry safety
> - A side effect could fail and the rollback behavior isn't documented (do we retry? alert? cancel the triggering transition?)
>
> **Cross-reference (every row in Event Map):**
> - Every Trigger that's a state transition appears in the State Machines section
> - Every Delivery Method appears in the Delivery Methods table above
> - Every Recipient role appears in Authentication & Authorization roles
> - Every Handler will become a service method documented in the downstream Component/Service Map
>
> **Skip this entire section only if:** The app is fully synchronous, has no notifications, no emails, no webhooks, no scheduled tasks. Almost never true for a real app.
>
> ⛔ **Architecture ceiling:** This section covers event-driven *behavior*. It does NOT cover event-driven *architecture* (Kafka, event sourcing, CQRS, sagas). If any of those apply, a dedicated Event-Driven Architecture doc is required.
>
> **Remove this block before delivering the filled doc.**

> **What this is:** Every trigger→action chain in the system — things that happen asynchronously, in the background, or as a consequence of another action. Emails, push notifications, webhooks, background jobs, scheduled tasks, and real-time updates all belong here.
>
> ⛔ **Architecture ceiling — read before continuing:** This section covers event-driven *behavior* (fire a job, send an email, push a WebSocket update). It does NOT cover event-driven *architecture*. If any of the following are true for this app, this section is insufficient and a dedicated Event-Driven Architecture doc is required before coding begins:
> - Using a message broker (Kafka, RabbitMQ, NATS, etc.) as the primary integration layer
> - Event sourcing — the event log is the source of truth, not the database rows
> - CQRS — separate read and write models derived from events
> - Saga pattern — multi-step distributed transactions where partial failures must trigger compensating actions
> - Event schema versioning — consumers at different versions must process the same event stream
> - Eventual consistency contracts — downstream systems are explicitly allowed to be stale, and the app must handle that
>
> If you're hitting any of those, stop here and flag it. Forcing that architecture into this table will produce incomplete documentation and missed failure modes.

---

### Delivery Methods

> Define the mechanisms used to deliver events in this app. Delete rows that don't apply.

| Method | Technology | Purpose | Notes |
|--------|-----------|---------|-------|
| Transactional email | [SendGrid / Postmark / SES / etc.] | [User notifications, confirmations, alerts] | [Template system — yes/no?] |
| Push notification | [FCM / APNs / OneSignal / etc.] | [Mobile alerts] | [Only if mobile app] |
| In-app notification | [Internal — DB-backed notification feed] | [Real-time or polled alerts inside the app] | — |
| Background job | [Bull / Sidekiq / Celery / etc.] | [Async processing, retryable work] | [Queue name, worker config] |
| Scheduled job / cron | [node-cron / pg_cron / Heroku Scheduler / etc.] | [Recurring tasks — cleanup, digests, reminders] | [Timezone considerations] |
| WebSocket / SSE | [Socket.io / native WS / SSE] | [Real-time UI updates] | [What events push to client] |
| Outbound webhook | [HTTP POST to external URL] | [Notify third parties of events] | [Retry strategy, signing] |

> Cross-reference: Every delivery method listed here needs a corresponding entry in Dependencies & Integrations above.

---

### Event Map

> One row per trigger→action pair. This is the full inventory of async behavior in the system.
>
> **Columns:**
> - **Trigger:** What causes this — a state transition, a user action, a scheduled time, an incoming webhook, or a system condition.
> - **Event / Action:** What happens as a result — what gets sent, queued, or pushed.
> - **Handler:** What code owns this — a service method, a job class, a cron task.
> - **Delivery method:** From the table above.
> - **Recipient:** Who receives it — specific user role, all users, an external system, etc.
> - **On failure:** What happens if delivery fails — retry (how many times?), log and ignore, alert admin, block the triggering action.

| Trigger | Event / Action | Handler | Delivery Method | Recipient | On Failure |
|---------|---------------|---------|----------------|-----------|------------|
| User registers | Send welcome email | `UserService.onRegistered` | Transactional email | New user | Log + retry 3× |
| `[Entity]` → `submitted` | Send confirmation email | `[Service].[method]` | Transactional email | [Recipient role] | [Retry / log / alert] |
| `[Entity]` → `approved` | Notify user in-app + email | `[Service].[method]` | In-app + email | [Recipient] | [—] |
| Payment fails | Notify admin, queue retry | `PaymentService.onFailure` | In-app (admin) + background job | Admin role | Alert on-call if retry queue fails |
| Nightly at 2am UTC | Archive expired `[entities]` | `ArchiveJob` | Scheduled job | — (system) | Log + alert if >0 errors |
| Incoming webhook from `[service]` | Update `[entity]` status | `WebhookHandler.[service]` | — (inbound) | — | Return 200, log failure, retry via queue |
| [User action — e.g., sends message] | Push real-time update to recipient | `[Service].[method]` | WebSocket | [Specific user] | Degrade gracefully — recipient polls on reconnect |
| [Trigger] | [Action] | [Handler] | [Method] | [Recipient] | [On failure] |

> ⚠️ **Every "Side Effect" listed in the State Machines section must have a corresponding row here.** If a transition fires a notification or job, define it here. If it's not here, it won't be built.

---

### Background Jobs

> One block per job class. Only fill in for jobs with non-trivial logic, retry behavior, or failure consequences. Simple fire-and-forget jobs (e.g., "send this email") don't need a block — the Event Map row is sufficient.

#### `[JobName]`

**Triggered by:** [Event / schedule / manual]
**Queue:** `[queue name]` — [Priority: high / default / low]
**Idempotent?** [Yes — safe to run twice / No — must deduplicate]

**What it does:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Retry policy:**
- Max attempts: [#]
- Backoff: [immediate / exponential — e.g., 1m, 5m, 30m]
- On final failure: [Log + alert / move to dead-letter queue / mark entity as failed]

**Side effects of this job:** [What it writes, sends, or triggers — and whether those are also retryable]

**Concurrency concern:** [Can multiple instances of this job run simultaneously? If not, how is it locked?]

---

#### `[JobName]`

*(Copy block above for each non-trivial background job)*

---

### Scheduled Tasks

> One row per recurring task. Include the schedule in plain language and as a cron expression.

| Task | Schedule | Cron | Handler | What it does | On failure |
|------|----------|------|---------|-------------|------------|
| [Task name] | [Nightly at 2am UTC] | `0 2 * * *` | `[JobClass]` | [What it does] | [Alert / log / retry next run] |
| [Task name] | [Every 15 minutes] | `*/15 * * * *` | `[JobClass]` | [What it does] | [—] |

> **Timezone rule:** All cron schedules run in UTC. If a task is user-timezone-sensitive (e.g., "send digest at 8am user local time"), document that the job fans out per timezone or per user preference.

---

### Real-Time Events (WebSocket / SSE)

> Only fill in if the app has real-time UI updates. Delete this subsection if polling or full page refresh is sufficient.

**Connection strategy:** [Single persistent connection per authenticated user / room-based / etc.]
**Auth:** [Token passed on connection / validated on upgrade / etc.]

| Event name | Direction | Payload | When emitted | Who receives it |
|------------|-----------|---------|-------------|----------------|
| `[event:name]` | Server → Client | `{ field, field }` | [When this fires — e.g., on state transition, on new message] | [Specific user / room / broadcast] |
| `[event:name]` | Client → Server | `{ field }` | [When client sends this] | [Handled by which service method] |

**Reconnect behavior:** [What happens when a client disconnects and reconnects — does it replay missed events? Poll for diff?]

---

## 🚦 Gate 2 — State Machines & Events Complete

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off.**
>
> This gate exists specifically because these two sections are most often rushed or skipped, and that mistake compounds during coding. Run every check carefully.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] Every entity in the Schema with a status enum (3+ values) has a State Machine block
- [ ] Every status value from the Schema appears in its entity's States table
- [ ] Every State Machine transition has Trigger, Guard, and Side Effects columns filled (use `—` or `none` explicitly — never blank)
- [ ] Every Side Effect listed in any State Machine appears as a row in the Event Map
- [ ] Every API endpoint listed in "API endpoints that trigger transitions" appears in API Endpoints
- [ ] Every Delivery Method named in the Event Map appears in the Delivery Methods table
- [ ] Every Delivery Method has a corresponding entry in Dependencies & Integrations
- [ ] Every Event Map row has an On Failure value — none blank
- [ ] Every non-trivial Background Job has a detail block with idempotency and retry policy
- [ ] Every Scheduled Task has a UTC cron expression (or explicit fan-out logic for user-timezone tasks)

**Sign-off:**
> 🚦 **Gate 2** — State machines and event behavior are complete and cross-referenced. Side effects accounted for. Ready for analytics and final sign-off.
>
> **Human sign-off:** ☐ Approved — proceed to Analytics

---

## Analytics & Instrumentation

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Map every PDD Success Metric to a measurement plan. If a metric can't be measured, flag it now. If analytics is deferred, say so explicitly.
>
> **A complete Analytics section covers:**
> - Analytics tool (specific, or explicit "None" with rationale)
> - What gets tracked (user actions, system events, or both)
> - PII policy
> - Event schema (the standard shape)
> - Event Inventory mapping every PDD metric to at least one event
> - Funnels for critical conversion paths
>
> **Incomplete looks like:**
> - "Analytics TBD" — choose now or explicitly defer
> - A PDD Success Metric with no corresponding event here — that metric is unmeasurable
> - Event schema with inconsistent naming (some snake_case, some camelCase)
>
> **Ask triggers:**
> - PDD names a metric the system can't measure with current data — flag it
> - PII policy unclear — what user data may go in event properties?
>
> **Remove this block before delivering the filled doc.**

> **What this is:** How the app measures whether it's working — what events are tracked, what tool collects them, and what the event schema looks like.
> **Why this exists:** Success Metrics in the PDD define *what* you want to measure. This section defines *how*.

**Analytics tool:** [PostHog / Mixpanel / Amplitude / Custom / None — explain why]

**What gets tracked:** [User actions only / user actions + system events / funnel-focused / etc.]

**PII policy:** [No PII in event properties / anonymized user ID only / explicit list of what's allowed]

---

### Event Schema

> Standard shape for every analytics event. Consistent properties enable reliable querying.

```
{
  event: "[noun].[verb]"       // e.g., "project.created", "user.signed_up", "invite.sent"
  userId: "[id or anonymous]"
  timestamp: "[ISO 8601]"
  properties: {
    // event-specific properties — no PII unless explicitly approved above
  }
}
```

> **Naming convention:** `[noun].[verb]` — lowercase, dot-separated. Past tense verb. Consistent nouns that match Core Entities.

---

### Event Inventory

> One row per tracked event. Every PDD Success Metric must map to at least one event here.

| Event name | When it fires | Key properties | Maps to PDD metric |
|------------|--------------|---------------|--------------------|
| `user.signed_up` | On successful registration | `{ method: "email\|oauth", source }` | [e.g., "User growth"] |
| `[entity].[action]` | [When] | `{ [field], [field] }` | [PDD metric or "—"] |
| `[event]` | [When] | `{ [field] }` | [—] |

> ⚠️ Every row in the PDD Success Metrics section should have at least one corresponding event here. If a metric has no event, you can't measure it — flag it.

---

### Funnels & Key Flows (optional)

> If the product has critical conversion paths (signup → onboarding → first value), define the funnel steps here so the event sequence can be verified.

| Funnel | Steps (in order) |
|--------|-----------------|
| [Funnel name — e.g., "Onboarding"] | `user.signed_up` → `profile.completed` → `[first value action]` |
| [Funnel name] | `[event]` → `[event]` → `[event]` |

---

## Known Constraints / Trade-offs

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Document the deliberate trade-offs made in this spec. PDD Technical Constraints lists hard requirements; this section captures the design choices made within those constraints — what was given up and why.
>
> **A complete Known Constraints section covers:**
> - The constraint or trade-off (what we're not doing or what limits exist)
> - What we give up by accepting this trade-off
> - The rationale (why this is still the right call)
>
> **Remove this block before delivering the filled doc.**

| Constraint | Trade-off | Rationale |
|------------|----------|-----------|
| [What we can't or won't do] | [What we give up] | [Why it's still the right call] |

---

## Future Improvements / TODOs

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Capture deferred work. Items here are things the human chose to push to a later phase — not bugs, not gaps. Every item has a "why deferred" reason.
>
> **Remove this block before delivering the filled doc.**

- [ ] [Item] — [Why deferred]

---

## 🚦 Gate 3 — Full Tech Spec Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off. This is the final gate before downstream coding-phase docs (API Contract, Module Breakdown, Component/Service Map) consume this spec.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] All sections complete — Status table shows no ⏳ or 🔄
- [ ] Gates 1 and 2 are signed off
- [ ] Every PDD Core Feature is realized by one or more endpoints in API Endpoints
- [ ] Every Schema entity with a status enum has a State Machine
- [ ] Every State Machine side effect appears in the Event Map
- [ ] Every Event Map row has a Delivery Method that appears in Dependencies & Integrations
- [ ] Every PDD Success Metric maps to at least one Analytics event (or analytics explicitly deferred)
- [ ] Every PDD Technical Constraint (compliance, performance, integration) is addressed in the appropriate section
- [ ] Every Environment Variable referenced in code design is documented
- [ ] No "TBD" left in any section — open items are in Future Improvements with rationale, or in an Open Questions area

**Sign-off:**
> 🚦 **Gate 3** — Tech Spec complete, internally consistent, traces fully to PDD and Schema. Ready for UI/UX final pass and downstream coding-phase docs.
>
> **Human sign-off:** ☐ Approved — Tech Spec complete. Proceed to UI/UX (or finalize UI/UX if filled in parallel).
