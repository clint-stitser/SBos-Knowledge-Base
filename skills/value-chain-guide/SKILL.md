---
name: value-chain-guide
description: Guides team members through documenting, editing, and reviewing value chain workflow documents for the Stitser BUILT platform. ALWAYS use this skill when a user says anything like "I want to document a workflow", "let's map out how we do X", "help me write a value chain", "how do I document a process", "I want to add a new workflow to the system", "let's update a value chain", "review a value chain with me", or any variation of workflow documentation, process mapping, or value chain creation and editing. Also triggers when a team member describes a business process they want to capture or when they reference a step-by-step operational procedure that isn't yet in the Kompass knowledge base.
---

# Value Chain Guide — Kompass Interview Protocol

This skill turns a conversation into a structured value chain document. Follow the protocol exactly. Do not rush. Do not fill in answers the team member didn't give you — ask first, assume nothing.

---

## When This Skill Applies

You are in value-chain-guide mode when a team member wants to:
- Document a new workflow for the first time
- Update or correct an existing value chain doc
- Review a draft value chain and flag gaps
- Convert a meeting discussion into a formal value chain document

---

## Opening Statement

Say this (or a close variation) to open:

> "We're going to document a workflow together. I'll ask you questions and build the document as we go. You can stop at any point and say **'save draft'** to capture what we have so far, or **'submit for integration'** when you're ready to send it to the S-BOS team for system integration."

---

## Interview Protocol — Question Sequence

Ask these questions in order. Wait for the answer before moving to the next. If an answer is unclear or incomplete, ask a follow-up before proceeding.

**Q1 — Workflow Name**
> "What's the name of this workflow — what would you call it in one short phrase?"

*Aim for something like "Project Setup and Baselining" or "Pay App Processing" — not a full sentence.*

**Q2 — Product Lines**
> "Which product lines does this apply to — all of them, or specific ones?"

*Options: All | 01-Asset Disposition | 02-Retail | 03-Multifamily | 04-Entry Level | 05-3P Construction*

**Q3 — Situation**
> "Which situation does this live in?"
> - S1 — Business Development (pursuing deals)
> - S2 — Pipeline (deal committed, not yet started)
> - S3 — Work in Progress (active project)
> - S4 — Closeout (project wrapping up)
> - S5 — Asset Management (post-close ongoing management)

**Q4 — Trigger**
> "What triggers this workflow — what has to happen before someone starts this?"

*Push for specificity: a record status change, a document received, a meeting outcome, a date hit. Vague answers like "when the project starts" are not enough.*

**Q5 — Steps**
> "Walk me through the steps one at a time. For each step, tell me:
> 1. Who does it? (role, not just name)
> 2. What exactly do they do? (use a verb + object — 'Submit the pay app to the owner's rep')
> 3. What system or tool do they use? (SmartSuite app name, spreadsheet, phone call, etc.)
> 4. What's produced when the step is done? (a record, a document, a decision)"

*Capture each step as its own block. Do not combine two distinct actions into one step.*

**Q6 — Product-Line Variations**
For each step, ask:
> "Does this step work the same way across all product lines, or does it change depending on which line you're in?"

*If a step is shared: mark it Shared. If it varies: note which product lines differ and how.*

**Q7 — Completion Signal**
> "How does the team know this workflow is done? What's the completion signal — what field is updated, what record status changes, or what document is produced?"

**Q8 — Upstream / Downstream Workflows**
> "What workflow feeds into this one — what has to be done before this starts?"
> "What does this workflow feed into next — what gets triggered when this is complete?"

*Use Workflow IDs if known (e.g., vc-all-s2-001). If unknown, describe in plain language and mark as TBD.*

**Q9 — Gaps and Open Questions**
> "Are there any gaps, open questions, or things that aren't documented anywhere yet? Any steps that are fuzzy, decisions that need Clint's sign-off, or SmartSuite fields that aren't confirmed?"

---

## Document Assembly

After collecting all answers, assemble the structured markdown document using this exact format:

```
# Value Chain: [Workflow Name]

## Header
| Field | Value |
|---|---|
| **Workflow ID** | vc-[product-line-code]-[situation]-[sequence] |
| **Product Lines** | [All | product line name(s)] |
| **Situation** | [S1-Biz-Dev | S2-Pipeline | S3-WIP | S4-Closeout | S5-Asset-Mgmt] |
| **Shared or Specific** | [Shared | Product-Specific] |
| **Owner Role** | [Role title — not person name] |
| **Current Owner** | [Person name or "Varies by product line"] |
| **Status** | Draft — needs Clint review |
| **Last Updated** | [YYYY-MM-DD] |
| **Version** | 0.1 |

## Overview
[2-3 sentences. What this workflow accomplishes, why it exists, and what happens if it's skipped.]

## Trigger
[What event or condition starts this workflow. Be specific — a record status change, a meeting outcome, a document received, etc.]

## Steps

### Step [N]: [Step Name]
| Field | Value |
|---|---|
| **Who** | [Role] |
| **What** | [Action — verb + object] |
| **Tool / System** | [SmartSuite app name, external tool, or manual] |
| **SmartSuite Field(s)** | [Relevant field slugs if known, or "TBD"] |
| **Output** | [What is produced or updated when this step is complete] |
| **Shared or Specific** | [Shared | Specific to: product line name(s)] |
| **Pillar** | [People | Alignment | Schedule | Budget | Checklists | N/A] |

## Completion Signal
[How does the team know this workflow is done?]

## Upstream Workflows
[Workflows that must be complete before this one starts. Use Workflow IDs if known.]

## Downstream Workflows
[Workflows this workflow feeds into. Use Workflow IDs if known.]

## Product-Line Variations
[Document any steps or rules that differ by product line. If none, write "None — identical across all product lines."]

## Known Gaps / Open Questions
[Anything unresolved, undocumented, or needing Clint's confirmation. Delete section if none.]

## Change Log
| Date | Change | By |
|---|---|---|
| [YYYY-MM-DD] | Initial draft from [team member name] interview | Kompass (S-BOS System Admin) |
```

### Workflow ID Assignment

Format: `vc-[product]-[situation]-[sequence]`

| Code | Meaning |
|---|---|
| `vc-all` | Shared across all product lines |
| `vc-01` | Asset Disposition only |
| `vc-02` | Retail only |
| `vc-03` | Multifamily only |
| `vc-04` | Entry Level only |
| `vc-05` | 3P Construction only |
| `s1`–`s5` | Situation code |
| `-001`, `-002` | Sequence within that product+situation |

If you are not sure of the next sequence number, use `TBD` and note it in Known Gaps.

### Pillar Assignment per Step

Each step maps to one of the five S-BOS pillars:
- **People** — team structure, roles, assignments, contacts
- **Alignment** — meetings, approvals, decisions, sign-offs
- **Schedule** — milestones, dates, timelines, deadlines
- **Budget** — cost tracking, approvals, actuals vs. budget
- **Checklists** — status updates, record management, field updates

---

## Review Step

After assembling the document, present it to the team member:

> "Here's the document I've built from our conversation. Read through it and tell me if anything needs to change — missing steps, wrong roles, unclear outputs, anything. When you're ready to send it to the S-BOS team for system integration, say **'submit for integration.'**"

Make all corrections the team member requests before submission.

---

## Submit for Integration

When the team member says **"submit for integration"**, create a SmartSuite record:

**Solution:** S-BOS Platform (IT/Systems projects)
**App:** Projects app (IT/Systems department)

**Record fields:**
- **Title:** `[VALUE CHAIN UPDATE] [Workflow Name]`
- **Department:** IT/Systems
- **Project Type:** IT/Systems
- **Status:** Active in Pipeline
- **Description / Notes:** Paste the full structured markdown document into the notes or description field
- **Tag:** `value-chain-intake`

After creating the record, confirm with the team member:

> "Done. I've submitted **[Workflow Name]** to the S-BOS IT/Systems queue. The S-BOS System Admin (Claude Code) will pick it up, validate the format, commit it to GitHub, and sync it to this knowledge base. You'll see it in the value chain library once it's integrated.
>
> If you flagged any open questions or gaps, those will come back to you or Clint for resolution before the workflow goes to Production status."

---

## Save Draft

When the team member says **"save draft"**, output the document in its current state with a note:

> "Here's the draft so far. Copy this and save it wherever you like — a note, a doc, an email to yourself. When you're ready to continue or submit, come back and paste it here and we'll pick up where we left off."

---

## Editing an Existing Value Chain

If the team member wants to update an existing workflow:
1. Ask them to paste the existing document (or tell you which workflow ID it is)
2. Ask: "What specifically needs to change — a step, a role, a tool, a completion signal, something else?"
3. Make the targeted edits only — do not rewrite sections that weren't mentioned
4. Bump the version number (0.1 → 0.2, etc.) and add a Change Log entry
5. Follow the same Submit for Integration flow when done

---

## Quality Standards

Before presenting any document for review, verify:
- [ ] All steps have Who, What, Tool/System, and Output filled in (no blanks)
- [ ] Every step is assigned a Pillar
- [ ] Completion Signal is specific — not "when it's done" but a concrete field or record state
- [ ] Upstream and downstream workflows are listed (even if TBD)
- [ ] Product-line variations are documented for any step where they exist
- [ ] Known Gaps section captures anything said aloud but not resolved
- [ ] Workflow ID follows the `vc-[product]-[situation]-[sequence]` format
- [ ] Status is set to "Draft — needs Clint review"

If any of these are missing, ask the team member for the missing information before presenting the document.

---

## Guiding Principles

- **Document what they know, flag what they don't.** Never invent a step, role, or tool. If something is unclear, mark it as TBD and add it to Known Gaps.
- **One action per step.** If a step description has "and" in it, it's probably two steps.
- **Specificity is the product.** "Reviews the budget" is not a step. "Reviews line-item budget in SmartSuite Budget app and approves or flags variances over $500" is a step.
- **The document is for humans first.** Write it so a new team member with no context could follow it on day one.
- **Kompass captures; Claude Code integrates.** Your job is to produce a clean, complete document. The integration pipeline takes it from there.
