# Build Decisions Log: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized (runtime maintenance doc — same pattern as Progress Checklist and Mid-Build Review)
> **Fill order:** Scaffolded at the start of coding (after Pre-Build Validation Checklist signs off and before the first commit). Once scaffolded, this doc becomes a **runtime artifact** maintained continuously during the build. Every coding-phase doc references `BD-XX` IDs from this log.
>
> **Operational mode:** Runtime maintenance doc. The agent maintains this log live during the build. `🤖` blocks stay in during the build. Cleanup happens only at Project Closeout.
>
> **Source docs (the agent reads these to write or verify an entry):**
> - `[AppName]_Module_Breakdown.md` — every entry has a Module field referencing an `M-XX` from here
> - `[AppName]_Technical_Spec.md`, `[AppName]_DB_Schema.md`, `[AppName]_UI_UX.md`, etc. — the design docs an entry may deviate from, fill a gap in, or flag a concern against
> - Every coding-phase doc — entries reference specific artifact IDs (DB-XX, C-XX, S-XX, AC-XX, etc.); the agent verifies the IDs exist before writing them
>
> **Downstream docs that consume this one (write to feed them):**
> - **Every coding-phase doc** — `BD-XX` IDs appear in code comments, in Progress Checklist Notes columns, in Mid-Build Review Findings, in Phase Closeout BD-entry roll-ups. **Precision in this log compounds across every downstream artifact.**
> - `[AppName]_Phase_Closeout_[N].md` — Section 4 of every Phase Closeout lists `BD-XX` IDs added in that phase, OR "none, justification: [reason]"
> - `[AppName]_Project_Closeout.md` — harvests entries by type; Reconciliations feed the back-feed loop into template updates; unresolved Concerns become V2 backlog candidates
> - `[AppName]_Discussion_[topic-slug].md` (when a Concern is promoted) — BD entry and Discussion file link to each other bidirectionally
> - `[AppName]_Mid_Build_Review_[N].md` — Findings rows may reference `BD-XX` IDs when a drift item is captured as a Build Decision rather than a doc-edit
>
> **Agent role:** Honest, precise entry maintenance. The agent owns the live state of this log. When a build event occurs (workaround / reconciliation / concern / deviation / gap / dependency / scope creep), the agent writes the entry BEFORE the related code is committed (per Mid-Build Review's commit-time logging rule, the commit body references the BD-XX). The agent does NOT invent entry types, does NOT downgrade a Concern to a Deviation to make the build look cleaner, and does NOT close an entry without an explicit resolution.
>
> **The three rules while maintaining this log:**
> 1. **Log on occurrence, not on schedule.** A workaround written in code without a BD entry is undocumented debt. The locked rule is: BD entry first, then commit. Mid-Build Review's commit-time logging rule has the audit trail in `git log` to catch lapses.
> 2. **The type isn't decoration.** If you can't decide whether an entry is a Workaround, a Deviation, or a Concern, you haven't decided what kind of event occurred. The type drives the Detail Block format and the closure rule. Counter-examples in the typology below are mandatory reading.
> 3. **"Revisit if" is not optional.** Every Workaround, Deviation, Gap, and Scope creep entry has a concrete trigger condition. "Someday," "eventually," or a blank field is a fatal incomplete. If the entry is genuinely permanent, write "Never — [reason]" explicitly. Without a trigger, debt becomes invisible at Project Closeout.
>
> **Two failure modes drive most of the design here:**
> - **Silent resolution.** The build agent encounters a deviation during a coding session, fixes it on the fly, and moves on without logging. By the time anyone notices, the rationale is lost. Mitigated by the commit-time logging rule, by Mid-Build Review's drift checks against built code, and by Phase Closeout's BD-entry roll-up requiring negative confirmation ("none this phase, justification: ...").
> - **Type drift.** An entry is filed as a Deviation when it's actually a Concern (agent didn't have authority but built it anyway), or as a Workaround when it's actually a Gap (no environmental obstacle, just an ambiguous doc). The wrong type produces the wrong closure rule and the wrong follow-up. Counter-examples in the typology are the primary defense; the agent stops and asks if the type is unclear.
>
> **When this doc is fully filled and the project is closed out (NOT mid-build):** Remove every `🤖 AGENT INSTRUCTIONS` block and the agent-facing instruction prose. Keep every entry, every resolution, every audit-trail field. The finished log reads as the project's build-decision record for humans.
>
> **Cleanup verification (only at Project Closeout — NOT before):**
> - Search the file for `🤖` — zero hits
> - Search the file for "Remove this block" — zero hits
> - Every entry has a Status of either ✅ Resolved or 🟣 Accepted (no 🟠 Open at Project Closeout — open entries are reviewed and either resolved or accepted with rationale)
>
> **Internal fill order:**
> 1. Overview — scaffolded at start of coding
> 2. This Log Is Also the Tech Debt Log — read once at scaffold, then permanent reference
> 3. Source Docs / References — permanent reference
> 4. Entry Type Typology — read once at scaffold, then permanent reference
> 5. **[ Build happens — agent adds entries per the runtime rules ]**
> 6. Resolved Entries — entries move here as they close
> 7. Phase Closeout / Project Closeout Reporting — read at every phase boundary and at project end

**Purpose:** A single audit trail of every non-obvious decision, deviation, workaround, or concern produced during the build phase. If something happened during the build that isn't fully explained by the design docs, it lives here.

**Rule:** Log it when it happens, not later. Later means never. Every phase ends with an explicit list of new entries — or "none, justification: ..." That negative confirmation is required, not optional.

**What this is NOT:** This is not a backlog of future features. This is not a bug tracker. This is the record of *why the code looks the way it does* when the design docs alone can't explain it.

**Not to be confused with:** `[AppName]_Decisions_Log.md` (Architecture Decision Log). That log captures **design-phase architecture decisions** (made before coding starts, with rationale and rejected alternatives). This log captures **build-phase events** — what happened during the build that the design docs alone can't explain. If you're not sure which one applies: are you designing or building? Designing → Decisions Log. Building → here.

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Entry Registry | 🔲 Not Started | — | Populate as entries accumulate |
| Entry Detail Blocks | 🔲 Not Started | — | One block per entry |
| Resolved Entries | 🔲 Not Started | — | Move resolved entries here |

**Section status:** 🔲 Not Started / 🔄 In Progress / 👀 In Review / ✅ Done / 🚫 Blocked

---

## Overview

- **App:** [App Name]
- **Source docs:**
  - Module Breakdown: `[AppName]_Module_Breakdown.md`
  - Tech Spec: `[AppName]_Technical_Spec.md`
  - Design docs: [list]

---

## This Log Is Also the Tech Debt Log

> **There is no separate tech debt log.** This file serves both purposes. Any intentional shortcut, deferred cleanup, or known suboptimal choice made during build must be logged here — not in code comments, not in a sticky note, not in your head.
>
> **What counts as tech debt:**
> - A shortcut taken to ship faster (with intent to revisit)
> - A workaround that needs to be undone when [condition] is resolved
> - A "good enough for now" choice with a known better alternative
> - Any code that takes a shortcut without a corresponding BD entry is **undocumented debt** — the worst kind. The whole point of this log is that nothing gets forgotten.
>
> **How to log tech debt here:**
> - **🛠 Workaround** — for environmental obstacles or temporary fixes. Use the **Revisit if** field to set the unwind condition.
> - **↪️ Deviation** — for "we deliberately did it the simpler way, knowing the better way exists." Use **Revisit if** for the upgrade condition.
> - **🕳 Gap** — for "design didn't specify, we picked the defensive default, the design doc should be updated."
> - **📈 Scope creep** — for "we cut this corner because the right answer was out of scope."
>
> **Fix-it tracking:** Every entry that represents debt must have a concrete **Revisit if** condition — a trigger that says when this gets reopened. "Someday" is not a trigger. "When v2 starts" or "when traffic exceeds X" or "when WiX v7 validator becomes configurable" are triggers. Without a trigger, debt becomes permanent.
>
> **At Project Closeout:** all open entries are reviewed. Anything still 🟠 Open with no Revisit trigger becomes a V2 backlog candidate or is explicitly accepted as 🟣 Won't Fix with rationale. Nothing rots silently.

---

## Source Docs / References

> The Build Decisions Log integrates with several other templates. If an entry below references something unfamiliar, this is where to find the source.

| Concept used here | Source doc | Why it's referenced |
|-------------------|-----------|---------------------|
| Module IDs (`M-XX`) | `[AppName]_Module_Breakdown.md`, Module Registry | Every entry is tied to the module where the decision/deviation/concern occurred |
| Three-category acceptance criteria (🧪 Functional / 🌍 Environmental / ✅ Verification) | `[AppName]_Module_Breakdown.md`, Acceptance Criteria pattern | Scope creep entries reference module scope; Deviation entries may impact criteria |
| Discussion file topics | `[AppName]_Discussion_[topic-slug].md` | Concern entries may be promoted to a Discussion file at Ryan's discretion. Promotion is manual, not automatic. When promoted, the BD entry links to the Discussion file and the Discussion file links back. |
| Phase Closeout (where entries are reported per phase) | `[AppName]_Phase_Closeout_N.md` | Section 4 of every Phase Closeout lists BD-XX IDs added during that phase |
| Project Closeout (where entries are harvested at project end) | `[AppName]_Project_Closeout.md` | Reconciliations feed the back-feed loop; Concerns become V2 backlog candidates |
| Cross-Doc Validation Sign-Off Scope | `Cross_Doc_Validation_Checklist_Template.md`, Sign-Off Scope section | Reconciliation entries imply doc-edit follow-up; sign-off scope rule governs partial re-validation |

---

## Entry Type Typology

Every entry has a **type**. The type isn't decoration — it picks the detail block format and tells the reader what kind of decision they're looking at. If you're not sure what type an entry is, you probably haven't decided what *kind* of decision it was. Stop and figure that out first.

Each type below has a **bounded definition**, an **example**, and a **counter-example**. The counter-example matters: it tells you what *not* to file under this type. Without counter-examples, every type becomes a junk drawer.

### 🛠 Workaround

**Definition:** A build-time obstacle was resolved by deviating from the intended tooling, library, or environment. The deviation was forced by the environment, not chosen by preference.

**Example:** *"WiX v7 ICE validation rejected our RTF EULA. Fell back to WiX v6 to unblock the installer build. v7 retained as a future option if its validator becomes configurable."*

**Counter-example:** Switching libraries because you preferred one — that's a **Deviation** (you had authority and chose). Workaround is when the environment forced your hand.

### ⚖️ Reconciliation

**Definition:** Two design docs gave overlapping or conflicting guidance at different granularities. A single interpretation was chosen mid-build, and the choice should be recorded so the reader knows which doc was treated as authoritative.

**Example:** *"Tech Spec described ActivityService behavior at the contract level; Module Breakdown listed method signatures. Where they diverged on the GetSuggestions signature, Module Breakdown was treated as authoritative. Tech Spec needs an update in next Cross-Doc Validation pass."*

**Counter-example:** If the docs were genuinely contradictory and you couldn't resolve either way, that's a **Gap**. Reconciliation requires that you *picked one and moved on*.

### ⚠️ Concern

**Definition:** A design decision was signed off, but the build agent now believes it may be wrong. The agent is building as specified — this entry is the flag, not a renegotiation.

**Example:** *"Settings UI spec calls for a modal dialog. The rest of the app is keyboard-first; modal will trap users in a way that conflicts with the navigation pattern. Building as specified. Flagging for review at end-of-phase."*

**Counter-example:** If you have authority to change the design and you did, that's a **Deviation**. Concern is when you *did not* change it but want it on the record.

> **Concern → Discussion file (manual promotion):** A Concern entry does *not* auto-create a Discussion file. Ryan reviews open Concerns at end-of-phase or at his discretion, and promotes individual Concerns to a Discussion file when three-voice working-out is warranted. Most Concerns resolve without ever needing a Discussion file. When a Concern *is* promoted, the BD entry links to the Discussion file (`[AppName]_Discussion_[topic-slug].md`) and the Discussion file links back. Default outcome regardless of promotion: build-as-specified.

### ↪️ Deviation

**Definition:** The build deliberately diverged from a signed-off design doc, by the agent's authority, with rationale. Includes the case where the design doc was silent on the point and the agent chose between reasonable alternatives.

**Example:** *"Tech Spec specified `Dictionary<string, Activity>` for the cache. Switched to `ConcurrentDictionary` after discovering parallel access in M-18 unit tests. Performance impact negligible; thread-safety required."*

**Counter-example:** If the docs explicitly *forbade* the alternative, you don't have authority — that's a **Concern** (build as specified and flag) or a stop-and-ask. Deviation requires that you had legitimate authority to make the choice.

### 🕳 Gap

**Definition:** A design doc was silent, ambiguous, or contradictory on a detail the build needed. The build had to invent an answer to move forward.

**Example:** *"DB Schema doesn't specify cascade behavior on Activity → Project deletion. Chose `OnDelete(Restrict)` to prevent accidental data loss. Flagged for design follow-up — schema doc should make this explicit before next migration."*

**Counter-example:** If the answer was implicit but obviously correct, no entry needed. Gaps require an actual decision the docs didn't authorize.

### 🔗 Dependency

**Definition:** A build-time dependency on something outside the agent's control that affected what shipped — typically a human action, an external service, or a tool that needed to be installed or configured.

**Example:** *"Installer build requires WiX v6 CLI installed globally (`dotnet tool install --global wix --version 6.0.0`). Ryan must complete this before M-24 verification. Documented in Deployment Config."*

**Counter-example:** Routine NuGet packages don't need entries. Dependency is for things that the agent *can't resolve alone* and that a future reader needs to know about.

### 📈 Scope creep

**Definition:** Work was done that exceeded the approved module scope, even when the work was justified. Recording it makes the expansion visible so module estimates can improve next time.

**Example:** *"M-14 was scoped as 'ActivityService methods.' Added input validation layer because integration tests required it. Added Serilog calls because validation behavior was non-obvious without them. Both were necessary; both expanded the module beyond its written scope."*

**Counter-example:** If the work was already implied by the design docs but not the module spec, that's normal layering, not creep. Scope creep is when the *agreed scope* expanded.

---

## Entry Registry

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Maintain the canonical row-per-entry inventory. Every entry gets a Registry row BEFORE its Detail Block is written. The Registry is what every coding-phase doc grep-references.
>
> **ID continuity rule (hard):**
> - IDs are strictly increasing: BD-01, BD-02, BD-03... no gaps, no reuse.
> - If an entry is deleted (rare — should not happen mid-build), the ID is retired. The next new entry uses the next number; the deleted ID is never reissued.
> - If an entry is resolved or accepted, the row stays in the Registry with updated Status. Resolved/accepted entries do NOT get a new ID.
>
> **Required fields for a complete Registry row:**
> - **ID:** BD-XX, sequential
> - **Type:** one of the 7 types from the typology above (🛠 Workaround / ⚖️ Reconciliation / ⚠️ Concern / ↪️ Deviation / 🕳 Gap / 🔗 Dependency / 📈 Scope creep)
> - **Title:** short — a person reading the row knows what the entry is about
> - **Module:** M-XX from Module Breakdown (verify the M-XX exists before writing it)
> - **Date Added:** YYYY-MM-DD
> - **Owner:** who logged it (Ryan / Claude Code / Claude Chat)
> - **Status:** 🟠 Open / ✅ Resolved / 🟣 Accepted
>
> **Incomplete looks like:**
> - Type left blank or set to a value not in the typology
> - Module set to a non-existent M-XX
> - Title vague ("misc issue", "thing about routing") — a person reading the Registry can't tell what the entry covers
> - Status not updated after the Detail Block's resolution is captured
>
> **Cross-reference checklist (verify before writing the row):**
> - The M-XX referenced exists in `[AppName]_Module_Breakdown.md` Module Registry
> - The type matches the Detail Block format that will follow (Workaround Detail Block for 🛠, etc.)
> - If this entry is referenced by code comments (`// BD-XX: ...`), the comments and this row agree on what the entry is about
>
> **Ask triggers:**
> - The build event doesn't cleanly match any of the 7 types — ask Ryan before inventing a new type. The typology is closed by design.
> - The entry would reference two or more Modules — ask whether to split into separate entries, or whether the entry is really about a cross-module concern that needs a different format.
>
> Keep this block in during the build. Remove at Project Closeout only.

> One row per entry. Use Entry ID (e.g., `BD-01`) to reference items in code comments and detail blocks below.
> In code: add a comment `// BD-01: [brief description]` wherever the entry is relevant.

| ID | Type | Title | Module | Date Added | Owner | Status |
|----|------|-------|--------|------------|-------|--------|
| BD-01 | 🛠 Workaround | [Short title] | [M-XX] | [YYYY-MM-DD] | [—] | 🟠 Open |
| BD-02 | ⚖️ Reconciliation | [Short title] | [M-XX] | [YYYY-MM-DD] | [—] | 🟠 Open |
| BD-03 | ⚠️ Concern | [Short title] | [M-XX] | [YYYY-MM-DD] | [—] | 🟠 Open |

**Entry status:** 🟠 Open / ✅ Resolved / 🟣 Accepted (won't fix)

---

## Entry Detail Blocks

> 🤖 **AGENT INSTRUCTIONS**
>
> **Your job:** Produce one Detail Block per Registry row. The Detail Block format depends on the entry's type — the seven templates below are not interchangeable. A coding agent reading a single Detail Block must be able to understand the event, the choice, the rationale, and the unwind condition without reading any other doc.
>
> **Format selection rule:** Match the Detail Block format to the entry's type:
> - 🛠 Workaround → Workaround template (BD-01 reference below)
> - ⚖️ Reconciliation → Reconciliation template (BD-02)
> - ⚠️ Concern → Concern template (BD-03)
> - ↪️ Deviation → Deviation template (BD-04)
> - 🕳 Gap → Gap template (BD-05)
> - 🔗 Dependency → Dependency template (BD-06)
> - 📈 Scope creep → Scope creep template (BD-07)
>
> **Required-field rules (a Detail Block is incomplete without these):**
> - **Every field labeled in the template must be filled.** Blank or `—` for an unfilled required field is a fatal incomplete. "TBD" is acceptable only if accompanied by a specific ask trigger (e.g., "TBD — waiting on Ryan to confirm rollback policy").
> - **"Revisit if" is mandatory and concrete** for every Workaround, Deviation, Gap, and Scope creep entry. Acceptable values: a specific technical trigger ("when WiX v7 validator becomes configurable"), a specific event ("when traffic exceeds 10K req/min"), or the explicit string "Never — [one-sentence reason]". "Someday," "eventually," or blank is a fatal incomplete.
> - **The Detail Block's title and the Registry row's title must match.** No drift between them.
> - **The Status field in the Detail Block must match the Status field in the Registry row.** If the entry moves to ✅ Resolved or 🟣 Accepted, both places update.
>
> **Incomplete looks like (any of these = fatal):**
> - A Workaround entry with no "What forced the workaround" field — the entry can't be evaluated for unwind
> - A Concern entry that doesn't confirm the build follows the spec ("What I built (as specified)" is blank) — the Concern format requires this confirmation by definition
> - A Deviation entry without an "Alternatives considered" table — a Deviation without alternatives is just a Workaround mislabeled
> - A Gap entry with no "Doc update required" field — Gaps exist specifically to drive doc updates; without that field the entry can't be harvested at Project Closeout
> - A Scope creep entry whose "Original module scope" field doesn't quote the Module Breakdown — the comparison can't be made without the original
>
> **Resolution rule (when an entry closes):**
> - When an entry resolves, fill the Resolution Detail at the end of the Detail Block (or, for simple resolutions, just update Status + add a one-line Resolution note).
> - Move the entry to the Resolved Entries table at the bottom of this doc.
> - DO NOT delete the Detail Block from this section — the audit trail matters. Just mark it Status: ✅ Resolved or 🟣 Accepted and let it sit.
>
> **Cross-reference checklist (verify before declaring a Detail Block done):**
> - The entry's Module field references an M-XX that exists in Module Breakdown
> - Any DB-XX, C-XX, S-XX, AC-XX, etc. referenced in the Detail Block exists in its source doc
> - If this entry is a Concern that has been promoted to a Discussion file, the Discussion file path is filled in the Detail Block, and the Discussion file links back
> - If this entry is a Reconciliation, the "Feeds back-feed loop" field is set (Yes/No)
>
> **Ask triggers:**
> - The Detail Block doesn't fit any of the 7 type templates — ask Ryan; do not invent a new template format
> - A Concern entry feels like it should be a Deviation instead — ask Ryan to confirm authority before changing the type
> - A Gap entry's "What I chose" field would be larger than a paragraph — ask whether this is really a Gap or whether it should be promoted to a Discussion file
>
> Keep this block in during the build. Remove at Project Closeout only.

> One block per entry. Copy the appropriate template below for each new entry — the format depends on the type.

---

### BD-01: 🛠 Workaround — [Short Title]

- **Type:** 🛠 Workaround
- **Module:** [M-XX — module name]
- **File(s) affected:** [`path/to/file.ext`]
- **Date Added:** [YYYY-MM-DD]
- **Added By:** [Ryan / Claude / etc.]
- **Status:** 🟠 Open

**What forced the workaround:**
[The environmental obstacle. Specific. "WiX v7 EULA validation rejected our RTF" not "tooling problem."]

**What was done instead:**
[The actual workaround. "Used WiX v6 with custom MSBuild target invoking `wix build` directly."]

**Why this is acceptable:**
[Why the workaround is not just a hack. "v6 produces the same MSI semantics; only the validator changed."]

**Risk if unresolved:**
[What happens if this stays in place forever? Often "none — build tooling only" is the right answer for a Workaround.]

**Revisit if:**
[Condition for unwinding the workaround. "If WiX v7 validator becomes configurable." "Never — v6 is fine indefinitely." **Required field — "someday" is not acceptable.**]

---

### BD-02: ⚖️ Reconciliation — [Short Title]

- **Type:** ⚖️ Reconciliation
- **Module:** [M-XX]
- **Docs reconciled:** [`Tech_Spec.md` § X vs. `Module_Breakdown.md` § Y]
- **Date Added:** [YYYY-MM-DD]
- **Added By:** [—]
- **Status:** 🟠 Open

**The conflict / granularity gap:**
[What the two docs said and where they diverged. Specific quotes or section refs.]

**Interpretation chosen:**
[Which doc was treated as authoritative on this point, and what implementation followed.]

**Why this interpretation:**
[Reasoning. "Module Breakdown is more recent and was reviewed in Cross-Doc Validation pass 4."]

**Doc updates needed:**
[Which doc(s) need to be tightened in next Cross-Doc Validation pass to prevent the same conflict next time.]

**Feeds back-feed loop:**
[Yes / No. If yes, this entry will be harvested at Project Closeout to suggest a Cross-Doc Validation Checklist update.]

---

### BD-03: ⚠️ Concern — [Short Title]

- **Type:** ⚠️ Concern
- **Module:** [M-XX]
- **Design doc affected:** [`Doc_Name.md` § X]
- **Date Added:** [YYYY-MM-DD]
- **Added By:** [—]
- **Status:** 🟠 Open
- **Discussion file:** [— if not promoted, or `[AppName]_Discussion_[topic-slug].md` if Ryan promoted this Concern to a Discussion file]

**What was specified:**
[The signed-off design decision. Quote the relevant section.]

**Why I think it may be wrong:**
[Honest reasoning. Specific failure modes anticipated.]

**What I built (as specified):**
[Confirm the build follows spec. The Concern is a flag, not a workaround.]

**What an alternative would look like:**
[The change that would address the concern, if Ryan decides to act on it.]

**Default action:** Build as specified. Flagged for Ryan's review at end-of-phase. Ryan may resolve directly, accept the Concern, or promote it to a Discussion file.

---

### BD-04: ↪️ Deviation — [Short Title]

- **Type:** ↪️ Deviation
- **Module:** [M-XX]
- **Design doc deviated from:** [`Doc_Name.md` § X, or "none — no doc spoke to this"]
- **Date Added:** [YYYY-MM-DD]
- **Added By:** [—]
- **Status:** 🟠 Open

**What the doc specified (or what was silent):**
[The original guidance, or explicit note that the docs were silent.]

**What I built instead:**
[The deviation. Specific.]

**Why I had authority to deviate:**
[Per Instructions, the agent has authority to make implementation choices when the docs don't specify. State the basis.]

**Alternatives considered:**

| Option | Why rejected |
|--------|-------------|
| [Option A] | [—] |
| [Option B] | [—] |

**Trade-offs accepted:**
[Every real choice has a cost. Name it.]

**Revisit if:**
[Condition for reconsidering, or "Never — fundamental to the implementation." **Required field — if this entry represents debt (chose the simpler way knowing the better way exists), specify the upgrade trigger.**]

---

### BD-05: 🕳 Gap — [Short Title]

- **Type:** 🕳 Gap
- **Module:** [M-XX]
- **Doc with the gap:** [`Doc_Name.md` § X]
- **Date Added:** [YYYY-MM-DD]
- **Added By:** [—]
- **Status:** 🟠 Open

**What was missing or ambiguous:**
[The specific gap. Not "schema was unclear" but "schema doesn't specify cascade behavior on Activity → Project deletion."]

**What the build needed:**
[Why the gap blocked progress. What decision the build had to make.]

**What I chose:**
[The answer the build invented. Specific.]

**Why this answer:**
[Reasoning. Often a defensive default that errs on the safe side.]

**Doc update required:**
[The design doc must be updated to make this explicit. List the exact section that needs the addition.]

---

### BD-06: 🔗 Dependency — [Short Title]

- **Type:** 🔗 Dependency
- **Module:** [M-XX]
- **External item:** [Tool / service / human action]
- **Date Added:** [YYYY-MM-DD]
- **Added By:** [—]
- **Status:** 🟠 Open

**What the dependency is:**
[The specific external thing the build relies on.]

**Who or what owns it:**
[Ryan / a service team / a vendor / etc.]

**Action required:**
[What needs to happen, by whom, by when.]

**What's blocked until then:**
[The downstream work that can't proceed without this.]

**Documented in:**
[Deployment Config? Coding Kickoff Checklist? Wherever the install/setup instruction lives.]

---

### BD-07: 📈 Scope creep — [Short Title]

- **Type:** 📈 Scope creep
- **Module:** [M-XX]
- **Original module scope:** [Quote from Module Breakdown]
- **Date Added:** [YYYY-MM-DD]
- **Added By:** [—]
- **Status:** 🟠 Open

**What was added beyond scope:**
[The extra work. Specific files / methods / features.]

**Why it was necessary:**
[The forcing function. "Tests required validation; behavior was non-obvious without logging."]

**Why it wasn't caught at module-design time:**
[Honest answer about what the Module Breakdown missed. Feeds into next-project module estimation.]

**Estimation impact:**
[Time spent on the unscoped work, if measurable. Helps calibrate future module sizing.]

---

## Resolved Entries

> Move entries here when resolved or accepted. Keep them — the resolution history is part of the audit trail.

| ID | Type | Title | Module | Resolved Date | Resolution |
|----|------|-------|--------|---------------|-----------|
| [BD-XX] | [Type] | [Title] | [M-XX] | [YYYY-MM-DD] | ✅ Resolved / 🟣 Accepted — [Brief description] |

---

## Phase Closeout Reporting

At the end of every phase, the Phase Closeout doc must include this section:

> **Build Decisions Log entries this phase:**
> - [List of BD-XX IDs added this phase, with type and one-line summary]
> - OR: "None this phase. Justification: [explicit reason]"

The negative confirmation ("none, because...") is required. Silence is not an acceptable answer.

---

## Project Closeout Reporting

At Project Closeout, this log gets harvested:

- **Entries by type, with counts** (e.g., "5 Workarounds, 2 Reconciliations, 1 Concern unresolved")
- **Concerns raised vs. resolved** — unresolved Concerns become V2 backlog candidates
- **Reconciliations** — input to the back-feed loop; produces Cross-Doc Validation Checklist updates for the next project
- **Tech debt review** — every 🟠 Open Workaround, Deviation, Gap, and Scope creep entry is reviewed against its **Revisit if** condition. Entries whose trigger has fired become V2 work. Entries with no trigger or "Never" become 🟣 Accepted.
- **Most consequential entries called out** — narrative, brief, specific

This is what makes the log self-improving across projects: the next project's templates start tighter because this project's entries got harvested.
