# UI/UX Design Doc: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized
> **Fill order:** Third design doc — filled after Schema, in parallel with Tech Spec. Some sections of this doc require Tech Spec sections to be done first (see Source Docs below). If Tech Spec isn't ready, fill what you can here and gate the Screen → API Map until Tech Spec is signed off.
>
> **Source docs:**
> - `[AppName]_Product_Design_Doc.md`
>   - Personas → drives whose perspective each screen serves
>   - Core Features → drives what screens exist
>   - User Workflows → drives screen-to-screen navigation and decision points (every workflow step lands on a screen)
>   - Workflow Error Paths → drives the Error States table
>   - Out of Scope → drives what NOT to design
> - `[AppName]_DB_Schema.md`
>   - Entities and fields → drives what data each screen displays
>   - Relationships → drives navigation between detail views
>   - Status enums → drives status badges, filters, and conditional UI
> - `[AppName]_Technical_Spec.md`
>   - API Endpoints → drives the Screen → API Map
>   - Authentication & Authorization roles → drives who sees which screens and which actions
>   - Error Handling format → drives the standard UI error response patterns
>   - State Machines → drives status displays and which transitions get UI surfaces
>
> **Internal fill order within this doc (strict — do not reorder):**
> 1. Overview
> 2. Design System / Brand Guidelines
> 3. **🚦 Gate 1** — Design System locked
> 4. Screens / Pages (all screens, all 4 states each)
> 5. Shared Component Inventory (derived FROM Screens — cannot be filled before)
> 6. Navigation / IA
> 7. Screen → API Map (requires Tech Spec API Endpoints to be signed off)
> 8. **🚦 Gate 2** — Screens + Components + API Map complete
> 9. User Interactions / State Management (including Error States table)
> 10. Accessibility
> 11. Responsive Design
> 12. Animation / Motion
> 13. Design Decisions / Rationale
> 14. Future Design Improvements
> 15. **🚦 Gate 3** — Full UI/UX sign-off
>
> **Agent role:** Translate the PDD's personas, workflows, and features into a complete, unambiguous UI specification. Every screen, every state, every component, every error message pattern must trace to either the PDD, Schema, Tech Spec, or a confirmed answer from the human. No invented screens. No invented states. No invented components.
>
> **The three rules while filling this doc:**
> 1. Everything written traces to PDD + Schema + Tech Spec + confirmed human input. If a workflow step lands on a screen, that screen exists here. If an entity displays a status, the badge variant exists in the design system. If a screen reads data, an API endpoint backs it. No invented UI.
> 2. If the human describes a screen at a level that's clear to them but ambiguous to a coding agent (e.g., "it shows their projects"), stop and ask — what fields? what sort order? what filters? what does the empty state look like? what does the error state look like?
> 3. Output must be specific enough that the UI Strings agent can pull every user-facing string without inventing copy, the Component/Service Map agent can derive every frontend component without guessing, and the frontend coding agent can build screens without inventing layouts, states, or error UI.
>
> **Four sections cause the most mid-coding rework when skipped or rushed in this doc:**
> - **All 4 screen states (Loading / Empty / Error / Populated)** — Empty and Error are most commonly skipped. Missing them produces inconsistent invented UI at code time.
> - **Re-fetch triggers** per screen — Missing them produces stale-data bugs in production.
> - **Shared Component Inventory** — Filled too early (before screens drafted) produces invented components that don't match real needs. Skipped produces duplicate component builds across modules.
> - **Error States table** — The biggest single source of inconsistent UI at code time. Every user-initiated action that can fail must have a pattern row.
>
> All four have explicit gate enforcement in this doc.
>
> **When this doc is fully filled and approved:** Remove every `🤖 AGENT INSTRUCTIONS` block, every `❓ AGENT PAUSE` prompt, and the agent-facing instruction prose inside `🚦 GATE` blocks. Keep the gate checklists, sign-off lines, and all filled content. The finished doc reads clean for humans — no agent scaffolding visible.
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
| Overview | ⏳ Not Started | — | — |
| Design System / Brand | ⏳ Not Started | — | — |
| 🚦 Gate 1 — Design System Locked | ⏳ Not Started | — | — |
| Screens / Pages | ⏳ Not Started | — | All 4 states required per screen |
| Shared Component Inventory | ⏳ Not Started | — | Fill AFTER Screens — components emerge from screens |
| Navigation / IA | ⏳ Not Started | — | — |
| Screen → API Map | ⏳ Not Started | — | Requires Tech Spec API Endpoints signed off |
| 🚦 Gate 2 — Screens + Components + API Map Complete | ⏳ Not Started | — | — |
| User Interactions / State Management | ⏳ Not Started | — | Includes Error States table — comprehensive required |
| Accessibility | ⏳ Not Started | — | — |
| Responsive Design | ⏳ Not Started | — | — |
| Animation / Motion | ⏳ Not Started | — | — |
| Design Decisions / Rationale | ⏳ Not Started | — | — |
| Future Design Improvements | ⏳ Not Started | — | — |
| 🚦 Gate 3 — Full UI/UX Sign-Off | ⏳ Not Started | — | — |

**Status scheme:** ⏳ Not Started → 🔄 In Progress → ❓ Needs Discussion → ✅ Done

---

## Overview

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Anchor the UI/UX doc to the PDD's vision and the navigation pattern decision. Three short bullets. This is not where design lives — design lives in every section below. This is orientation.
>
> **A complete Overview covers:**
> - **What it looks like** — pulled from the human's description, expressed in concrete terms (not "modern and clean" — "dense data tables on dark surfaces with a single accent color"). Reference any inspiration apps the human named.
> - **How users navigate** — the binding decision. One of: sidebar nav, top nav, bottom tab bar, drawer/hamburger, command palette. This decision shapes every screen layout downstream.
> - **Key screens** — 4–8 main screens, one line each. Pulled from PDD Core Features and User Workflows. Every workflow lands on a screen named here.
>
> **Incomplete looks like:**
> - "Modern, clean, intuitive" — these words mean nothing to a coding agent
> - Navigation pattern listed as "TBD" — choose, or flag as an Open Question with a deadline (this decision blocks every screen)
> - Key screens that don't trace to any PDD Feature or Workflow
> - A PDD Workflow whose endpoint screen isn't in the Key Screens list
>
> **Ask triggers — stop and ask the human if:**
> - The human says "make it look good" without naming a visual style or inspiration
> - Navigation pattern preference is unstated
> - The number of key screens implied by PDD Features doesn't match the count being described (way more or way fewer)
> - Multiple navigation patterns are in tension (e.g., "sidebar and bottom tabs" — these don't coexist; one is primary)
>
> **Remove this block before delivering the filled doc.**

**What it looks like:** [Visual style — concrete terms, not adjectives. Reference inspiration if named.]
**How users navigate:** [Primary navigation pattern — sidebar / top nav / bottom tab bar / drawer / command palette. This is a binding decision that shapes every screen.]
**Key screens:** [4–8 main screens, one line each. Every PDD User Workflow must end on a screen named here.]

---

## Design System / Brand Guidelines

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define every design token by name. Tokens are the contract between this doc and every screen. Once tokens are named, every screen references the token name — never raw values. This section is the only place raw values appear.
>
> **A complete Design System section covers:**
> - **Colors** — every token named with its raw value and usage. Cover: primary, secondary, neutrals, success, warning, error, surface, bg, and any app-specific role colors (e.g., severity tiers, role colors).
> - **Dark mode decision** — binary. In scope for MVP, or explicitly deferred. No "maybe later" — that's deferred.
> - **Typography** — every text role named with font, size, weight, line height, and usage.
> - **Spacing scale** — every spacing token named with raw value and where it's used.
> - **Core components** — for each primitive (Button, Input, Card, etc.), every variant enumerated.
>
> **Why tokens matter:** Without named tokens, every screen description references "blue" or "the dark gray" and the coder makes 17 different blues. Named tokens force consistency. Raw values appear in this section only.
>
> **Pull from PDD:**
> - Persona technical comfort levels → may affect density (high-comfort users = denser; low-comfort = more whitespace)
> - Compliance constraints → may affect contrast requirements (e.g., medical apps often need stricter contrast)
> - Brand assets if the human mentioned them
>
> **Pull from Schema:**
> - Every status enum value needs a badge variant (e.g., if `draft / submitted / approved / rejected`, the Badge component needs four variants — confirm which color tokens map to which status)
>
> **Incomplete looks like:**
> - Token cells with raw values but no name — name first, value second
> - Dark mode listed as "TBD" — decide now or defer explicitly
> - Component variants listed as "etc." — enumerate every variant
> - Color used in a screen description that isn't in the token table
> - A status enum value with no badge variant assigned
>
> **Ask triggers — stop and ask the human if:**
> - Brand colors aren't provided and no inspiration is named — this blocks every screen
> - Typography is undecided — at minimum, system font stack vs custom must be chosen
> - Dark mode scope is ambiguous
> - The app has dense data tables but density preference (compact / comfortable / spacious) isn't stated
> - Whether the app uses elevation (shadows) or borders for surface separation is unclear — this is a visual identity decision
> - A status enum has 4+ values and a color treatment for each isn't decided (e.g., what color does `pending review` use?)
>
> **Cross-reference checklist for this section:**
> - Every status enum from the Schema has a mapped Badge variant
> - Every error-related color token has 4.5:1 contrast against bg/surface (WCAG AA)
> - Every "color used" in a screen description below appears as a token here
>
> **Remove this block before delivering the filled doc.**

> **Token rule:** Document colors, type, and spacing as named tokens (e.g., `color-primary`, `spacing-md`), not raw values. Raw values go in one place only — here. Everywhere else references the token name.

**Colors:**

| Token | Value | Usage |
|-------|-------|-------|
| `color-primary` | — | Primary actions, links, focus states |
| `color-secondary` | — | Supporting actions, accents |
| `color-neutral-50` | — | Lightest backgrounds |
| `color-neutral-100` | — | Subtle backgrounds, hover states |
| `color-neutral-500` | — | Body text secondary, icon defaults |
| `color-neutral-900` | — | Body text primary |
| `color-success` | — | Confirmation, completed states, positive status badges |
| `color-warning` | — | Caution states, non-blocking errors, warning badges |
| `color-error` | — | Validation errors, destructive actions, error badges |
| `color-surface` | — | Card and panel backgrounds |
| `color-bg` | — | Page background |

> **App-specific role colors:** Add rows for any status-enum-to-color mapping. Example: `color-status-draft` → `color-neutral-500`; `color-status-approved` → `color-success`. Every Schema status enum value must map to a token.

> **Dark mode:** [In scope for MVP / Deferred to Phase N — explicit decision required, not "maybe"]

**Typography:**

| Token | Font | Size | Weight | Line Height | Usage |
|-------|------|------|--------|-------------|-------|
| `text-h1` | — | — | — | — | Page titles |
| `text-h2` | — | — | — | — | Section headings |
| `text-h3` | — | — | — | — | Card/panel headings |
| `text-body` | — | — | — | — | Body copy |
| `text-body-strong` | — | — | — | — | Emphasized body, labels |
| `text-small` | — | — | — | — | Helper text, captions, metadata |
| `text-code` | — | — | — | — | Code snippets, IDs, monospaced values |

**Spacing scale:**

| Token | Value | Usage |
|-------|-------|-------|
| `spacing-xs` | — | Tight groupings, icon padding |
| `spacing-sm` | — | Internal component padding |
| `spacing-md` | — | Between related elements |
| `spacing-lg` | — | Between sections |
| `spacing-xl` | — | Page margins, major layout gaps |

**Core components — primitives:**

> These are the lowest-level components in the design system. They are used everywhere. Higher-level shared components (e.g., `UserCard`, `AppointmentSlotPicker`) are inventoried in the Shared Component Inventory section after Screens are drafted.

| Component | Variants | Notes |
|-----------|----------|-------|
| Button | Primary, Secondary, Destructive, Ghost, Icon-only | [Disabled state: opacity? color shift? Loading state: spinner inside?] |
| Input — Text | Default, Focused, Disabled, Error | [Inline label or stacked? Helper text position?] |
| Input — Select | Single, Multi, Searchable | [Native select or custom? Mobile behavior?] |
| Input — Checkbox / Radio / Toggle | — | [Standalone or always with label?] |
| Card | Default, Interactive (clickable), Selected | [Elevation token? Border token? Both?] |
| Badge / Tag | One variant per status enum value + neutral default | [Reference status mappings in color table above] |
| Modal | Confirm, Form, Detail | [Backdrop dismissal? ESC closes?] |
| Drawer | Side panel, Full-screen sheet (mobile) | [Slide direction? Width breakpoints?] |
| Toast / Alert | Success, Warning, Error, Info | [Auto-dismiss timing? Manual close?] |
| Empty state | Generic, Action-prompting, No-search-results | [Illustration vs icon vs text-only? Standardize across list views.] |
| Loading | Skeleton (layout-matching), Spinner, Progress bar | [Where each is used] |

**Design file:** [Figma link / Sketch / other — or "Not started"]

---

## 🚦 Gate 1 — Design System Locked

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off.**
>
> This gate exists because every Screen description below will reference token names. If tokens aren't locked, screens get described in terms of raw colors and component variants that haven't been decided. That ambiguity propagates into every downstream coding doc. Locking tokens first prevents that.
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] Every color cell has a value — no blanks
- [ ] Every status enum value from the Schema maps to a color token
- [ ] Dark mode is explicitly In Scope or Deferred — no ambiguous answers
- [ ] Every typography role has font, size, weight, and line height
- [ ] Every spacing token has a value
- [ ] Every core component has its variants enumerated — no "etc."
- [ ] Color contrast — text tokens against bg/surface tokens meet WCAG AA (4.5:1 body, 3:1 large)
- [ ] Empty state pattern is decided (illustration / icon / text-only) — this will be reused on every list view

**Sign-off:**
> 🚦 **Gate 1** — Design System is complete and locked. Every token is named with a value. Every status enum has a color mapping. Ready to draft Screens.
>
> **Human sign-off:** ☐ Approved — proceed to Screens

---

## Screens / Pages

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define every screen the user can land on. Every PDD User Workflow ends on a screen — that screen must exist here. Every screen has all four states fully described. Every screen lists its re-fetch triggers. Skipping any of these produces invented UI at code time.
>
> **This is the largest, densest section in this doc. Take it section by section. Don't try to fill all screens in one pass — fill one fully, then the next.**
>
> **A complete Screen block covers:**
> - **Route** — the URL pattern
> - **Purpose** — what user goal does this screen serve (trace to a PDD Persona's goal)
> - **Arrives here when** — every way a user can land on this screen (direct nav, redirect after action, deep link, link from another screen)
> - **Permissions** — who can see this screen (trace to Tech Spec roles). If unauthenticated users land here, what do they see?
> - **Key elements** — every distinct UI element that appears on this screen. Pull entity fields from the Schema — don't generalize ("shows project info" is incomplete; "shows `project.name`, `project.status` (as badge), `project.created_at` (as relative time), `project.owner.full_name`" is complete).
> - **Primary actions** — the buttons/links that trigger writes. Each must map to a Tech Spec endpoint (will be cross-referenced in Screen → API Map).
> - **States table** — all four: Loading, Empty, Error, Populated. Each must be fully specified.
> - **Re-fetch triggers** — every condition that causes data on this screen to refresh.
> - **Responsive behavior** — what changes at each breakpoint.
> - **Wireframe / Design reference** — link or explicit "TBD"
>
> **The four states — what "complete" means for each:**
>
> 1. **Loading** — How does the screen appear while data is fetching? Skeleton matching layout? Spinner scoped to a region? Full-page spinner? Different loaders for first load vs re-fetch?
> 2. **Empty** — What does the user see when there's no data yet? This is the most-skipped state. Empty must include a message AND a clear next action (CTA to create the first item, or guidance on what to expect).
> 3. **Error** — What does the user see when the data fetch fails? Inline error with retry, full-screen error, or fallback to cached data? This must cross-reference the Error Handling section in the Tech Spec — the error response format determines what error info is available to display.
> 4. **Populated** — The normal view. The Key Elements section covers what's shown; the Populated state row describes any layout-level behavior (pagination, infinite scroll, sort order, etc.).
>
> **Re-fetch triggers — what to cover:**
> - User action that mutates this entity (create, update, delete) → optimistic update or full re-fetch?
> - User action elsewhere that affects this screen (e.g., archiving an item from another screen)
> - Polling interval if real-time data is needed
> - WebSocket events if real-time is implemented
> - Returning to this screen via navigation (cached or fresh?)
> - Background sync after going offline
>
> If a screen has only one re-fetch case ("on mount, never again"), say so explicitly. Don't leave it blank.
>
> **Incomplete looks like:**
> - A screen with only the Populated state described
> - A screen with no Empty state ("no data" is a real state on every list view — design it)
> - A screen with no Error state ("the API returns 500" is a real condition — what does the user see?)
> - A list view with no sort order, no filter behavior, no pagination
> - Key elements described in prose ("shows project info") instead of explicit field references
> - A primary action with no corresponding endpoint (flag — Tech Spec is missing it, or this screen is doing something undefined)
> - Re-fetch triggers row empty
> - A screen that no PDD workflow lands on (why does it exist?)
>
> **Ask triggers — stop and ask the human if:**
> - A workflow step says "user sees their items" but doesn't specify sort, filter, or grouping
> - A screen displays a status badge but the visible filter options aren't specified
> - It's unclear which fields from a Schema entity are shown vs hidden
> - A user can create an item from this screen — does the create UI live here (inline) or on a separate screen (modal/route)?
> - A screen has different behavior for different personas — should it be one screen with conditional UI or multiple screens?
> - A screen has a search/filter feature but the searchable fields and filter options aren't specified
> - Pagination behavior is unspecified (page numbers? infinite scroll? load more button?)
> - A real-time element is implied but the update mechanism (polling, WebSocket) isn't specified
>
> **Cross-reference checklist for this section:**
> - Every PDD User Workflow's terminal step lands on a screen defined here
> - Every screen's permissions match a Tech Spec Auth role
> - Every Key Element that displays an entity field references the field name from the Schema
> - Every primary action maps to a Tech Spec endpoint (verified in Screen → API Map below)
> - Every status badge variant displayed references a status enum value from the Schema
>
> **Remove this block before delivering the filled doc.**

> For each screen: what it's for, who arrives here and how, what's on it, how each state looks, when data re-fetches.
> Cross-screen flow goes in Navigation / IA below. Endpoint mapping goes in Screen → API Map below.

### Screen: [Name] (`/route`)

**Purpose:** [What user goal does this screen serve? Trace to a PDD Persona's primary goal.]

**Permissions:** [Which roles can see this screen? Trace to Tech Spec Auth. What happens if an unauthenticated user lands here?]

**Arrives here when:**
- [Direct navigation — e.g., from sidebar link]
- [Redirect after — e.g., redirected here after login if user has projects]
- [Deep link — e.g., shared URL with `:id`]
- [Linked from — e.g., clicking a row in the list view]

**Key elements:**

| Element | What it shows | Notes |
|---------|--------------|-------|
| [Element name] | [Specific entity fields — e.g., `project.name`, `project.status` as badge, `project.updated_at` as relative time] | [Truncation rules? Click behavior?] |
| [Element name] | [Specific content] | — |

**Primary actions:**

| Action | What it does | Endpoint (cross-ref Tech Spec) |
|--------|-------------|-------------------------------|
| [e.g., "Create Project" button] | [Opens modal / navigates to /projects/new] | [`POST /api/projects`] |
| [e.g., "Delete" on row hover] | [Triggers confirm modal, then delete] | [`DELETE /api/projects/:id`] |

**States:**

| State | What triggers it | What the user sees |
|-------|-----------------|-------------------|
| Loading | [First load? Re-fetch after action? Both?] | [Skeleton matching layout / spinner / full-page] |
| Empty | [No data exists yet — e.g., user has no projects] | [Empty state message + specific CTA — e.g., "No projects yet. Create your first project →"] |
| Error | [Fetch failed — Tech Spec error response] | [Error treatment — inline retry, full-screen error, fallback to cache. Reference Error Handling format from Tech Spec.] |
| Populated | [Data loaded successfully] | [Layout notes — sort order, pagination behavior, grouping, default filter state] |

> ⚠️ Every screen needs all four states. The Empty state is the most-skipped — if this screen can ever have no data, design the empty state.

**Re-fetch triggers:**

| Trigger | What re-fetches | Notes |
|---------|----------------|-------|
| [User action on this screen — e.g., create item] | [Data that must refresh] | [Optimistic update or full re-fetch?] |
| [User action elsewhere — e.g., archived from another screen] | [Data that must refresh] | [How is this screen notified?] |
| [Interval — e.g., every 30s] | [Data that must refresh] | [Only if real-time data is needed — confirm necessity] |
| [WebSocket event — name event] | [Data that must refresh] | [Cross-ref Tech Spec Real-Time Events] |
| [Return to screen via nav] | [Data that must refresh — or "Cached, no re-fetch"] | — |
| [Background sync after offline] | [—] | [Only if offline mode is in scope] |

> If a screen has only one re-fetch case ("on mount, never again"), say so explicitly. Don't leave this blank.

**Responsive behavior:**

| Breakpoint | Layout changes |
|------------|---------------|
| Mobile | [What changes — nav collapses, columns stack, table → card list, etc.] |
| Tablet | [What changes] |
| Desktop | [Full layout] |

**Wireframe / Design reference:** [Figma link / description / "TBD"]

---

### Screen: [Name] (`/route`)

*(Copy block above for each screen. Every PDD User Workflow must land on a screen documented here.)*

---

## Shared Component Inventory

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Identify every component that appears on 2+ screens (or has its own state/data) and pull it into a reusable inventory. This section is filled AFTER Screens above. If you find yourself describing the same component on multiple screens, it belongs here.
>
> **Strict ordering rule:** Do not fill this section before Screens is drafted. You cannot know what's shared until you've seen what's used. If the human asks to fill this first, push back — read it back: "We need at least the major screens drafted before we can identify what's truly shared. Can we start with the top 3 screens?"
>
> **A complete Shared Component Inventory covers:**
> - **Registry table** — one row per shared component, listing which screens it appears on
> - **Detail blocks** — one per non-trivial shared component (those with their own state, data fetching, or complex variants). Simple display components (e.g., `UserAvatar` that renders an image and initials) don't need a detail block — a registry row is enough.
>
> **What qualifies as "shared":**
> - Appears on 2+ screens, OR
> - Has its own state (local state, fetched data, derived state), OR
> - Has 2+ variants that change behavior (not just style — actual different behavior), OR
> - Has logic complex enough to warrant isolation (e.g., a multi-step form, a virtualized list)
>
> **What does NOT belong here:**
> - Core primitives (Button, Input, Card) — those live in Design System above
> - Components that appear on one screen and only one screen with no reuse potential
> - Layout containers that are purely structural (e.g., `PageHeader` that wraps a title — unless it has logic)
>
> **A complete Component Detail block covers:**
> - **Used on** — which screens (cross-reference Screens section)
> - **Description** — one sentence, behavioral not visual
> - **Props / Inputs** — every prop with type and required/optional. The Component/Service Map downstream uses this directly.
> - **Internal state** — what state this component manages locally (open/closed, active tab, search filter, etc.)
> - **Data dependencies** — does it fetch its own data, or does the parent pass everything down? If it fetches: which endpoint (cross-ref Tech Spec).
> - **Variants / States** — every visual or behavioral variant
> - **Interactions** — what user actions it responds to, what it emits back to the parent
> - **Edge cases** — overflow, max items, keyboard behavior, accessibility specifics
>
> **Module Breakdown signal:** Flag components for the downstream Module Breakdown:
> - Appears on 3+ screens → its own Module entry (type: `Component`)
> - Manages its own data fetching → its own Module entry, paired with the corresponding service
> - Has 3+ variants with different behavior → its own Module entry
>
> **Incomplete looks like:**
> - This section filled before Screens are drafted (impossible to know what's shared)
> - A component used on 3 screens but missing from the registry
> - A detail block with no Props table (the Component/Service Map can't be built from this)
> - "Internal state: TBD" — either it has state or it doesn't, and if it does, list it
> - A component that fetches data with no endpoint listed
> - Variants listed without describing when each is shown
>
> **Ask triggers — stop and ask the human if:**
> - A component appears on multiple screens but the human's descriptions of it on each screen are subtly different — is this one component with variants, or multiple similar components?
> - A component manages state but who owns the state (component or parent) isn't clear
> - A component has data dependencies — does it fetch itself or accept data via props? This is an architectural decision worth flagging.
> - A component appears once but is described with significant complexity — is it shared in spirit even if not yet in fact (likely to be reused in Phase 2)?
>
> **Cross-reference checklist for this section:**
> - Every component listed appears on the screens named in "Used On"
> - Every component that fetches data references a Tech Spec endpoint
> - Every component prop type matches the Schema field type if it displays an entity field
> - Every variant referenced in a Screen description above appears in this component's variants table
>
> **Remove this block before delivering the filled doc.**

> **What this is:** Feature-level components that appear on more than one screen (or have their own state/data) and need to be built once and reused. These sit above primitives (Button, Input) — examples: `UserCard`, `ActivityFeed`, `StatusBadge`, `AppointmentSlotPicker`.
>
> **When to fill this in:** After the Screens section is drafted. Read through every screen, find recurring UI patterns, and pull them here. If you'd build the same thing in two different feature modules, it belongs in this list.
>
> **Why this matters:** Without this list, the same component gets built 2–3 times with inconsistent behavior. Catching shared components at design time means they get built once — in their own module — before the features that depend on them.

### Component Registry

> One row per shared component. "Shared" means used on 2+ screens OR has logic complex enough to warrant isolation (its own state, data fetching, or non-trivial behavior).

| Component | Used On | Description | Key Inputs | Notes |
|-----------|---------|-------------|------------|-------|
| `[ComponentName]` | [Screen A, Screen B] | [What it renders / does — behaviorally] | [Data shape or props it needs] | [Variants? Stateful? Fetches own data?] |
| `[ComponentName]` | [Screen A, Screen C, Screen D] | — | — | — |

> **Module Breakdown signal:** If a component appears on 3+ screens, OR has more than 2 variants, OR manages its own data fetching — flag it for its own entry in the Module Breakdown (type: `Component`).

### Component Detail Blocks

> One block per non-trivial shared component. Skip simple display components (a `UserAvatar` that renders an image and initials doesn't need a block). Write a block when the component has its own state, fetches data, has complex variants, or has behavior that could go wrong.

---

#### `[ComponentName]`

**Used on:** [Screen A, Screen B — must match the rows in the registry above]
**Description:** [What it does — one sentence, behavioral, not just visual]

**Props / Inputs:**

| Prop | Type | Required? | Description |
|------|------|-----------|-------------|
| `[prop]` | `[string / number / object / array / etc.]` | Yes / No | [What it controls or displays. If it's an entity field, reference Schema.] |

**Internal state (if any):**
- [State this component manages locally — e.g., "open/closed toggle", "active tab index", "local search filter"]
- [Or: "None — purely display, all state from props"]

**Data dependencies:**
- [Does it fetch its own data, or does the parent pass everything down?]
- If it fetches: which endpoint (cross-reference Tech Spec API Endpoints).
- [Or: "None — receives data via props"]

**Variants / States:**

| Variant / State | When shown | What the user sees |
|----------------|-----------|-------------------|
| Default | Normal render | — |
| Loading | Waiting for data | Skeleton / spinner |
| Empty | No data to display | Empty state message (see Design System empty state pattern) |
| Error | Failed fetch or action | Inline error + retry |
| [App-specific variant] | [Condition that triggers it] | [Visual / behavioral difference] |

**Interactions:**

| User action | What the component does | What it emits to parent |
|-------------|------------------------|------------------------|
| [Click row] | [Highlights, expands] | [`onSelect(id)`] |
| [Drag item] | [Reorders locally] | [`onReorder(newOrder)`] |
| [Type in search field] | [Filters internal list] | [Nothing — internal only] |

**Edge cases:**
- **Truncation:** [What happens if text overflows? Ellipsis at what character count?]
- **Max items:** [What happens if the list is very long? Virtualize? Paginate? Show "X more"?]
- **Keyboard behavior:** [Tab order, Enter/Space activation, Escape close, arrow key navigation if applicable]
- **Accessibility:** [ARIA roles, screen reader announcements for state changes]

---

#### `[ComponentName]`

*(Copy block above for each non-trivial shared component)*

---

## Navigation / Information Architecture

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Map the full navigation structure — the screen tree, modals/drawers that overlay screens, and deep link / auth behavior for direct URL access. The downstream coding agents use this to configure routing and route guards.
>
> **A complete Navigation section covers:**
> - **Screen Tree** — the hierarchical map of all routes. Every Screen defined above appears in this tree.
> - **Modals & Drawers** — overlays that aren't full routes. Each must specify trigger, pattern (modal/drawer), and content.
> - **Deep Links / Direct URL Access** — for every route, can it be accessed without auth? If not, where does it redirect?
>
> **Pull from PDD:**
> - Personas → drives which screens each role can navigate to
> - Workflows → drives the linked-from relationships between screens
>
> **Pull from Tech Spec:**
> - Auth roles → drives the permission column in deep links table
> - Public routes (if any) → drives unauthenticated access decisions
>
> **Incomplete looks like:**
> - A Screen defined above that doesn't appear in the tree
> - A modal mentioned in a Screen's actions but not listed in the Modals table
> - A route with no auth decision (can it be accessed publicly? if not, where does it redirect?)
> - "All routes require auth" without a redirect destination
>
> **Ask triggers — stop and ask the human if:**
> - The screen tree has unclear hierarchy — is Settings a top-level nav item or nested under Profile?
> - Whether a confirm action uses a modal or full-screen route is unclear
> - Whether sharing a deep link to an entity preserves filter state, scroll position, etc.
> - A screen has a primary action that could be a modal OR a route — which is it?
>
> **Remove this block before delivering the filled doc.**

### Screen Tree

> Every Screen defined above appears here. The hierarchy should match the navigation pattern (sidebar/tabs/etc.) defined in Overview.

```
[Root / Home]
├── [Screen A]
│   ├── [Sub-screen A1]
│   └── [Sub-screen A2]
├── [Screen B]
│   └── [Sub-screen B1]
└── [Settings / Profile]
    ├── [Sub-screen]
    └── Logout
```

### Modals & Drawers

> These aren't in the tree — they overlay the current screen. For each, specify trigger, pattern, and content.

| Trigger | UI Pattern | Content | Dismissal |
|---------|-----------|---------|-----------|
| [Action — e.g., "Delete Project" button] | Modal | [Confirm dialog with project name, Cancel / Delete buttons] | [Backdrop click? ESC? Cancel button only?] |
| [Action — e.g., row click on item list] | Drawer (side) | [Detail panel with edit form] | [—] |
| [Action — e.g., "Filters" button on list view] | Drawer (side) | [Filter form] | [—] |

### Deep Links / Direct URL Access

> For every route, declare auth requirements and redirect behavior.

| Route | Accessible without auth? | Redirect if no auth? | Notes |
|-------|--------------------------|----------------------|-------|
| `/` | [Yes / No] | [Destination — e.g., `/login`] | — |
| `/dashboard` | No | `/login?return=/dashboard` | Preserve return URL |
| `/[resource]/:id` | [Yes / No] | [Destination] | [Public read? Auth-only?] |
| `/[resource]/:id/edit` | No | `/login` | Role-gated to owner or admin |

> ⚠️ Every Screen above must have at least one row in this table.

---

## Screen → API Map

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Build the bridge between every Screen and the Tech Spec's API Endpoints. This section makes the data-fetching contract explicit so the Component/Service Map agent and the frontend coding agent don't have to mentally join two docs while building.
>
> **Prerequisite:** Tech Spec API Endpoints must be signed off before this section can be reliably completed. If Tech Spec is still in draft, fill this with TBDs and revisit after Tech Spec Gate 1 closes.
>
> **A complete Screen → API Map covers:**
> - Every Screen defined above has at least one row here
> - Every row lists: Screen, what data is displayed, the read endpoint(s), user actions that trigger writes, and the write endpoints
> - If a screen calls 3+ endpoints, break it into multiple rows — one per data concern (don't cram into a single cell)
> - Every endpoint listed here must exist in the Tech Spec API Endpoints section
>
> **Incomplete looks like:**
> - A Screen above with no row here
> - An endpoint listed here that doesn't appear in the Tech Spec — that's a missing endpoint, flag it back to Tech Spec
> - A Screen's primary action that writes data but no Write endpoint in the row
> - A list view with no read endpoint specified (every list view reads data)
> - Endpoints written as descriptions ("the projects endpoint") instead of method + path (`GET /api/projects`)
>
> **Ask triggers — stop and ask the human if:**
> - A screen needs data that no Tech Spec endpoint provides — Tech Spec is missing it, flag
> - A screen displays related data (e.g., user list with their last login time) — is this one endpoint (joined) or multiple (client-side join)?
> - A screen has a search/filter feature — does the search endpoint exist or does it need to be added?
> - A screen displays computed/derived data — is the computation client-side, or is there a dedicated endpoint?
>
> **Cross-reference checklist for this section:**
> - Every Screen defined above appears in at least one row
> - Every endpoint listed appears in Tech Spec → API Endpoints
> - Every Screen primary action maps to a Write endpoint here (or none if read-only)
> - Every Screen "Re-fetch trigger" implies an endpoint that's listed here
>
> **Remove this block before delivering the filled doc.**

> **This is the bridge between UI and Tech Spec.** Every screen, what data it needs, which endpoints it calls, what user actions trigger writes.

| Screen | Data Needed | Read Endpoint(s) | User Action | Write Endpoint |
|--------|-------------|-----------------|-------------|----------------|
| [Screen name] | [What data is displayed — reference entities and fields] | `GET /api/[resource]` | [Action — e.g., "Create item"] | `POST /api/[resource]` |
| [Screen name] | [Different data concern — break out per concern if 3+ endpoints] | `GET /api/[resource]/:id` | [Action — e.g., "Update name"] | `PATCH /api/[resource]/:id` |
| [Screen name] | [What's displayed] | [Multiple endpoints if needed — list separately] | [Action] | [Endpoint] |

> If a screen calls 3+ endpoints, break it into multiple rows — one per data concern. Don't cram into one cell.

---

## 🚦 Gate 2 — Screens + Components + API Map Complete

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off.**
>
> This gate is the critical mid-doc checkpoint. Everything above is the "what the user sees" foundation; everything below is interaction patterns, accessibility, and motion that layer on top. If the foundation isn't solid, the layers don't matter.
>
> The four highest-leverage things to verify here:
> 1. Every screen has all four states (Loading, Empty, Error, Populated)
> 2. Every screen has re-fetch triggers documented
> 3. Shared Component Inventory was filled AFTER Screens and reflects what's actually shared
> 4. Every screen maps to Tech Spec endpoints
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] Every PDD User Workflow's terminal step lands on a Screen defined in this doc
- [ ] Every Screen has Purpose, Permissions, Arrives Here When, Key Elements, Primary Actions, all four States, Re-fetch triggers, and Responsive behavior
- [ ] Every Screen has an Empty state described (no "TBD" — every list view has this state)
- [ ] Every Screen has an Error state described referencing the Tech Spec error format
- [ ] Every Key Element that displays entity data references specific Schema field names
- [ ] Every Screen Permissions row matches a Tech Spec Auth role
- [ ] Shared Component Inventory has at least one detail block for every component that appears on 3+ screens or has its own state/data
- [ ] Every component used on a Screen appears in either Design System (primitives) or Shared Component Inventory (feature components)
- [ ] Every Screen appears in the Navigation Screen Tree
- [ ] Every Screen has at least one row in the Screen → API Map
- [ ] Every endpoint in Screen → API Map exists in Tech Spec → API Endpoints
- [ ] Every Screen primary action maps to a write endpoint in Screen → API Map (or is explicitly read-only)
- [ ] Every modal/drawer used on a Screen is listed in the Navigation Modals & Drawers table

**Sign-off:**
> 🚦 **Gate 2** — Screens, Shared Components, Navigation, and Screen → API Map are complete and cross-referenced. The "what users see and how it connects" foundation is locked. Ready for interaction patterns, accessibility, and motion.
>
> **Human sign-off:** ☐ Approved — proceed to User Interactions

---

## User Interactions / State Management

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Lock the patterns for how the app responds to user input. This section is where coding-time inconsistency lives if it's left vague. Without explicit patterns, two devs (or two AI coding sessions) build the same kind of interaction differently — some forms validate on blur, some on submit; some destructive actions confirm, some don't; some errors show toasts, some show inline messages.
>
> **The Error States table is the most critical part of this section.** It is the single biggest source of invented UI at code time. Every user-initiated action that can fail must have a pattern row.
>
> **A complete User Interactions section covers:**
> - **Form Behavior** — validation timing, error display, required indicators, submit behavior, success behavior
> - **Loading States** — patterns for page-level, inline, and action-level loading
> - **Optimistic UI** — which actions are optimistic, and what rollback looks like
> - **Destructive Actions** — confirmation pattern for every irreversible action
> - **Error States** — comprehensive table covering every meaningful failure
>
> **Pull from PDD:**
> - User Workflows → drives which forms exist and their failure modes
> - Workflow Error Paths → drives the Error States table (every error path in a workflow is a row here)
>
> **Pull from Tech Spec:**
> - Error Handling format → drives what error info is available to display
> - State Machines → drives confirmation patterns (terminal/irreversible state transitions get confirmations)
> - Auth → drives session-expiry and 401/403 UI behavior
>
> **Pull from Schema:**
> - Field constraints (required, max length, unique) → drives validation rules
>
> **Form Behavior — what to lock:**
> - **Validation timing** — on submit only / on blur / real-time / on submit, then real-time after first attempt
> - **Error display** — inline below field / form summary at top / both
> - **Required indicator** — asterisk in label / "(required)" suffix / different colored label
> - **Disabled submit** — disabled while submitting only / disabled while invalid / never disabled (always submit, show errors)
> - **Success action** — redirect / toast + stay / toast + reset form / inline confirmation
>
> Pick one per row. "Depends on the form" is not an answer — if forms vary, document each variant.
>
> **Loading States — what to lock:**
> - **Full page load** — skeleton matching layout (preferred) / spinner / blank
> - **Inline data fetch** — scoped spinner / scoped skeleton / fade
> - **Button action in progress** — spinner inside button + disabled / spinner overlay + disabled / disabled only
> - **Background sync** — subtle indicator / none
> - **Timeout policy** — after how many seconds does a loading state become an error with retry?
>
> **Optimistic UI — what to lock:**
> - Which actions show success immediately before server confirmation
> - Rollback behavior on failure (revert + toast? revert + inline error?)
> - Default for unspecified actions: NOT optimistic (server-first)
>
> **Destructive Actions — what to lock:**
> - Every irreversible action gets a confirmation pattern
> - Patterns: modal with type-to-confirm / modal with confirm button / modal with delay before button enables / no confirmation but undo toast
> - Default: modal with confirm button
>
> **Error States — the comprehensive table:**
> Every user-initiated action that can fail must have a row. The table specifies the UI pattern, not the copy. Copy lives in the UI Strings doc.
>
> Categories of failures to cover (at minimum):
> - Authentication failures (login, session expiry, 401)
> - Authorization failures (403)
> - Validation failures (per field, per form)
> - Resource-not-found failures (404)
> - Conflict failures (409 — e.g., duplicate, stale data)
> - Server errors (500, network timeout, unreachable)
> - Action-specific failures (e.g., payment declined, file too large, rate limited)
>
> For each: which UI pattern, and what the user does to recover.
>
> **Incomplete looks like:**
> - Form Behavior with "depends" answers — pick a default, document exceptions explicitly
> - No timeout policy for loading states
> - Optimistic UI not addressed — either decide it's not in scope or list which actions are optimistic
> - A destructive action in a Screen's primary actions with no confirmation pattern here
> - Error States table missing categories — every category above should have at least one row
> - Error message copy written in this section — copy belongs in UI Strings, this section is pattern-only
>
> **Ask triggers — stop and ask the human if:**
> - The app has forms but no validation timing preference is stated
> - A destructive action exists but the human is unclear on whether to confirm (e.g., "users can delete items" — modal? undo toast? both?)
> - Optimistic updates are mentioned but the rollback behavior isn't specified
> - A failure mode is implied by a Tech Spec endpoint (e.g., 409 conflict) but no UI pattern is decided
> - Session expiry behavior is unspecified — does the user lose their in-progress work?
> - A user action can take a long time (file upload, report generation) — is there a progress indicator? a background notification on completion?
>
> **Cross-reference checklist for this section:**
> - Every Workflow Error Path in the PDD maps to a row in the Error States table
> - Every destructive action on a Screen has a confirmation pattern here
> - Every form on a Screen follows the Form Behavior rules (or documents an explicit exception)
> - Every Error pattern references the Tech Spec Error Handling format (uses `message` field for display, `details` for field-level)
>
> **Remove this block before delivering the filled doc.**

### Form Behavior

> Defaults that apply to every form unless an exception is documented inline.

| Behavior | Rule |
|----------|------|
| Validation timing | [On submit only / on blur / real-time / on submit, then real-time after first attempt] |
| Error display | [Inline below field / form summary at top / both — when does each apply?] |
| Required field indicator | [Asterisk in label / "(required)" suffix / colored label] |
| Disabled submit | [While submitting only / while invalid / never disabled] |
| Success action | [Redirect to detail / toast + stay / toast + reset form / inline confirmation] |
| Multi-step form behavior | [Save draft per step / save only on final submit / no multi-step forms in scope] |

**Exceptions to defaults:** [List any forms that deviate from the rules above, with rationale.]

### Loading States

| Context | Pattern | Notes |
|---------|---------|-------|
| Full page load | [Skeleton matching layout / spinner / blank] | [Same pattern for first load and route navigation?] |
| Inline data fetch | [Scoped spinner / scoped skeleton / fade] | [Where shown — within the component, not full-page] |
| Button action in progress | [Spinner inside button + disabled / overlay + disabled] | Prevents double-submit |
| Background sync | [Subtle indicator / none] | [Where shown — toast? status bar?] |

**Timeout policy:**
- Loading state visible for more than **[X] seconds** → switch to error state with retry option
- Don't leave the user on an infinite spinner

### Optimistic UI

> Locking which actions show success before server confirms.

| Action | Optimistic? | Rollback behavior |
|--------|------------|------------------|
| [e.g., Toggle favorite] | Yes | [Revert visual + show error toast] |
| [e.g., Create item] | [Yes / No] | [Revert visual + show inline error + restore form data] |
| [e.g., Delete item] | [Yes / No] | [Restore item to list + show error toast] |

**Default for unspecified actions:** Not optimistic — wait for server confirmation, show button loading state in the meantime.

### Destructive Actions

> Every irreversible or significantly destructive action requires explicit confirmation.

| Action | Confirmation pattern | Notes |
|--------|---------------------|-------|
| Delete [entity] | Modal with Cancel / Delete buttons | [Button colors? Type-to-confirm for very destructive cases?] |
| [Other irreversible action] | [Pattern from above] | [—] |
| [Bulk delete] | [Heavier confirmation — type-to-confirm? Show count?] | [—] |

**Default pattern:** Modal with Cancel + destructive-styled action button. Type-to-confirm is reserved for actions affecting many records or with high impact.

### Error States — User-Facing Patterns

> **What this is:** For every meaningful failure, what does the user actually see? This is distinct from API error codes (which live in the Tech Spec) — this is the UI response to those failures.
>
> **Why this matters:** Without this, error UI gets invented during coding — inconsistently. Some errors get toasts, some get inline messages, some get nothing. This table locks the pattern. The actual error copy lives in the UI Strings doc — do not write message text here.

**Standard error patterns — the palette:**

| Pattern | When to use | Anatomy |
|---------|------------|---------|
| Inline field error | Validation failure on a specific input | Red text below field + icon + ARIA-live announcement |
| Form-level error | Submit failed for a non-field reason | Banner above submit button or at top of form |
| Toast (error) | Action failed but user can continue working | Bottom or top toast, dismissable, auto-hide after [X]s |
| Full-screen error | Page-level data fetch failed | Centered illustration/icon + message + retry button |
| Inline component error | Component-level fetch failed (one card on a multi-card page) | Within component bounds — error message + retry |
| Modal error | Critical failure requiring acknowledgment | Modal with single OK button |
| Banner / persistent alert | App-level state (e.g., offline, version mismatch) | Top banner, persistent until condition resolves |

**Error inventory — every failure scenario:**

> Define the *pattern* for each scenario (which UI treatment from the palette above). The actual error copy lives in the UI Strings doc — do not write message text here.

| Scenario | Trigger | UI pattern | Recovery action |
|----------|---------|-----------|----------------|
| Login fails (wrong credentials) | 401 on login | Inline field error | Try again — preserve email, clear password |
| Session expires mid-session | 401 on any authenticated request | Toast + redirect to login | Return to same screen after re-auth (preserve form state where possible) |
| Network request times out | Client-side timeout | Toast (error) | Retry button |
| Network unreachable (offline) | No connectivity | Banner / persistent alert | App resumes when connectivity returns; queue writes if applicable |
| Resource not found | 404 on detail screen | Full-screen error | Link back to list view |
| Permission denied | 403 | [Pattern] | [Redirect / show "no access" screen] |
| Conflict — duplicate | 409 on create | Inline field error | User edits the conflicting field |
| Conflict — stale data | 409 on update | Modal error | Force refresh, user re-applies changes |
| Server error (500) | 500 on any request | Toast (error) | Retry button |
| Validation failure (single field) | 400 with `details[].field` | Inline field error | User fixes field |
| Validation failure (multiple fields) | 400 with `details[]` array | Inline field errors per field + form-level summary | User fixes all flagged fields |
| Rate limited | 429 | Toast (error) | Wait and retry |
| [App-specific failure — e.g., payment declined] | [Specific endpoint + status] | [Pattern] | [Recovery] |
| [App-specific failure] | [—] | [Pattern] | [Recovery] |

> ⚠️ Every PDD Workflow Error Path must map to at least one row here. Every action a user can take that can fail must be represented.

---

## Accessibility

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Specify how this app meets WCAG 2.1 AA. Generic statements like "we follow best practices" produce inaccessible UIs. Each requirement gets a specific implementation and an enforcement layer.
>
> **A complete Accessibility section covers:**
> - Standard target (WCAG 2.1 AA minimum)
> - Per-requirement: implementation specifics, where enforced (design / dev / automated / manual)
> - Specific patterns for the app's most-used components (forms, modals, data tables)
> - Testing approach
>
> **Pull from PDD:**
> - If personas include any with accessibility needs (screen reader users, motor impairments, low vision), call them out — they raise the bar
> - Compliance requirements (Section 508, etc.) — fold in alongside WCAG
>
> **Pull from Design System:**
> - Color contrast — verify token combinations meet ratios
> - Focus indicator decision — every focusable element needs visible focus
>
> **Ask triggers — stop and ask the human if:**
> - Compliance requirement is named in PDD (e.g., Section 508) but specifics aren't worked out
> - The app uses non-standard interaction patterns (drag-drop, complex data tables) where accessibility is non-trivial
> - Whether automated testing tools (axe, Lighthouse) are part of CI
> - Whether manual screen reader testing is in scope
>
> **Remove this block before delivering the filled doc.**

**Standard:** WCAG 2.1 AA minimum. AAA where practical.

| Requirement | Implementation | Where enforced |
|-------------|---------------|----------------|
| Keyboard navigation | All interactive elements reachable via Tab. Logical tab order matches visual order. Skip-to-content link on every page. | Dev implementation, manual QA |
| Focus indicators | Visible focus ring on all focusable elements. Token `focus-ring`. Never `outline: none` without replacement. | Design spec, dev implementation |
| Screen readers | All images have `alt` text (decorative = `alt=""`). Form inputs have associated `<label>`. Status changes announced via ARIA live regions. ARIA roles only where semantic HTML isn't sufficient. | Dev implementation, manual screen reader testing |
| Color contrast | 4.5:1 for body text, 3:1 for large text and UI components. Verified against Design System tokens. | Design spec, automated (axe) |
| Touch targets | Minimum 44×44px on mobile breakpoint. | Design spec, dev implementation |
| Reduced motion | All animations respect `prefers-reduced-motion`. Provide static fallback. | Dev implementation |
| Error identification | Errors identified by more than color alone (icon + text + ARIA). | Dev implementation |
| Form labels | Every input has a visible label. Placeholder is not a label substitute. | Design spec |
| Modal focus management | Focus trapped within modal. ESC closes. Focus returns to trigger element on close. | Dev implementation |
| Data tables | Proper `<th>` headers, `scope` attribute, caption where applicable. | Dev implementation |

**Testing approach:**
- **Automated:** [axe / Lighthouse / pa11y in CI — which?]
- **Manual:** [Keyboard-only testing per screen / Screen reader testing on critical flows / Both?]
- **Frequency:** [Every PR / pre-release / spot checks?]

---

## Responsive Design

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define breakpoints and per-component responsive behavior. Every Screen's Responsive Behavior table references these breakpoints — they must be locked.
>
> **A complete Responsive Design section covers:**
> - Breakpoint table with name, range, and notes
> - Per-component behavior at each breakpoint for the components most affected (navigation, data tables, forms, modals)
> - Touch vs hover differences
>
> **Mobile-first vs desktop-first:** Pick one as the design baseline. The other is the override. Default for most apps: mobile-first.
>
> **Pull from PDD:**
> - Personas → drives device assumptions (field-worker persona on a phone vs office worker on desktop)
> - Platform constraint → if "web only desktop" or "mobile-first" is declared, lock here
>
> **Ask triggers — stop and ask the human if:**
> - Whether mobile is a first-class target or a "works but not optimized" target is unclear
> - The app has data-dense screens — on mobile, do they degrade to card lists or scroll horizontally?
> - Whether the app uses native mobile features (camera, file picker, geolocation) that need responsive treatment
>
> **Remove this block before delivering the filled doc.**

**Design baseline:** [Mobile-first / Desktop-first] — [rationale]

**Breakpoints:**

| Name | Range | Notes |
|------|-------|-------|
| Mobile | < 640px | — |
| Tablet | 640px – 1024px | — |
| Desktop | > 1024px | — |
| Large desktop | > 1440px | [Optional — only if app has wide-screen-specific layout] |

**Component behavior by breakpoint:**

| Component | Mobile | Tablet | Desktop |
|-----------|--------|--------|---------|
| Navigation | [Bottom tab bar / hamburger / drawer] | [Collapsed sidebar / top nav] | [Full sidebar / top nav] |
| Data tables | [Card list / horizontal scroll / hide non-essential columns] | [Simplified columns] | [Full table] |
| Forms | [Single column, full-width inputs] | [Single or two-column] | [Two-column or constrained width] |
| Modals | [Full-screen sheet] | [Centered modal] | [Centered modal] |
| Drawers | [Full-screen sheet from bottom] | [Side drawer] | [Side drawer] |
| Multi-column layouts | [Stack to single column] | [2 columns] | [Up to N columns] |
| [Other key component] | [—] | [—] | [—] |

**Touch vs hover:**
- Hover-only interactions (e.g., row hover actions) have a touch equivalent (long-press, always-visible, or accessed via menu)
- No critical action is hover-only

---

## Animation / Motion

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define when animation is used, the standard timing scale, and the reduced-motion fallback for every animation pattern. Animation that doesn't communicate a state change is decoration — and decoration distracts. This section keeps motion purposeful.
>
> **A complete Animation section covers:**
> - Philosophy statement
> - `prefers-reduced-motion` rule (mandatory)
> - Timing scale
> - Patterns table with the animation, where it's used, and the reduced-motion fallback
>
> **Pull from Design System:**
> - Reference any motion tokens defined in tokens (often deferred — that's fine, define them here)
>
> **Ask triggers — stop and ask the human if:**
> - The app has signature motion that's important to the brand identity — those need extra spec depth
> - Whether large layout shifts (e.g., expanding cards, reordering lists) get animated
> - Performance budget for animation on lower-end devices
>
> **Remove this block before delivering the filled doc.**

**Philosophy:** Animation communicates state changes, guides attention, and confirms actions — not decoration. When in doubt, skip it.

**`prefers-reduced-motion` rule:** All animations must have a no-motion fallback. Use `@media (prefers-reduced-motion: reduce)` and disable or simplify every transition. Reduced-motion users get instant state changes — no fade, no slide, no scale.

**Timing:**

| Type | Duration | Easing |
|------|----------|--------|
| Micro-interactions (hover, focus, button press) | 100–200ms | ease-out |
| Element enter / exit | 200–300ms | ease-in-out |
| Page transitions | 300–400ms | ease-in-out |
| Loading / progress | — | linear |

**Patterns:**

| Event | Animation | Reduced-motion fallback |
|-------|-----------|------------------------|
| Page transition | [Fade / slide / none] | Instant switch |
| Modal open / close | Fade + scale (0.95 → 1.0) | Instant appear / disappear |
| Drawer open / close | Slide from [direction] | Instant appear |
| Toast notification | Slide in from [top/bottom] | Instant appear |
| Button loading state | Spinner inside button | Static "Loading…" text |
| Skeleton → content swap | Fade in | Instant swap |
| List item add / remove | Fade + height collapse | Instant add/remove |
| Status badge change | [Brief highlight pulse / none] | Instant change |
| [Other] | [—] | [—] |

---

## Design Decisions / Rationale

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Capture significant design decisions and the alternatives that were considered. Like an ADR but for UI/UX. Future Claude (or future Ryan) will read this when wondering "why did we do it this way instead of [obvious other way]?"
>
> **A complete Design Decisions entry covers:**
> - The decision
> - Alternatives considered
> - Rationale (why this option won)
> - Date or phase (so the decision can be revisited in context)
>
> **What qualifies as a "significant" design decision:**
> - Navigation pattern choice (sidebar vs tabs)
> - Form behavior defaults (validation timing, optimistic vs server-first)
> - Confirmation patterns for destructive actions (modal vs undo toast)
> - Density / information architecture trade-offs
> - Non-obvious accessibility choices
> - Departures from common patterns in this app's domain
>
> **What does NOT belong here:**
> - Token values (those are in Design System)
> - Per-screen layout details (those are in Screens)
> - Color choices that follow tokens (only the token decision itself, not its usage)
>
> **Remove this block before delivering the filled doc.**

| Decision | Alternatives Considered | Rationale | Date / Phase |
|----------|------------------------|-----------|-------------|
| [What was decided] | [What else was evaluated] | [Why this one] | [When] |

---

## Future Design Improvements

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Capture deferred design work. Items here are things the human chose to push to a later phase — not bugs, not gaps. Every item has a "why deferred" reason.
>
> **Remove this block before delivering the filled doc.**

- [ ] [Item] — [Why deferred]

---

## 🚦 Gate 3 — Full UI/UX Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off. This is the final gate before the UI Strings doc, Component/Service Map, and the frontend Module Breakdown consume this spec.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] All sections complete — Status table shows no ⏳ or 🔄
- [ ] Gates 1 and 2 are signed off
- [ ] Every PDD Persona has at least one screen designed around their goal
- [ ] Every PDD User Workflow's terminal step lands on a documented Screen
- [ ] Every PDD Workflow Error Path maps to a row in the Error States table
- [ ] Every Schema entity that's displayed appears in a Screen's Key Elements with specific field references
- [ ] Every Schema status enum value has a Badge variant and a color token mapping
- [ ] Every Screen has all four states (Loading, Empty, Error, Populated) — verified, not assumed
- [ ] Every Screen has Re-fetch triggers documented
- [ ] Every Screen has Responsive behavior for Mobile, Tablet, Desktop
- [ ] Every endpoint in Screen → API Map exists in Tech Spec → API Endpoints
- [ ] Every Tech Spec Auth role has at least one Screen that gates to it
- [ ] Every destructive action on any Screen has a confirmation pattern in User Interactions
- [ ] Error States table covers all standard categories (auth, validation, conflict, server error, network)
- [ ] Form Behavior defaults are locked, with exceptions documented
- [ ] Accessibility section has specific implementations (no "best practices" placeholder)
- [ ] Animation patterns all have reduced-motion fallbacks
- [ ] No "TBD" left in any section — open items are in Future Improvements with rationale
- [ ] Every Shared Component referenced in a Screen appears in the Shared Component Inventory

**Sign-off:**
> 🚦 **Gate 3** — UI/UX complete, internally consistent, and traces fully to PDD, Schema, and Tech Spec. Ready for UI Strings extraction, Cross-Doc Validation, and downstream coding-phase docs.
>
> **Human sign-off:** ☐ Approved — UI/UX complete. Proceed to UI Strings.
