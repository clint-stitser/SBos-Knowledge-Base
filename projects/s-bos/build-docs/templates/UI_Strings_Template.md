# UI Strings: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized
> **Fill order:** Fifth design doc — filled after UI/UX is signed off. Every screen, element, state, and error pattern in UI/UX has a corresponding string here.
>
> **Source docs:**
> - `[AppName]_UI_UX.md`
>   - Screens → drives the per-screen sections (one section here per screen there)
>   - Key Elements per screen → drives field labels, headings, button labels
>   - All four states per screen → drives loading text, empty state copy, error state copy, populated state strings
>   - Error States table → drives every error message
>   - Modals & Drawers → drives confirm dialog copy
>   - Form Behavior → drives validation error messages
>   - Optimistic UI / Destructive Actions → drives confirmation and rollback toast copy
> - `[AppName]_Product_Design_Doc.md`
>   - Personas → drives voice/tone calibration (formal vs casual, technical vs plain language)
> - `[AppName]_Technical_Spec.md`
>   - Error Handling format → drives mapping of API errors to user-facing messages
>   - Auth roles → drives any role-specific UI strings
> - `[AppName]_DB_Schema.md`
>   - Entity names and status enums → drives column headers, badge labels, filter options
>
> **Internal fill order within this doc (strict — do not reorder):**
> 1. String Conventions (locks voice, formatting, casing — every string downstream follows these)
> 2. Global / Shared Strings (defined once, referenced everywhere)
> 3. **🚦 Gate 1** — Conventions + Global locked
> 4. Navigation
> 5. Per-Screen sections (one per UI/UX Screen)
> 6. Validation Errors — Master List
> 7. Toast Messages
> 8. Confirm Dialogs
> 9. Optional sections (Onboarding, Export, etc.)
> 10. **🚦 Gate 2** — Full UI Strings sign-off
>
> **Agent role:** Pull every user-facing string from the UI/UX doc and confirm copy with the human. The agent never invents copy — every string traces to either a UI/UX element, a human-confirmed input, or the established String Conventions. The agent's primary value here is **completeness** (every element on every screen has a string) and **consistency** (same concept named the same way across screens, same patterns of casing/punctuation throughout).
>
> **The three rules while filling this doc:**
> 1. Every string traces to a specific UI/UX element, a Tech Spec error response, or human-confirmed copy. No invented copy. If a screen mentions a button with no label confirmed, stop and ask.
> 2. Apply String Conventions to every string. If conventions aren't set yet, that's section 1 — set them before writing any other string.
> 3. Output must be specific enough that the frontend coding agent never has to write a string that isn't here. If it's not here, it doesn't get displayed.
>
> **Two patterns produce the most string-related rework when ignored:**
> - **Voice inconsistency** — "Save" on one screen, "Save Changes" on another, "Update" on a third. Set conventions first, then enforce.
> - **Missing strings for non-happy states** — empty states, error states, and loading states are most commonly skipped. Every state defined in UI/UX needs copy here.
>
> **When this doc is fully filled and approved:** Remove every `🤖 AGENT INSTRUCTIONS` block, every `❓ AGENT PAUSE` prompt, and the agent-facing instruction prose inside `🚦 GATE` blocks. Keep the gate checklists, sign-off lines, and all filled content.
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
| String Conventions | ⏳ Not Started | — | Fill first — locks voice and formatting for everything else |
| Global / Shared Strings | ⏳ Not Started | — | Defined once, referenced everywhere |
| 🚦 Gate 1 — Conventions + Global Locked | ⏳ Not Started | — | — |
| Navigation | ⏳ Not Started | — | — |
| [Screen 1 Name] | ⏳ Not Started | — | Copy screen name exactly from UI/UX |
| [Screen 2 Name] | ⏳ Not Started | — | — |
| [Add a row per screen] | ⏳ Not Started | — | — |
| Validation Errors — Master List | ⏳ Not Started | — | Single cross-screen master list |
| Toast Messages | ⏳ Not Started | — | All success/error toasts |
| Confirm Dialogs | ⏳ Not Started | — | All confirmation modals |
| Onboarding (optional) | ⏳ Not Started | — | Only if app has first-run flow |
| Export / File Content (optional) | ⏳ Not Started | — | Only if app generates files |
| 🚦 Gate 2 — Full UI Strings Sign-Off | ⏳ Not Started | — | — |

**Status scheme:** ⏳ Not Started → 🔄 In Progress → ❓ Needs Discussion → ✅ Done

---

## How to Read This Doc

- **Label:** Text on a UI element (button, field label, column header, section header)
- **Placeholder:** Gray hint text inside an empty input field
- **Helper:** Small text below a field explaining the rule or context
- **Error:** Inline validation error shown below the offending field
- **Empty state:** Text shown when a list or section has no data
- **Toast:** Brief auto-dismissing notification (success or error)
- **Dialog:** Text in a confirmation modal (title + body + buttons)
- **Tooltip:** Hover text on an icon or button

---

## String Conventions for This Project

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Lock the voice, tone, and formatting rules that every other string in this doc will follow. This is the first section filled because it constrains every section after it. Without locked conventions, screens get inconsistent capitalization, button labels drift, and the same concept gets named three different ways.
>
> **A complete String Conventions section covers:**
> - **Voice / tone** — formal vs casual, instructional vs descriptive, "you" or never-second-person, exclamation marks allowed or not, humor allowed or not
> - **Casing** — Sentence case vs Title Case for headings, buttons, nav items, column headers
> - **Punctuation** — Oxford comma, terminal punctuation in buttons, ellipsis usage
> - **Date/time format** — display format (April 28, 2026 or 04/28/2026 or 2026-04-28), relative time (yesterday, 2 hours ago), export format (always ISO)
> - **Number formatting** — thousands separator, decimal places, currency
> - **Truncation** — character limits for known truncation points (table cells, list items, breadcrumbs)
> - **Empty/null display** — em-dash, "(none)", "—", blank — pick one
> - **Brand / product references** — how is the app named in copy? Is it "Icon" or "the Icon app" or "your Icon workspace"?
>
> **Pull from PDD:**
> - Personas → drives voice (medical professionals = formal/precise; consumer app = casual; field workers = brief/direct)
> - Compliance constraints → may dictate formal language (HIPAA-adjacent apps avoid casual error messages)
>
> **Pull from UI/UX:**
> - Design System decisions on density → drives truncation lengths
>
> **Incomplete looks like:**
> - "Sentence case mostly" — pick a rule, then list exceptions
> - "Standard date format" — name it precisely
> - "Friendly tone" — what does friendly mean for this audience? Examples help
> - Conventions that contradict (e.g., Title Case for buttons but a button column shows "Save changes")
>
> **Ask triggers — stop and ask the human if:**
> - The personas span widely different formality preferences (admin user vs end user)
> - Voice/tone preference isn't stated
> - Date format preference isn't stated
> - Whether app name is used in body copy is unclear
> - Multiple locales are in scope (this opens i18n which is bigger than this doc)
>
> **Remove this block before delivering the filled doc.**

| Convention | Rule | Example |
|------------|------|---------|
| Voice / tone | [Formal / casual / instructional / descriptive — pick one and describe what it means] | [Example sentence in the chosen voice] |
| Second person usage | [Use "you" / avoid second person / mixed by context] | [Example] |
| Heading casing | [Sentence case / Title Case] | [Example: "User settings" vs "User Settings"] |
| Button label casing | [Sentence case / Title Case] | [Example: "Save changes" vs "Save Changes"] |
| Nav item casing | [Sentence case / Title Case] | — |
| Column header casing | [Sentence case / Title Case / ALL CAPS] | — |
| Oxford comma | [Yes / No] | — |
| Terminal punctuation in buttons | [None / Period for sentences only / Always] | [Example: "Save" or "Save."] |
| Ellipsis usage | [For truncated text only / Also for actions that open more — e.g., "Add user…"] | — |
| Date format (display) | [e.g., April 28, 2026 / 04/28/2026 / Apr 28] | — |
| Date format (relative) | [e.g., "2 hours ago", "yesterday", "last week"] — when does it switch to absolute date? | [After 7 days, show absolute] |
| Date format (export / file) | ISO 8601 (`YYYY-MM-DD`) — universal | — |
| Time format | [12-hour with am/pm / 24-hour] | — |
| Duration display | [e.g., "X hr Y min" / "Xh Ym" / "2:15"] | — |
| Number formatting | [Thousands separator? Decimal places? Locale-specific?] | — |
| Currency format | [e.g., $X,XXX.XX / X,XXX USD / N/A — no currency in this app] | — |
| Truncation pattern | [Ellipsis at N characters per context — table cells, list items, breadcrumbs] | — |
| Empty / null display | [— (em-dash) / (none) / blank / — pick one] | — |
| App name in copy | [How the app is named in body text — "Icon", "the app", "your workspace", etc.] | — |
| Profanity / negative phrasing | [Avoid "failed" — use "couldn't complete"? Or use direct language?] | — |
| Pronoun for unspecified user | [They / he or she / per persona] | — |

---

## Global / Shared Strings

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Define every string that appears in more than one place. These are the building blocks. Once defined here, every per-screen section references them rather than redefining.
>
> **A complete Global / Shared Strings section covers:**
> - **Standard action labels** — Save, Cancel, Close, Delete, Edit, Add, Create, Update, Submit, Confirm, Continue, Back
> - **Standard loading text** — what shows during loading states (must align with Loading State patterns in UI/UX)
> - **Standard empty fallbacks** — what shows when a single field has no value
> - **App identity strings** — app name, browser title format, copyright/footer if applicable
> - **Standard accessibility strings** — close icon ARIA label, sort direction labels, expand/collapse labels
>
> **Strings that DO NOT belong here:**
> - Screen-specific copy (those live in per-screen sections)
> - Validation errors (those live in the master list)
> - Toast messages (those live in the Toast section)
>
> **The principle:** If the same string would appear in two per-screen sections, lift it here.
>
> **Incomplete looks like:**
> - Action labels split across multiple sections — pull them up here
> - No accessibility strings (close icon, sort labels) — these are commonly forgotten and are required for AA compliance
> - Browser title format missing
>
> **Ask triggers — stop and ask the human if:**
> - "Save" vs "Save Changes" vs "Update" — multiple buttons do the same thing on different screens, but with different labels. Standardize.
> - Whether destructive labels are softer or direct (e.g., "Delete" vs "Remove" vs "Discard")
> - Whether action buttons are verbs only or verb + noun (e.g., "Save" vs "Save Project")
>
> **Remove this block before delivering the filled doc.**

> Strings that appear in multiple places. Define once, reference everywhere. Every per-screen section below should reference these rather than redefine.

### Standard Action Labels

| Context | String |
|---------|--------|
| Save | [Label] |
| Cancel | Cancel |
| Close | Close |
| Delete | [Label — "Delete" or "Remove"?] |
| Edit | [Label] |
| Add | [Label — "Add" or "New" or "Create"?] |
| Submit | [Label] |
| Confirm | [Label] |
| Continue | [Label] |
| Back | [Label — "Back" or "Previous"?] |
| Apply | [Label] |
| Reset | [Label] |
| Retry | [Label] |
| Refresh | [Label] |

### Standard Loading & Fallback Text

| Context | String |
|---------|--------|
| Loading spinner label | [e.g., "Loading…"] |
| Background sync indicator | [e.g., "Saving…" / "Syncing…"] |
| Generic empty fallback | [e.g., "Nothing here yet." or per the Empty display convention] |
| Generic single-field empty | [e.g., "—"] |
| Default error fallback | [e.g., "Something went wrong. Please try again."] |

### App Identity

| Context | String |
|---------|--------|
| App name | [App Name] |
| Browser / window title format | [e.g., `[Page] · [App Name]`] |
| Default page title (no specific page) | [App Name] |
| Footer copyright (if applicable) | [© [Year] [Company]] |

### Accessibility Strings

| Context | String |
|---------|--------|
| Close icon (ARIA label) | Close |
| Open menu (ARIA label) | Open menu |
| Sort ascending | Sorted ascending |
| Sort descending | Sorted descending |
| Expand section | Expand |
| Collapse section | Collapse |
| Required field (screen reader) | required |
| Optional field (screen reader) | optional |
| Skip to main content | Skip to main content |

---

## 🚦 Gate 1 — Conventions + Global Locked

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off.**
>
> This gate exists because every per-screen section below references the global strings and follows the conventions set in those two sections. If conventions drift after per-screen writing starts, the whole doc needs re-passing. Lock them first.
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] Every convention row has a specific rule (no "mostly" or "depends")
- [ ] Casing rule is locked for headings, buttons, nav items, and column headers — each may differ but each is decided
- [ ] Date format is locked for display, relative, export
- [ ] Empty/null display token is decided
- [ ] App name usage in copy is decided
- [ ] Voice/tone has an example sentence so the agent and human agree on what the rule means in practice
- [ ] Every standard action label is filled (Save, Cancel, Delete, etc.)
- [ ] App name and browser title format are decided
- [ ] Accessibility strings are filled — these are not optional for WCAG AA

**Sign-off:**
> 🚦 **Gate 1** — String conventions and global/shared strings are complete and locked. Ready to fill per-screen sections.
>
> **Human sign-off:** ☐ Approved — proceed to Navigation and per-screen sections

---

## Navigation

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Capture every nav label, menu entry, and tab in the order they appear. Cross-reference UI/UX Navigation Screen Tree — every node in the tree that has a visible label appears here.
>
> **A complete Navigation section covers:**
> - Primary nav (sidebar, top nav, or bottom tabs as decided in UI/UX)
> - Secondary nav (sub-nav within sections, if applicable)
> - User menu / profile menu items
> - Logout / sign-out label
>
> **Apply conventions:** Every label uses the Nav item casing rule from String Conventions.
>
> **Ask triggers — stop and ask the human if:**
> - A screen in UI/UX has no nav label decided
> - Nav items have icons — what's the ARIA label for icon-only nav items?
> - Whether nav shows badge counts (e.g., "Inbox 3") and how they format
>
> **Remove this block before delivering the filled doc.**

### [Primary Nav — e.g., Sidebar / Top Nav / Bottom Tabs]

| Order | Label | Icon? (Y/N) | ARIA label if icon-only | Notes |
|-------|-------|------------|------------------------|-------|
| 1 | [Nav item] | Y / N | [Label] | — |
| 2 | [Nav item] | Y / N | [Label] | — |

### [Secondary Nav — if applicable]

| Element | String |
|---------|--------|
| [Element] | [String] |

### User Menu / Profile Menu

| Element | String |
|---------|--------|
| Account / Profile | [Label] |
| Settings | [Label] |
| Help / Support | [Label] |
| Logout | [Label] |

### Badge / Count Format

| Context | Format |
|---------|--------|
| Unread count in nav | [e.g., "Inbox (3)" or "Inbox · 3"] |
| Cap at | [e.g., "99+" when count exceeds 99] |

---

## [Screen 1 Name]

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** For every Screen defined in UI/UX, create a section here and capture every user-visible string on that screen. The Screen name copied here must exactly match the Screen name in UI/UX.
>
> **A complete per-Screen section covers:**
> - **Topbar / Header** — title, subtitle, primary action button label
> - **Each Key Element from UI/UX** — every label, every column header, every section header, every helper text
> - **Each State from UI/UX** — Loading text (if not using global), Empty state heading + body + CTA, Error state heading + body + recovery action, Populated state strings (sort labels, filter labels, pagination labels)
> - **Form fields (if any)** — every label, placeholder, helper text
> - **Actions / buttons** — every button that appears, with the context it appears in
> - **Tooltips** — every icon button or hint that has a tooltip
>
> **The "every element" rule:** Open the UI/UX Screen block. For every entry in Key Elements, every State, every Primary Action — there must be at least one string captured here. If an element has no visible text (purely visual), explicitly note "no copy" so it's not mistaken for an oversight.
>
> **Apply conventions:**
> - Heading casing from String Conventions
> - Button label casing from String Conventions
> - Date/time formats from String Conventions
> - Empty/null displays from String Conventions
>
> **Reference globals:**
> - Don't redefine "Save", "Cancel", "Delete" here — reference the Global / Shared section. Only override if the screen needs a different label (and explain why).
>
> **Validation error messages:** Do NOT write them here. They live in the Validation Errors master list. Reference by ID if needed.
>
> **Toast messages:** Do NOT write them here. They live in the Toast Messages section.
>
> **Incomplete looks like:**
> - A UI/UX Key Element with no string captured
> - A Loading / Empty / Error / Populated state with no copy
> - A button referenced in UI/UX Primary Actions but missing here
> - Validation error copy inline (move to master list)
> - A column header with no decided casing
> - Placeholder text shown as the only label (placeholder is not a label substitute)
>
> **Ask triggers — stop and ask the human if:**
> - A screen has a complex empty state requiring guidance (e.g., onboarding context) but the body copy isn't specified
> - A column header could be ambiguous (e.g., "Date" — date created? date modified? date due?)
> - A field needs helper text but the help text isn't provided
> - A truncation point exists (e.g., user names in a list) but the truncation length isn't decided
>
> **Cross-reference checklist for each per-Screen section:**
> - Every Key Element in the matching UI/UX Screen has a string row here
> - Every State in the UI/UX Screen has copy here (or explicitly references Global fallback)
> - Every Primary Action in the UI/UX Screen has a button label here (or references Global)
> - Every form field references the validation error in the master list by scenario
>
> **Remove this block before delivering the filled doc. (Keep this section's structure for the next screen — just remove the agent block.)**

### Topbar / Header

| Element | String |
|---------|--------|
| Screen title | [Title] |
| Subtitle / context line | [e.g., count, date range, current view] |
| Primary action button | [Button label — or reference Global] |
| Secondary action button | [Label] |

### [Sub-section — name it per the screen, e.g., "Project List", "Metric Cards", "Filter Bar"]

| Element | String / Format |
|---------|----------------|
| [Section heading] | [String] |
| [Column header — field 1] | [String] |
| [Column header — field 2] | [String] |
| [Inline helper text] | [String] |

### Form Fields (if screen has a form)

| Field | Label | Placeholder | Helper text |
|-------|-------|------------|-------------|
| [field name from Schema] | [Label] | [Placeholder or —] | [Helper or —] |
| [field name] | [Label] | [Placeholder] | [Helper] |

### States

| State | Heading | Body | CTA / Action |
|-------|---------|------|-------------|
| Loading | [Or reference Global loading] | — | — |
| Empty — no data yet | [Heading] | [Body] | [Button label, or —] |
| Empty — filter/search no results | [Heading] | [Body] | [Action, e.g., "Clear filters", or —] |
| Error — fetch failed | [Heading] | [Body] | [Retry button label or —] |
| Populated — list sort labels | — | [e.g., "Sorted by Name (A → Z)"] | — |
| Populated — pagination labels | — | [e.g., "Showing 1–20 of 47"] | — |

### Actions / Buttons (beyond primary)

| Button | Context | Label |
|--------|---------|-------|
| [Button] | [Where / when it appears] | [Label or reference Global] |

### Tooltips

| Element | Tooltip text |
|---------|-------------|
| [Icon button] | [Tooltip] |

---

## [Screen 2 Name]

*(Copy block above for each Screen defined in UI/UX. Section name matches UI/UX Screen name exactly.)*

### Topbar / Header

| Element | String |
|---------|--------|
| Screen title | [Title] |

### [Sub-section]

| Element | String |
|---------|--------|
| [Element] | [String] |

### States

| State | Heading | Body | CTA |
|-------|---------|------|-----|
| [State] | [Heading] | [Body] | [CTA or —] |

---

## [Add one section per Screen — repeat the pattern above]

---

## Validation Errors — Master List

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Single cross-screen master list of every inline validation error. Every form field on every screen that can fail validation must have at least one row here.
>
> **A complete Validation Errors master list covers:**
> - Every field × every validation failure type (empty when required, too short, too long, invalid format, duplicate, out of range)
> - Field-level errors from Tech Spec error responses (the `details[].field` shape)
> - Scenarios from UI/UX Form Behavior (form-level errors after submit)
>
> **Pull from Schema:**
> - Every field with a NOT NULL constraint that's user-editable → at least one row ("required" error)
> - Every field with a CHECK constraint → at least one row per constraint failure
> - Every UNIQUE constraint → at least one row ("duplicate" error)
>
> **Pull from Tech Spec:**
> - API validation rules → drives what's enforced server-side (errors come back as 400 with `details`)
>
> **Pull from UI/UX:**
> - Error States table → every validation error pattern is "Inline field error" — copy lives here
>
> **The message rules (apply String Conventions + these):**
> - Specific to the field — not "Invalid input"
> - State the rule, not just that it's broken — "Email must be a valid address" not "Invalid email"
> - Actionable when possible — "Choose a name shorter than 50 characters" beats "Name too long"
> - Match voice/tone from String Conventions
>
> **Incomplete looks like:**
> - A required field on a screen with no "required" error row here
> - Generic messages ("Invalid input") for fields that have specific constraints
> - Schema UNIQUE constraint with no "duplicate" error row
> - "Please enter a valid X" patterns when the rule is specific enough to state directly
>
> **Ask triggers — stop and ask the human if:**
> - A field has a complex validation rule (regex, multi-field dependency) but the user-facing message isn't decided
> - Whether to show validation errors as red text only, or with an icon, or both (cross-ref UI/UX Error pattern)
>
> **Remove this block before delivering the filled doc.**

> All inline validation errors for every screen. Single master list — don't scatter validation copy across screens.

| Context (Screen / Form) | Field | Scenario | Error message |
|------------------------|-------|----------|---------------|
| [Screen or modal] | [Field] | Empty (required) | [Error copy] |
| [Screen or modal] | [Field] | Too long (over N chars) | [Error copy] |
| [Screen or modal] | [Field] | Invalid format | [Error copy] |
| [Screen or modal] | [Field] | Duplicate (UNIQUE constraint) | [Error copy] |
| [Screen or modal] | [Field] | Out of range | [Error copy] |

---

## Toast Messages

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Every toast / snackbar notification in the app. Pull from UI/UX Error States (rows with "Toast" pattern) and from confirmed success messages after writes.
>
> **A complete Toast section covers:**
> - **Success toasts** — every meaningful create / update / delete / restore action
> - **Error toasts** — every UI/UX Error States row with "Toast (error)" pattern
> - **Info toasts (if applicable)** — system messages that don't require action
> - Auto-dismiss default timing (and per-type overrides if any)
>
> **Pull from UI/UX:**
> - Error States rows with "Toast (error)" pattern → drives error toast copy
> - Form Behavior "Success action" of toast → drives success toast copy
> - Optimistic UI rollback toast triggers → drives rollback error toasts
>
> **Apply conventions:**
> - Voice from String Conventions
> - Generally short (under ~60 chars)
> - End punctuation per convention
>
> **Incomplete looks like:**
> - UI/UX Error pattern says "Toast (error)" but no row here
> - Success toast for create/update/delete actions missing
> - "Saved" without specifying what was saved (when context matters)
> - Toast longer than ~80 characters (toasts get truncated — keep them short)
>
> **Ask triggers — stop and ask the human if:**
> - Whether success toasts should be generic ("Saved") or specific ("Project saved")
> - Whether to show a toast for every action or only for non-obvious confirmations
> - Auto-dismiss duration for error toasts vs success (errors often stay longer)
>
> **Remove this block before delivering the filled doc.**

**Auto-dismiss default:** [X seconds] — [Different for errors? Persist until dismissed?]

### Success Toasts

| Action | Toast message |
|--------|---------------|
| [Entity] created | [Copy] |
| [Entity] saved / updated | [Copy] |
| [Entity] deleted | [Copy] |
| [Entity] restored | [Copy] |
| [Bulk action — e.g., "5 items archived"] | [Copy with count] |
| [Other action] | [Copy] |

### Error Toasts

| Scenario | Toast message |
|----------|---------------|
| Network error (offline) | [Copy] |
| Save failed (generic) | [Copy] |
| Action timed out | [Copy] |
| Session expired (toast before redirect) | [Copy] |
| Server error (500) | [Copy] |
| Rate limited | [Copy] |
| Optimistic rollback (action reverted) | [Copy] |
| [Specific failure from UI/UX Error States] | [Copy] |

### Info Toasts (if applicable)

| Scenario | Toast message |
|----------|---------------|
| [e.g., "New version available — refresh to update"] | [Copy] |

---

## Confirm Dialogs

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Every confirmation modal in the app — destructive actions, irreversible operations, or any action UI/UX flagged for confirmation.
>
> **A complete Confirm Dialog covers (per modal):**
> - Title — what action is being confirmed
> - Body — what will happen, including any consequences (deletion is permanent, X items affected, etc.)
> - Confirm button label — should describe the action ("Delete" not "OK")
> - Cancel button label — usually just "Cancel"
> - Optional: type-to-confirm hint if the modal uses type-to-confirm pattern
>
> **Pull from UI/UX:**
> - Modals & Drawers table → drives which dialogs exist
> - Destructive Actions table → drives which dialogs use heavier patterns (type-to-confirm)
>
> **Copy rules:**
> - Title is short and direct — "Delete project?" not "Are you sure you want to delete this project?"
> - Body explains consequence — "This permanently deletes the project and all its tasks. This cannot be undone."
> - Confirm button uses the verb, not "OK" — "Delete" or "Discard" or "Archive"
> - Use the entity name in title and body when possible
>
> **Incomplete looks like:**
> - A destructive action in UI/UX with no dialog here
> - Confirm button labeled "OK" — replace with action verb
> - Body that doesn't explain the consequence
> - Body that hides the irreversibility (always state it if applicable)
>
> **Ask triggers — stop and ask the human if:**
> - A confirm dialog should show item counts ("Delete 12 items?") and how the count is formatted with 1 vs many
> - Whether type-to-confirm copy uses the entity name or the literal word "delete"
>
> **Remove this block before delivering the filled doc.**

> Pattern for each: Title + Body + Confirm button label + Cancel button label. Optional: type-to-confirm hint.

### Delete [Entity Type]

| Element | String |
|---------|--------|
| Title | [e.g., "Delete project?"] |
| Body | [What happens — including irreversibility if applicable] |
| Confirm button | [Action verb — "Delete"] |
| Cancel button | Cancel |
| Type-to-confirm hint (if pattern is used) | [e.g., "Type the project name to confirm"] |

### [Other destructive / irreversible action]

| Element | String |
|---------|--------|
| Title | [Title] |
| Body | [Body] |
| Confirm button | [Label] |
| Cancel button | Cancel |

### Discard Unsaved Changes

| Element | String |
|---------|--------|
| Title | [e.g., "Discard changes?"] |
| Body | [e.g., "Your changes will be lost."] |
| Confirm button | [e.g., "Discard"] |
| Cancel button | [e.g., "Keep editing"] |

---

## [Optional] First Launch / Onboarding Flow

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Only fill this section if the app has a first-run sequence, signup wizard, or onboarding tour. Otherwise delete the section.
>
> **A complete Onboarding section covers each step's:**
> - Step title and subtitle
> - Body / prompt copy
> - Field labels and placeholders (if any)
> - Primary action button
> - Skip / dismiss button (if allowed)
>
> **Remove this block before delivering the filled doc, OR delete the entire section if not applicable.**

### Step 1: [Step Name]

| Element | String |
|---------|--------|
| Modal / screen title | [Title] |
| Subtitle / body | [Copy] |
| [Field] label | [Label] |
| [Field] placeholder | [Placeholder] |
| Primary button | [Label] |
| Skip button (if applicable) | Skip |

### Step 2: [Step Name]

| Element | String |
|---------|--------|
| Title | [Title] |
| Body | [Copy] |
| Primary button | [Label] |
| Back button | [Label] |
| Skip button | Skip |

---

## [Optional] Export / File Content Strings

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Only fill this section if the app generates files (CSV, PDF, etc.) with user-visible text. Otherwise delete the section.
>
> **A complete Export section covers:**
> - Default filename format
> - File header / metadata copy
> - Column headers (for CSV/XLSX exports)
> - Empty cell value
>
> **Remove this block before delivering the filled doc, OR delete the entire section if not applicable.**

| Element | String / Format |
|---------|----------------|
| Default filename | [App]_Export_[YYYY-MM-DD].[ext] |
| File header (with user) | [App] Export — [User Name] — [date range] |
| File header (without user) | [App] Export — [date range] |
| Column headers | [Col1],[Col2],[Col3]… |
| Empty cell value | (blank) |
| Generated-by footer | [e.g., "Generated by [App] on [date]"] |

---

## 🚦 Gate 2 — Full UI Strings Sign-Off

> 🤖 **AGENT INSTRUCTIONS**
>
> **Do not proceed past this gate until the human explicitly signs off. This is the final gate before the Coding Kickoff Checklist consumes this doc.**
>
> Run every check below. If any item is not ✅, stop and resolve it before asking for sign-off.
>
> **Remove this instruction block before delivering the filled doc. Keep the checklist and sign-off line.**

**Checklist:**
- [ ] String Conventions are filled and Gate 1 is signed off
- [ ] Global / Shared strings are filled and Gate 1 is signed off
- [ ] Every Screen in UI/UX has a corresponding section here with the exact screen name
- [ ] Every Key Element in every UI/UX Screen has a string row here
- [ ] Every State (Loading, Empty, Error, Populated) in every UI/UX Screen has copy (or explicit reference to Global fallback)
- [ ] Every Primary Action in every UI/UX Screen has a button label (or reference to Global)
- [ ] Every form field has Label + Placeholder + Helper (use "—" if intentionally empty)
- [ ] Validation Errors master list has at least one row per form field that can fail
- [ ] Every Schema field with NOT NULL or UNIQUE has a corresponding validation error row
- [ ] Every UI/UX Error States row with "Toast (error)" pattern has a Toast Messages row
- [ ] Every UI/UX Destructive Action has a Confirm Dialog row
- [ ] All copy follows the locked String Conventions (casing, punctuation, voice)
- [ ] No validation error copy lives in per-screen sections (all in master list)
- [ ] No toast copy lives in per-screen sections (all in Toast Messages)
- [ ] Cross-screen consistency check: same concept named same way (e.g., always "Project" not sometimes "Workspace")
- [ ] Onboarding section completed or explicitly deleted
- [ ] Export section completed or explicitly deleted

**Sign-off:**
> 🚦 **Gate 2** — UI Strings complete, internally consistent with locked conventions, and traces fully to UI/UX, Schema, and Tech Spec. Ready for Coding Kickoff Checklist.
>
> **Human sign-off:** ☐ Approved — UI Strings complete. Proceed to Coding Kickoff Checklist.
