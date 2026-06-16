# Project Closeout: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized (fixed-shape end-of-project report; harvests across the Phase Closeout chain)
> **Fill order:** Written at the end of the final phase, AFTER that phase's Phase Closeout is signed off. One file per project.
>
> **Filename convention:** `[AppName]_Project_Closeout.md`. Lives in the project's folder, alongside the Phase Closeouts.
>
> **Source docs (read every one before writing this Closeout):**
> - **All `[AppName]_Phase_Closeout_[N].md` files** — the full chain. Sections 2 walks them; Sections 3, 4, 5 harvest content from each.
> - `[AppName]_Build_Decisions_Log.md` — the master entry list. Sections 3, 4, 5 categorize and roll up entries by type and status. At Project Closeout every entry must be ✅ Resolved or 🟣 Accepted; 🟠 Open entries are reviewed here and given disposition (resolved, accepted, or marked → V2).
> - **All `[AppName]_Mid_Build_Review_[N].md` files** — every instance closed during the project. Findings that were deferred across instances eventually resolve here or escalate to a BD entry.
> - **All `[AppName]_Discussion_[topic-slug].md` files** (if any) — their resolutions feed Sections 4 and 6.
> - All design docs — to evaluate which template improvements (Section 6) should be proposed back to the canonical templates.
>
> **Downstream docs that consume this one (write to feed them):**
> - `[AppName]_Template_Update_Worklog.md` — **Section 6 generates a real worklog file on disk** using the Template_Update_Worklog_Template structure. This is the most important downstream artifact — the back-feed loop's prescription.
> - **Ryan** — reads the entire Closeout under decision pressure: approve, or send back. Section 8's outstanding-question pattern forces the wait.
> - **The next project** — reads NOT this doc but the updated canonical templates that result from Ryan working through the worklog. The Closeout is the diagnosis; the canonical templates are the cure that the next project consumes.
>
> **Agent role:** Synthesizer and worklog generator. The agent reads across the Phase Closeout chain, BD Log, Mid-Build Reviews, and design docs; rolls up the project's actual decision and learning history; and proposes specific template deltas to the canonical templates. The agent does NOT invent learnings, does NOT skip the worklog generation step in Section 6, and does NOT close the project with statements (Section 8 ends in a question, always).
>
> **The three rules while filling this doc:**
> 1. **Every section is filled — blank is not an answer.** A section with no content gets either real content or an explicit `None — justification: [reason]`. The negative-confirmation rule is even stronger here than in Phase Closeout: zero-Concerns, zero-Reconciliations, or zero-deltas on a real project is a signal of under-surfacing, not a victory. Surface and ask.
> 2. **The last line is a question to Ryan, not a statement.** Same structural rule as Phase Closeout. Sign-off here closes the project AND hands the worklog to Ryan for template adjustment.
> 3. **Section 6 has a side effect: it writes a real worklog file to disk.** The worklog filename, location, creation date, and item count go in Section 6's table — that's the audit trail. The Closeout and the worklog point to each other bidirectionally.
>
> **Two failure modes this doc is designed to prevent:**
> - **Under-surfacing the retro.** A clean-looking project that produces zero deltas, zero open Concerns, and zero Reconciliations is almost certainly a project where the retro didn't dig hard enough. Each of those zero-states is explicitly flagged in its section with a "reflect on whether this is plausible" prompt.
> - **Worklog amnesia.** A Project Closeout signs off without actually generating the worklog file, or with a worklog file that's missing items from Section 6's source list. Section 6's two-table structure (Source List + Worklog Generated metadata) makes the gap visible: if Source List has 5 items and Worklog item count says 3, the gap is on the page.
>
> **When this Closeout is fully filled and approved:** Remove every `🤖 AGENT INSTRUCTIONS` block. Keep all filled content, every roll-up table, the worklog reference, Ryan's approval, and the closing question. The finished Closeout reads as the project's permanent end-of-project record.
>
> **Cleanup verification (before declaring the Closeout signed off):**
> - Search the file for `🤖` — zero hits
> - Search the file for "Remove this block" — zero hits
> - Every section has content or an explicit `None — justification: ...` line
> - The last line of the doc is a question, not a statement
> - The worklog file referenced in Section 6 actually exists on disk at the named path

**Purpose:** A fixed-shape report generated at the end of a project. Synthesizes across all Phase Closeouts, harvests learnings, and produces an actionable Template Update Worklog so the *next* project starts from improved templates instead of repeating the same mistakes.

**When to write:** When the final phase has been signed off and the app has shipped (or the project has been formally stopped). One file per project.

**Pair with:** All `[AppName]_Phase_Closeout_N.md` files (the chain), `[AppName]_Build_Decisions_Log.md`, and the Discussion file (if present). This doc rolls up across them.

**Critical output:** Section 6 of this doc generates a real `[AppName]_Template_Update_Worklog.md` file using the Template_Update_Worklog_Template. The closeout is the diagnosis; the worklog is the prescription. Don't conflate them — closeout is a snapshot of *why* the deltas exist, worklog is *what gets done about them*.

**Where the back-feed loop ends:** The loop terminates at the worklog. Ryan works through the worklog (with Claude's help, in a separate session) and adjusts the canonical templates in the Generic templates folder. The *next project* starts from those clean, updated templates — it never reads the worklog. The worklog is a working document for the template-adjustment phase, not a handoff artifact for the next project.

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| 1. Project Summary | 🔲 Not Started | — | Narrative, 1–2 paragraphs |
| 2. Phase Closeout Chain | 🔲 Not Started | — | Links to all Phase_Closeout_N.md files |
| 3. Build Decisions Log Roll-Up | 🔲 Not Started | — | Entries by type with counts |
| 4. Concerns Raised vs. Resolved | 🔲 Not Started | — | V2 backlog pipeline (in-table) |
| 5. Reconciliations Harvested | 🔲 Not Started | — | Back-feed loop input |
| 6. Template Deltas Proposed | 🔲 Not Started | — | Generates Template_Update_Worklog.md |
| 7a. What Worked / What Didn't — Claude Code | 🔲 Not Started | — | Build agent surface |
| 7b. What Worked / What Didn't — Claude Chat | 🔲 Not Started | — | Design/planning agent surface |
| 8. Outstanding Question | 🔲 Not Started | — | Required: last line is a question to Ryan |

**Section status:** 🔲 Not Started / 🔄 In Progress / 👀 In Review / ✅ Signed Off / 🚫 Blocked

---

## Source Docs / References

> The Project Closeout pulls vocabulary, structure, and content from many places. If a section below references something unfamiliar, this is where to find the source.

| Concept used here | Source doc | Why it's referenced |
|-------------------|-----------|---------------------|
| Phase Closeout chain (`[AppName]_Phase_Closeout_N.md`) | `Phase_Closeout_Template.md` | Section 2 walks the chain; sections 3–5 harvest content from each one |
| `BD-XX` entry IDs, type icons (🛠 ⚖️ ⚠️ ↪️ 🕳 🔗 📈), status icons (🟠 ✅ 🟣) | `[AppName]_Build_Decisions_Log.md` | Sections 3, 4, 5 categorize and roll up entries by type and status |
| Sign-off scope rule (material vs. cosmetic edits invalidate validation) | `Cross_Doc_Validation_Checklist_Template.md`, Sign-Off Scope section | Section 5 (Reconciliations) feeds directly into this rule |
| Back-Feed Loop (project closeout → template updates → next project starts clean) | `Cross_Doc_Validation_Checklist_Template.md`, Back-Feed Loop reference | Section 6 is the loop's output mechanism |
| Template Update Worklog format (Categories A/B/C/D, status flow, scratch pad) | `Template_Update_Worklog_Template.md` | Section 6 generates a filled instance of this template |
| Three-voice discussion threads (Code / Chat / Ryan), Current Turn marker | `[AppName]_Discussion_File.md` (when present) | Sections 7a / 7b reference Discussion topics for context |

---

## How to Use This Doc

This template is enforced, not advisory:

1. **Every section has required content or an explicit `None — justification: [reason]`.** Blank is not an answer.
2. **The last line of this doc is a question to Ryan, never a statement.** Same rule as Phase Closeout — it's what stops the agent from quietly considering the project "closed" without explicit sign-off.
3. **Section 6 has a side effect: it writes a real worklog file.** Don't skip the worklog generation step. The worklog filename, location, and creation date go in Section 6's table — that's the audit trail.
4. **Sign-off is binary.** Either Ryan replies "approved" and the project is closed (which hands the worklog to Ryan for template-adjustment work), or Ryan replies with corrections and the doc is revised before re-asking.
5. **Don't start the next project until the worklog is closed and the templates are updated.** A Project Closeout with an unworked worklog means the next project starts from stale templates and will re-surface the same problems. The next project reads only the updated canonical templates — never the worklog itself.

---

## 1. Project Summary

> 🤖 **AGENT INSTRUCTIONS — Section 1**
>
> **Your job:** 1-2 paragraphs capturing what the project shipped AND what the project taught. Both layers are required. For pipeline-test projects, the process outcome (what the system taught about the templates) is often the more important deliverable.
>
> **A complete summary covers:**
> - The functional outcome (did the app ship; what does it do; what's the user-visible state)
> - The process outcome (what worked in the pipeline; what surfaced in retro; total BD-XX count and rough mix; whether the build phase ran autonomous or required human re-architecture)
> - The headline number for the worklog (how many template-improvement candidates surfaced, how many made it into the worklog)
>
> **Incomplete looks like:**
> - One layer only (functional but not process, or vice versa)
> - A bullet list — this section is prose
> - Vague qualifiers without numbers ("went mostly well" — quantify what went well)
>
> Remove this block before delivering the signed Closeout.

> One to two paragraphs. What was the project's goal? Did it ship? What does the world have now that it didn't have at the start? Narrative — saved you from having to read the rest of this doc to know whether the project succeeded.
>
> **Include both layers:** what the app does (functional outcome) and what the project taught (process outcome). For pipeline-test projects, the process outcome is often the more important deliverable.

[REP Tracker shipped on 2026-04-29 as a single-installer Windows desktop app for tracking IRS REP qualifying hours. The app is functional, signed-off against design, and installable on a clean machine. The pipeline test — design templates → coding docs → Claude Code context package → autonomous build — also succeeded: the build agent ran the 24-module Module Breakdown to completion with no human re-architecture during build. The retro surfaced ~32 template-improvement candidates, triaged into a 13-item Template Update Worklog that is now the input to the next project.] *(Example. Replace with this project's actual summary.)*

---

## 2. Phase Closeout Chain

> 🤖 **AGENT INSTRUCTIONS — Section 2**
>
> **Your job:** List every Phase Closeout file in order. Each row is a real, on-disk file. Verify each one exists before writing the row.
>
> **A complete Section 2:**
> - One row per phase in Module Breakdown's Phase Plan
> - File column points to a real `[AppName]_Phase_Closeout_[N].md` file
> - Date Signed pulled from the file's sign-off section
> - Modules in Phase pulled from the file's Section 2
> - Notable Entries: top 1-3 BD-XX entries from that phase's Section 4, with type icon
>
> **Hard rule (chain integrity):**
> If any phase has no closeout file: STOP. Do not proceed. A missing closeout means the chain is broken and this Project Closeout cannot honestly summarize the project. Either find the missing closeout or write it before continuing.
>
> **Cross-reference checklist:**
> - Every phase file referenced here exists on disk
> - Module IDs in the Modules column match Module Breakdown's Phase Plan
> - Notable Entries match the actual BD-XX entries in each phase's Closeout Section 4
>
> Remove this block before delivering the signed Closeout.

> Every Phase Closeout file in order. This is the audit trail — the chain Ryan signed his way through. Each link must be a real, on-disk file.

| Phase | File | Date Signed | Modules in Phase | Notable Entries |
|-------|------|-------------|------------------|-----------------|
| 1 | `[AppName]_Phase_Closeout_1.md` | YYYY-MM-DD | M-01 to M-04 | BD-001 (Workaround), BD-002 (Deviation) |
| 2 | `[AppName]_Phase_Closeout_2.md` | YYYY-MM-DD | M-05 to M-12 | BD-003 (Concern, → V2) |
| 3 | `[AppName]_Phase_Closeout_3.md` | YYYY-MM-DD | M-13 to M-18 | BD-004 (Reconciliation), BD-005 (Gap) |
| 4 | `[AppName]_Phase_Closeout_4.md` | YYYY-MM-DD | M-19 to M-24 | None this phase |

> If any phase has no closeout file: stop. Do not proceed. A missing closeout means the chain is broken and this Project Closeout cannot honestly summarize the project. Either find the missing closeout or write it before continuing.

---

## 3. Build Decisions Log Roll-Up

> 🤖 **AGENT INSTRUCTIONS — Section 3**
>
> **Your job:** Roll up every BD-XX entry across the project by type, count, and status. Open `[AppName]_Build_Decisions_Log.md` and walk it row by row. Counts must match the BD Log exactly.
>
> **A complete Section 3:**
> - Counts-by-type table with all 7 types (counts may be 0 — zero-counts are still rows)
> - Status mix per type matches the BD Log's actual statuses
> - Total row sums correctly
> - "Open at project end" sub-table lists every 🟠 Open entry with its Phase and the reason it's still open
> - "Accepted entries" sub-table lists every 🟣 Accepted entry with rationale
>
> **Hard rule (no 🟠 Open without disposition):**
> Any 🟠 Open entry at Project Closeout MUST be given disposition. Acceptable dispositions: ✅ Resolved (fix shipped), 🟣 Accepted (deliberate "won't fix" with rationale), or → V2 (moved to the V2 backlog table in Section 4). 🟠 Open at sign-off = project closing with unresolved liability; surface it explicitly to Ryan, do not silently close.
>
> **Cross-reference checklist:**
> - Total count matches the BD Log's Registry row count
> - Every 🟠 Open / 🟣 Accepted entry in the BD Log appears in the corresponding sub-table
> - Status icons match BD Log's status field exactly (no ✅ here for a 🟣 there)
>
> Remove this block before delivering the signed Closeout.

> Every BD-XX entry across the whole project, grouped by type, with counts. This is the at-a-glance shape of the project's actual decision history — far more honest than memory.
>
> **Source:** `[AppName]_Build_Decisions_Log.md`. Don't paraphrase entries — link to them. The log is the authoritative source.

### Counts by type

| Type | Icon | Count | Status mix (🟠 Open / ✅ Resolved / 🟣 Accepted) |
|------|------|-------|------------------------------------------------|
| Workaround | 🛠 | 3 | 🟠 0 / ✅ 2 / 🟣 1 |
| Reconciliation | ⚖️ | 2 | 🟠 0 / ✅ 2 / 🟣 0 |
| Concern | ⚠️ | 4 | 🟠 1 / ✅ 2 / 🟣 1 |
| Deviation | ↪️ | 3 | 🟠 0 / ✅ 3 / 🟣 0 |
| Gap | 🕳 | 2 | 🟠 0 / ✅ 1 / 🟣 1 |
| Dependency | 🔗 | 1 | 🟠 0 / ✅ 1 / 🟣 0 |
| Scope creep | 📈 | 1 | 🟠 0 / ✅ 0 / 🟣 1 |
| **Total** | — | **16** | 🟠 1 / ✅ 11 / 🟣 4 |

### Open at project end

> Any 🟠 Open entry at project end is a real liability — the project shipped with this unresolved. List them explicitly.

| BD ID | Type | Title | Why still open |
|-------|------|-------|----------------|
| BD-008 | ⚠️ Concern | Settings UI modal pattern conflicts with keyboard-first nav | Marked → V2 in Section 4. Behavior accepted in V1 with explicit user-visible workaround. |

> If "None": write **"All entries closed at project end (✅ Resolved or 🟣 Accepted)."**

### Accepted entries

> 🟣 Accepted means *deliberately living with this*. List them — these are the project's known-and-accepted compromises.

| BD ID | Type | Title | Acceptance rationale |
|-------|------|-------|---------------------|
| BD-001 | 🛠 Workaround | EF Core 8 SQLite parameter naming workaround | Vendor bug confirmed; workaround is small, isolated, documented. Revisit on EF Core 9. |
| BD-006 | 🕳 Gap | UI string locale handling not specified in design | English-only V1; gap accepted, design doc to be updated for V2. |

---

## 4. Concerns Raised vs. Resolved

> 🤖 **AGENT INSTRUCTIONS — Section 4**
>
> **Your job:** Walk every ⚠️ Concern entry in the BD Log. Each one gets a row with its raising Phase, disposition (Resolved / → V2 / 🟣 Accepted), and current Status.
>
> **A complete Section 4:**
> - One row per ⚠️ Concern entry across the entire project
> - Disposition is explicit — no "TBD" at Project Closeout
> - → V2 entries are marked with the literal arrow notation; these are the V2 backlog
>
> **V2 backlog convention:**
> There is no separate V2 backlog file. This table IS the V2 backlog. The next project's Pre-Design reads it directly.
>
> **Hard rule (zero-Concerns is a signal):**
> If genuinely no Concerns: write **"No ⚠️ Concern entries logged this project."** Then in chat: ask whether the project was unusually clean or whether Concerns weren't being captured. Concern-count of zero on a non-trivial project is itself a signal worth flagging to Ryan.
>
> **Cross-reference checklist:**
> - Every ⚠️ Concern in the BD Log appears here
> - Phase column matches the Phase Closeout that first logged the entry
> - Every → V2 entry has a clear scope-deferral rationale in the Disposition column
>
> Remove this block before delivering the signed Closeout.

> The Concerns pipeline — every ⚠️ Concern logged during the project, with disposition. Concerns are the most forward-looking entry type; they are the project's debt-vs-deferral statement.
>
> **V2 marking convention:** Concerns deferred to a future version are marked **→ V2** in the Disposition column. There is no separate V2 backlog file — this table *is* the V2 backlog, and the next project's Pre-Design phase reads it.

| BD ID | Title | Raised in Phase | Disposition | Status |
|-------|-------|-----------------|-------------|--------|
| BD-003 | Settings UI modal pattern conflicts with keyboard-first nav | Phase 2 | → V2 (accepted in V1, revisit when keyboard-first nav added) | 🟠 Open |
| BD-007 | First-launch wizard skipped if app crashes mid-init | Phase 3 | Resolved — added retry-on-launch logic | ✅ Resolved |
| BD-009 | Activity duration overflow not bounded above | Phase 3 | Resolved — capped at 24h with explicit error | ✅ Resolved |
| BD-011 | CSV export uses local timezone, ambiguous for cross-timezone CPAs | Phase 4 | → V2 (accepted in V1; CSV header notes timezone) | 🟣 Accepted |

> The next project's Pre-Design reads this table. → V2 entries are candidates for the next project's scope. 🟣 Accepted entries are *finalized* compromises and don't need revisiting unless surfaced again.

> If no Concerns: write **"No ⚠️ Concern entries logged this project."** — and reflect on whether that's because the project was unusually clean or because Concerns weren't being captured. Concern-count of zero on a non-trivial project is itself a signal.

---

## 5. Reconciliations Harvested

> 🤖 **AGENT INSTRUCTIONS — Section 5**
>
> **Your job:** Walk every ⚖️ Reconciliation entry in the BD Log. For each, capture: which design doc was updated, and what the Reconciliation reveals about the templates that should have caught this earlier.
>
> **A complete Section 5:**
> - One row per ⚖️ Reconciliation entry
> - "Design doc updated" column names the specific section that was edited (per the BD entry's "Doc updates needed" field)
> - "What this Reconciliation reveals about templates" column proposes the back-feed insight — even if it doesn't become a delta in Section 6
>
> **Why this section is critical:**
> Reconciliations are the back-feed loop's input. Every Reconciliation is *evidence* that something in the design phase didn't catch what build caught. Section 5 surfaces the pattern; Section 6 decides which ones become template deltas. Skipping Section 5 = skipping the back-feed loop.
>
> **Hard rule (zero-Reconciliations is a signal):**
> If genuinely no Reconciliations: write **"No ⚖️ Reconciliation entries this project."** Then ask in chat: was this a clean project, or did Reconciliations get logged as Deviations or not logged at all? Pattern-of-zero is a signal.
>
> **Cross-reference checklist:**
> - Every ⚖️ Reconciliation in the BD Log appears here
> - Every "Design doc updated" matches the actual edit (open the doc and verify the edit happened)
> - The back-feed insight is concrete enough to evaluate in Section 6 (not vague hand-wave)
>
> Remove this block before delivering the signed Closeout.

> Every ⚖️ Reconciliation entry. These are the moments where design doc said one thing, build needed another, and the reconciliation went *back into the design doc* (per the Cross-Doc Validation Sign-Off Scope rule). This is the back-feed loop's input — what the templates and process should have caught earlier.
>
> Not every Reconciliation produces a template delta — but every one is *evidence* that something in the design phase didn't catch what build caught. Section 6 decides which become deltas.

| BD ID | Title | Design doc updated | What this Reconciliation reveals about templates |
|-------|-------|-------------------|--------------------------------------------------|
| BD-004 | ActivityService method signatures diverged from Tech Spec | Tech Spec § ActivityService Contracts — material edit, partial re-validation done | Tech Spec template doesn't ask "what's the signature *contract* vs. the implementation freedom" — opportunity for template improvement |
| BD-010 | DB Schema cascade behavior unspecified for Activity → Project FK | DB Schema § Activity entity, FK Constraints — material edit, re-validated | Schema template doesn't enforce a cascade-decision per FK — every FK should require explicit `OnDelete` choice |

> If "None": write **"No ⚖️ Reconciliation entries this project."** Then ask in chat: was this a clean project, or did Reconciliations get logged as Deviations or not logged at all? Pattern-of-zero is a signal.

---

## 6. Template Deltas Proposed

> 🤖 **AGENT INSTRUCTIONS — Section 6**
>
> **Your job:** Produce two artifacts: the Deltas Proposed table below (in this doc), AND a real `[AppName]_Template_Update_Worklog.md` file written to disk using the Template_Update_Worklog_Template structure. The Closeout is the diagnosis; the worklog is the prescription.
>
> **A complete Section 6:**
> - Deltas Proposed table populated with one row per proposed template change
> - Each row has: trigger (BD-XX or section reference), target canonical template filename, category (A/B/C/D per the worklog template), and proposed direction
> - Worklog Generated table filled with the actual filename, location, creation date, item count, and source closeout reference
> - Worklog file actually exists on disk at the named path (the agent writes it before Section 6 is considered complete)
>
> **Hard rule (worklog must be written):**
> Section 6 is incomplete until the worklog file exists on disk. The Worklog Generated table is the audit trail — if it says the worklog was created on YYYY-MM-DD, the file with that creation date must exist. The agent does not declare Section 6 done without writing the file.
>
> **Source list for proposed deltas:**
> Pull candidates from these sources, in order:
> 1. Every ⚖️ Reconciliation in Section 5 (most direct back-feed signal)
> 2. Every 🕳 Gap in the BD Log that drove a design doc update (similar back-feed signal)
> 3. Every "What Didn't" item in Sections 7a and 7b that maps to a template improvement
> 4. Every cross-template inconsistency surfaced during the project (rare but high-value)
>
> **Categorization rule (A/B/C/D):**
> - **A** — New template needed (rare; only when an existing template can't host the change)
> - **B** — Overhaul (structural change to an existing template)
> - **C** — Touch-up (small targeted edit, single section or rule)
> - **D** — Out of scope / parking lot (acknowledge but defer)
>
> **Hard rule (zero-deltas is a signal):**
> If genuinely "None — no deltas proposed": write the explicit line **"No template deltas proposed. All template behavior survived this project unchanged."** Then ask in chat: is that because templates are mature, or because the retro under-surfaced learnings? Zero-deltas on a real project is a flag, not a victory.
>
> **Cross-reference checklist (verify before declaring Section 6 done):**
> - Every trigger references a real BD-XX, Section reference, or chat exchange
> - Every target template is a real canonical template filename
> - Worklog file exists on disk at the named path
> - Worklog item count matches the Deltas Proposed source list count (or the difference is justified)
> - Worklog header includes back-pointer to this Closeout Section 6
>
> Remove this block before delivering the signed Closeout.

> The output of this section is two things: the table below (deltas in-doc, for the Closeout's audit trail) AND a real `[AppName]_Template_Update_Worklog.md` file written to the project folder using the Template_Update_Worklog_Template structure.
>
> **The closeout is the diagnosis. The worklog is the prescription.** This doc captures *why* each delta exists; the worklog is the working document for *what gets done about each one*. Section 6 is the handoff between them.
>
> **Where the worklog goes from here:** Ryan works through the worklog in a separate session, adjusting the canonical templates in the Generic templates folder. Once the templates are updated, the worklog is closed (status: Done, with a closeout note inside the worklog). The next project reads only the updated canonical templates — the worklog is not propagated forward.

### Deltas Proposed (Source List)

> Every proposed change to a canonical template, with the trigger (what surfaced it), the target template file, and the proposed direction. Don't write the actual edit here — that happens in the worklog. Just identify what should change and why.

| # | Trigger (BD-XX or section reference) | Target template (canonical) | Category (A/B/C/D) | Proposed direction |
|---|-------------------------------------|------------------------------|---------------------|--------------------|
| 1 | BD-004 (Reconciliation, § 5) | `Tech_Spec_Template.md` | C — Touch-up | Add a "Service Contracts" sub-section requiring explicit signature stability declarations |
| 2 | BD-010 (Reconciliation, § 5) | `DB_Schema_Template.md` | C — Touch-up | Make `OnDelete` policy a required field per FK, not optional |
| 3 | § 7a What Didn't (Claude Code) | `Module_Breakdown_Template.md` | B — Overhaul | Acceptance criteria's Environmental category needs an explicit "first-launch on clean machine" sub-criterion for installer-shipping projects |
| 4 | § 7b What Didn't (Claude Chat) | `Instructions.md` | C — Touch-up | Add rule: "When proposing non-trivial structural edits to templates, propose-then-write — do not write directly" (formalize the working agreement that emerged mid-worklog) |
| 5 | BD-006 (Gap) | `App_Design_Context_Template.md` | C — Touch-up | Add localization/i18n line to scope-boundaries section by default — don't make it a discovery |

### Worklog Generated

> The actual working document for processing the deltas above. This is a real file on disk, not a notional one. After Project Closeout sign-off, this worklog is **Ryan's tool** for adjusting the canonical templates — it is not consumed by the next project.

| Field | Value |
|-------|-------|
| **Worklog filename** | `[AppName]_Template_Update_Worklog.md` |
| **Worklog location** | `C:\Users\rfalke\Documents\Claude Projects\[AppName]\` |
| **Worklog created on** | YYYY-MM-DD |
| **Worklog item count** | [N items, distributed: A=, B=, C=, D=] |
| **Source closeout (this doc)** | `[AppName]_Project_Closeout.md` § 6 |

**Worklog status at closeout sign-off:** 🔲 Not Started — to be worked through by Ryan before the next project begins. Output of working through it: updated canonical templates in the Generic templates folder. The worklog itself is not a deliverable to the next project.

> **Cross-reference rule:** The generated worklog must include a "Source Closeout" pointer back to this doc (§ 6) so the audit trail is bidirectional. Closeout points to worklog (in this section); worklog points to closeout (in its header).

> If "None — no deltas proposed": write the explicit line **"No template deltas proposed. All template behavior survived this project unchanged."** Then ask: is that because templates are mature, or because the retro under-surfaced learnings? Zero-deltas on a real project is a flag, not a victory.

---

## 7a. What Worked / What Didn't — Claude Code

> 🤖 **AGENT INSTRUCTIONS — Section 7a**
>
> **Your job:** Honest, evidence-based retro of the build agent's behavior during the project. Every claim points to a phase, module, or BD-XX entry. No vague "agent did well overall" — specific behaviors, specific evidence.
>
> **A complete Section 7a:**
> - 2-5 bullets in What Worked, each tied to evidence (phase, module, BD-XX, verify script result, test outcome)
> - 2-5 bullets in What Didn't, each tied to evidence AND cross-linked to a Section 6 delta if it became one
> - "None — no notable misses" only with an explicit reflection that this might be implausible
>
> **Distinction from 7b:** Code agent failure modes look like: misinterpreted spec, fabricated dependency, skipped verification step, drifted from Module Breakdown order. Chat agent failure modes look like: agreement bias, premature solution, lost thread, sycophantic walk-back. Don't mix.
>
> **Hard rule (zero-misses is a signal):**
> If genuinely "None — no notable misses": write that explicitly AND ask in chat whether that's plausible for a non-trivial project. The pattern of finding nothing wrong is itself a flag.
>
> **Cross-reference checklist:**
> - Every What Didn't bullet that became a template delta has "→ Delta #X in § 6" reference
> - Every What Worked bullet references the specific template/process that supported the behavior
>
> Remove this block before delivering the signed Closeout.

> The build-agent surface. What Claude Code did well during autonomous build, what it did poorly. Specific, evidence-based — every claim should point to a phase, a module, or a BD entry.
>
> **Why this is split from 7b:** Claude Code and Claude Chat have different failure modes. Code agent failures look like: misinterpreted spec, fabricated dependency, skipped verification step, drifted from Module Breakdown order. Chat agent failures look like: agreement bias, premature solution, lost thread, sycophantic walk-back. Mixing them obscures the back-feed targeting.

### What Worked

> Concrete behaviors during build that should be preserved/replicated. Each item: what happened, why it worked, what template/process supported it.

- [Honored Module Breakdown order strictly across all 24 modules. Verify-script convention surfaced one real bug at M-14 that would have leaked to integration. Module Breakdown's 2-minute verify rule made this fast enough to actually run.]
- [Build Decisions Log entries were typed correctly throughout — 16 entries, 0 mis-typed (verified manually at retro). Type-icon system (🛠 ⚖️ ⚠️ ↪️ 🕳 🔗 📈) gave enough scaffolding to disambiguate without thinking.]

### What Didn't

> Concrete behaviors during build that need correction. Each item: what happened, why it failed, what template/process should change (cross-link to § 6 if it became a delta).

- [Skipped first-launch-on-clean-machine verification at M-22 (installer module). Build agent assumed local dev environment was equivalent. Surfaced at user-acceptance, not at module verify. → Delta #3 in § 6.]
- [Did not log a ↪️ Deviation entry when ActivityService grew beyond spec. The deviation was caught in cross-check at retro, not in the moment. Possible cause: Build Decisions Log triggers are unclear for "expansion of scope within a single module." → Candidate for future iteration of Build Decisions Log template.]

> If genuinely "None — no notable misses": write that explicitly *and* ask whether that's plausible for a non-trivial project. The pattern of finding nothing wrong is itself a flag.

---

## 7b. What Worked / What Didn't — Claude Chat

> 🤖 **AGENT INSTRUCTIONS — Section 7b**
>
> **Your job:** Honest, evidence-based retro of the design/planning agent's behavior. Same evidence rule as 7a. Specific behaviors, specific evidence.
>
> **A complete Section 7b:**
> - 2-5 bullets in What Worked, each tied to evidence (Pre-Design pass, Cross-Doc Validation finding, BD-XX, Discussion file resolution)
> - 2-5 bullets in What Didn't, each tied to evidence AND cross-linked to a Section 6 delta if it became one
>
> **Chat agent failure modes to watch for:**
> - Sycophantic walk-back (agreed too quickly; reversed under pushback)
> - Premature solution (jumped to design before problem was scoped)
> - Lost thread (forgot context across long sessions)
> - Agreement bias (didn't push back when Ryan was wrong)
> - Under-scoping (failed to surface a category like i18n / accessibility / monitoring entirely)
>
> **Hard rule (zero-misses is a signal):**
> Same as 7a. Zero misses on a long project is more likely under-surfacing than a clean run.
>
> Remove this block before delivering the signed Closeout.

> The design/planning-agent surface. What Claude Chat did well during design and inter-phase work, what it did poorly. Same evidence rule as 7a.

### What Worked

> Concrete behaviors during design/planning that should be preserved/replicated.

- [Question-driven Pre-Design caught the durationMinutes integer-vs-hours ambiguity at the Tech Spec phase rather than at coding. Saved a guaranteed Reconciliation. Working pattern: stop on unfamiliar terminology, define before deciding.]
- [Cross-doc validation pass caught two material misalignments before coding started (DB Schema vs. Tech Spec on cascade behavior). Validation checklist's granularity rule worked as intended.]

### What Didn't

> Concrete behaviors during design/planning that need correction. Each item: what happened, why it failed, what template/process should change (cross-link to § 6 if it became a delta).

- [Working agreement around "propose non-trivial edits in chat before writing to disk" had to be established mid-worklog, not at start. Pattern that should be in Instructions from day one. → Delta #4 in § 6.]
- [Initial design phase under-scoped i18n entirely — not even mentioned as out-of-scope. Discovered at Phase 3 when CSV export needed timezone handling. → Delta #5 in § 6.]
- [Sycophantic walk-back behavior surfaced once during the worklog (agreed too quickly on a design call, then reversed when Ryan pushed back). Calibrated-honesty rule in Instructions caught it on second occurrence — but the rule was *just added* in this worklog. Effectiveness over multiple projects unproven.]

> If genuinely "None": same skepticism rule as 7a. Zero misses on a long project is more likely an under-surfacing than a clean run.

---

## 8. Outstanding Question

> 🤖 **AGENT INSTRUCTIONS — Section 8**
>
> **Your job:** Close the project with a question to Ryan. Same structural rule as Phase Closeout's Section 8.
>
> **Hard rule:** The last line of this doc is a question. Not a statement.
>
> **Sign-off side effect:** Approval here closes the project AND hands the worklog (Section 6) to Ryan for template adjustment. Before declaring this section ready for sign-off, verify the worklog file actually exists on disk and reflects what Section 6 specifies.
>
> **The approval question format is fixed below — do not edit it.** Only the App Name and worklog filename change.
>
> Remove this block before delivering the signed Closeout. Keep the question itself.

> **The last line of this doc is a question. Statements pass through silently; questions force a wait.**
>
> Sign-off here closes the project *and* hands the worklog (§ 6) to Ryan for template adjustment. The worklog is not the next project's input — the *updated canonical templates* are. Don't approve unless you've reviewed the worklog file on disk and it reflects what § 6 specifies.

---

**Ryan, do you approve closing the [App Name] project and accepting the generated `[AppName]_Template_Update_Worklog.md` as your working document for adjusting the canonical templates? (Reply "approved" to close the project. Reply with corrections to revise this doc and re-ask.)**
