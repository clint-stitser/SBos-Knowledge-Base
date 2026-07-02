# Discussion: Category-Scoped One-Way Cutover (Pilot: Entry-Level Housing)

> **Status:** Decided — ready for Claude Code to execute. This is a **refinement of the "bidirectional mirror" migration decision**, not a blanket replacement — see §4 for the exact boundary of when this pattern applies vs. when the full bidirectional mirror is still needed.
> **Why this exists:** Clint wants to start working Entry-Level Housing product-line projects entirely in the new Supabase app *today*, while the rest of the org keeps operating in SmartSuite as normal, without building the full bidirectional-sync engine first.
> **Execution note:** this doc is a spec for the **Claude Code session** (the single write surface for `sb-crm-poc`, per `restart.md`) to implement — this claude.ai chat documents and designs, it doesn't have network access to Supabase or SmartSuite to build this directly.

---

## 1. The Model: Category-Scoped, One-Way Cutover

Instead of a global bidirectional mirror (two-way sync, conflict resolution, last-write-wins) across the whole system, this pattern applies **per Category**:

- A Category (e.g., "Entry-Level Housing" product line) gets flagged **cutover mode: Supabase-native.**
- **New projects created under that Category live and get edited entirely in the new (Supabase) app** — never created directly in SmartSuite.
- Selected facets of those projects (see §2) push **one-way, Supabase → SmartSuite**, so the rest of the org can still see them exist and their status without leaving SmartSuite.
- Because SmartSuite is never the place those facets get *edited*, there's **no conflict to resolve** — this is why it's simpler to build than the full bidirectional mirror. The trade is that the SmartSuite copy must actually be locked against edits (§3), or the "one-way" assumption silently breaks.
- Everything **outside** a cutover-flagged Category keeps working exactly as it does today, fully in SmartSuite, untouched.

## 2. What Syncs Today vs. What's Deferred

**Syncs today (one-way, Supabase → SmartSuite):** Details · Schedule · Tasks/Checklists · Team.

**Deferred — Budget & Pay App facet:** not built for these projects yet. **This is workable specifically because these are brand-new projects with no existing SmartSuite budget/financial data** — there's nothing to reconcile or dual-enter. Budget & Pay App gets built once that parity mechanism ships (a separate, higher-risk follow-on piece — real money, retention calcs, Sr Mgt sign-off, AP approval), at which point it plugs into the same Category-cutover pattern.

**Scoping caveat — do not generalize this shortcut:** this "just don't build budget yet" simplification works *because* these projects are new. It does **not** apply to any category or project that already carries live budget/financial data in SmartSuite — those need real reconciliation (the full bidirectional mirror, or a one-time verified migration), not this shortcut. Confirm "no existing budget data" before applying this pattern to any other category.

## 3. Locking the SmartSuite Side (synced facets only)

Only the facets that are actually syncing (§2) need to be locked in SmartSuite — Budget/Pay App isn't built there yet either, so there's nothing to lock on that facet.

**Day 1 (build today, no verification needed):**
- A visible flag/banner field on the record: "🔒 Managed in New App — do not edit here."
- Pull these records out of the normal working Grid views into a separate read-only report/dashboard view.

**Fast-follow (this week, pending verification):**
- Check SmartSuite's actual Record-permission / Field-level-restriction granularity — specifically whether it supports conditional-on-field-value scoping (e.g., "if Managed-by-Supabase = true, this record is read-only for standard users"). Not confirmed either way as of this writing — verify directly in the SmartSuite instance before building around it.
- If it supports that: apply a real permission lock, scoped to the synced facets, with the sync process writing through a service API token that bypasses UI permissions.

**Safety net (add once the push mechanism is live):**
- A watchdog automation: fires on any edit to a flagged record's synced fields; if the edit didn't come from the sync process, revert it and/or notify Clint.

## 4. When This Pattern Applies vs. When the Full Bidirectional Mirror Is Still Needed

| Situation | Pattern |
|---|---|
| New Category, no existing SmartSuite data, willing to defer Budget/Pay App | **This doc** — category-scoped one-way cutover |
| Existing Category/projects with live SmartSuite data that must keep being edited from *both* sides during transition | **Original bidirectional-mirror decision** (`memory.md`, 2026-06-28/29) — two-way sync, ID pairing, last-write-wins, sync ledger, loop prevention |

Both patterns can coexist long-term — this doc doesn't retire the bidirectional-mirror spec, it adds a faster on-ramp for the specific case (new work, no legacy data) where the full mirror is more engineering than the situation needs.

## 5. Build Checklist (for Claude Code, today)

- [ ] Flag the Entry-Level Housing Category as `cutover_mode: supabase_native` (or equivalent).
- [ ] Create an **ID-pairing table** — `supabase_record_id ↔ smartsuite_record_id`, per synced entity (project, schedule item, task, team link) — required so the one-way push updates the right SmartSuite record instead of duplicating it.
- [ ] Build the one-way push (Supabase → SmartSuite API) for Details, Schedule, Tasks/Checklists, Team — create-on-first-push, update thereafter, keyed off the ID-pairing table.
- [ ] Apply the Day-1 SmartSuite lockout (banner flag + view removal) to any record that push touches.
- [ ] Update the "New Project" flow so that, for this Category, project creation happens **only** in the new app — the SmartSuite record is a byproduct of the push, never created manually.
- [ ] Leave Budget & Pay App facet unbuilt for these projects — no placeholder, no dual-entry, nothing to sync yet.
- [ ] **Which specific projects launch today:** Clint to specify directly (not decided in this session).

## 6. Open Items

- [ ] Verify SmartSuite's conditional Record-permission / Field-level-restriction capability (§3 fast-follow).
- [ ] Confirm exact SmartSuite field/table IDs for Schedule, Tasks/Checklists, and Team mapping (cross-check against the live-app research already in `memory.md`).
- [ ] Design and build Budget/Pay App parity (separate, higher-risk follow-on — not today).
- [ ] Decide whether this pattern gets offered to other new Categories going forward, or stays a one-off pilot until proven.
