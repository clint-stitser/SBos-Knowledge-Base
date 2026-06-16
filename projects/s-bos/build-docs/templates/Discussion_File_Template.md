# Discussion: [App Name] — [Topic Slug]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized (light pass — freeform three-voice working file)
> **Fill order:** Created on-demand when Ryan promotes a `🟠 Open ⚠️ Concern` from `[AppName]_Build_Decisions_Log.md` (or, rarely, when Ryan opens a Discussion directly). NOT auto-created from open Concerns.
>
> **Filename convention:** `[AppName]_Discussion_[topic-slug].md` where `[topic-slug]` is short kebab-case. Example: `Icon_Discussion_modal-vs-keyboard-nav.md`.
>
> **Source docs:** Whatever docs the topic touches — Tech Spec, UI/UX, the BD-XX entry that promoted this Discussion, etc.
>
> **Downstream docs that consume this one:**
> - `[AppName]_Build_Decisions_Log.md` — typically the topic resolves into a BD entry (closure note in this Discussion file links to the resolving BD-XX or design-doc edit)
> - `[AppName]_Phase_Closeout_[N].md` and `[AppName]_Project_Closeout.md` — open Discussion topics surface at phase and project boundaries
> - Any design doc the topic touches — closure may be an edit to that doc
>
> **Agent role:** Three-voice scribe. The agent records Code's input, Chat's input, and Ryan's input as distinct voices in the discussion. The agent does NOT close the topic — only Ryan does. The agent does NOT collapse the voices into a synthesis — the three-voice structure is the point.
>
> **The three rules while maintaining this file:**
> 1. **One file = one topic.** Multiple topics → multiple files. Don't multi-thread inside one file. If a second topic surfaces during discussion, open a new Discussion file for it.
> 2. **Only Ryan closes.** Silence does not close. "Let's move on" does not close. The State field changes only when Ryan explicitly writes a closure entry. Code and Chat do not unilaterally mark a topic Resolved.
> 3. **Voices stay distinct.** Don't merge Code's voice into Chat's or vice versa. The three-voice structure exists to surface where Code and Chat actually disagree — collapsing them defeats the purpose.
>
> **When this doc is fully filled and closed:** Remove every `🤖 AGENT INSTRUCTIONS` block. Keep all three-voice content, every turn, and Ryan's closure entry. The finished Discussion is permanent reference — it documents how a contested topic was worked out.

**Purpose:** A single-topic, three-voice working file where Claude Code, Claude Chat, and Ryan work out a real disagreement or open question. Not a chat log. Not a decision log. This is where the *unresolved* lives until Ryan closes it.

**One file = one topic.** Multiple topics → multiple files. Don't multi-thread inside one file.

**Filename convention:** `[AppName]_Discussion_[topic-slug].md` where `[topic-slug]` is short kebab-case (e.g., `REPTracker_Discussion_modal-vs-keyboard-nav.md`). Keeps files sortable and grep-able alongside other project docs.

**When to write:** Ryan explicitly instructs Code or Chat to start a Discussion file on a flagged item. Discussion files are not auto-created from open Concerns — Ryan triages first, then promotes. This keeps the file count low and the signal high.

**Pair with:** `[AppName]_Build_Decisions_Log.md` (the topic typically resolves into a BD entry), Phase / Project Closeout (open topics surface there), and any design doc the topic touches.

---

## Status

| Field | Value |
|-------|-------|
| **Topic** | [One-sentence description of the actual question] |
| **State** | 🟠 Open |
| **Opened by** | [Code / Chat / Ryan] |
| **Opened on** | YYYY-MM-DD |
| **Current Turn** | [Code / Chat / Ryan] |
| **Closed on** | — (filled at resolution) |
| **Resolution link** | — (filled at resolution: BD-XX entry, design doc edit, or → V2) |

**State values:** 🟠 Open / 🟣 Parked / ✅ Resolved / 🚫 Abandoned

**Closure rule:** Only Ryan closes a topic. Silence does not close. "Let's move on" does not close. The State field above changes only when Ryan explicitly writes the closure entry in § 3 below.

---

## Source Docs / References

> Where this topic comes from and what it might affect. Fill in at file creation; update if scope shifts during discussion.

| Reference | Why it's relevant |
|-----------|-------------------|
| [BD-XX entry / design doc § / phase closeout § / cross-doc validation finding] | [What it triggered or what it would affect] |

---

## How to Use This Doc

1. **One topic per file.** If a sub-question emerges that deserves its own discussion, open a new file. Cross-link them.
2. **Voices speak in headings.** Each entry starts with `## 🤖 Code`, `## 💬 Chat`, or `## 👤 Ryan` — followed by a date stamp on the same line. This makes the file scannable at a glance.
3. **Current Turn at the top is the source of truth.** When you finish writing your entry, update the Current Turn field in the Status table to whichever voice you're handing it to. Without this, threads die from "I thought you were going to respond."
4. **Resolution is binary.** Ryan writes the closure entry in § 3 with the resolution link (BD entry ID, design doc section, or → V2 with rationale). State field flips to ✅ Resolved at that moment, not before.
5. **Resolved files stay on disk.** Don't delete. They're part of the project's audit trail — what was disputed, who said what, how it landed.

---

## 1. The Question

> One paragraph. State the actual question or disagreement plainly. No preamble, no narrative buildup. If you can't state it in one paragraph, the topic isn't well-formed yet — sharpen before opening the file.

[Example: The Module Breakdown specifies a modal Settings dialog. The keyboard-first navigation pattern in the UI/UX doc implies users should be able to reach Settings without lifting hands from the keyboard. Modal dialogs interrupt keyboard-first flow. The question: keep the modal as specified, or replace with an in-place panel that preserves keyboard navigation? — REPLACE THIS WITH THE REAL QUESTION.]

---

## 2. Discussion

> Three-voice exchange. Each entry is a heading + date + body. Voices alternate as needed — not strict rotation. Keep entries focused: one position per entry, evidence where possible, no repeating what an earlier entry already said.
>
> **Update the Current Turn field at the top after every entry.**

### Format example (delete this block when first real entry is written)

```
## 🤖 Code — YYYY-MM-DD

[Code's position. Short, evidence-based. Reference modules, BD entries, file paths where relevant.]

## 💬 Chat — YYYY-MM-DD

[Chat's position. Short, evidence-based. Reference design docs, sections, decisions where relevant.]

## 👤 Ryan — YYYY-MM-DD

[Ryan's position, question, or arbitration.]
```

### Entries

[Real entries go here, in chronological order, top to bottom.]

---

## 3. Resolution

> Filled by Ryan only, when the topic closes. Until then, this section reads "— (open)".
>
> **Required content at closure:**
> - **Decision:** What was decided, in one sentence.
> - **Mechanism:** How the decision is preserved — BD-XX entry, design doc edit (with section), or → V2 marking.
> - **Closed on:** Date.
> - **Final State:** ✅ Resolved (decision made), 🚫 Abandoned (topic no longer relevant — with reason), or 🟣 Parked (deliberately paused — with reason and revisit trigger).
>
> When this section is filled, update the Status table at the top to match (Closed on, Resolution link, State).

**Status:** — (open)

---

## 4. Notes

> Optional. Anything that's relevant context but doesn't fit in The Question, Discussion, or Resolution. Examples: prior conversations referenced, external links, screenshots, drafts considered and discarded.
>
> Delete this section entirely if not used. Don't leave a placeholder.

—
