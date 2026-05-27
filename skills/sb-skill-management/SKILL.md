---
name: sb-skill-management
description: Manages the full lifecycle of Claude skills at Stitser BUILT — from intake through creation, logging, and admin handoff. ALWAYS use this skill when a user says anything like "I want to create a skill", "let's build a skill", "can we make a skill for X", "I want to improve a skill", "update the skill for Y", or any variation of skill creation or enhancement. Also triggers when a user describes a workflow they want to systematize or automate into a repeatable Claude behavior. This skill prevents fragmented skill sprawl by evaluating existing skills before creating new ones, and closes the loop by logging every skill artifact to the S-BOS Activity Log and notifying the system admin to update the project knowledge base.
---

# Stitser BUILT Skill Management Protocol

This skill governs the complete lifecycle of Claude skills at Stitser BUILT: intake, deduplication check, creation or enhancement, artifact logging to SmartSuite, admin email notification, and HITL confirmation tracking.

---

## Step 1: Intake & Deduplication Check

Before writing a single line of a new skill, evaluate what already exists.

**Scan the skill inventory:**
```
/mnt/skills/public/       ← Anthropic-provided public skills
/mnt/skills/user/         ← Installed user/org skills
/mnt/skills/examples/     ← Example skills
/mnt/skills/organization/ ← Org-specific skills (if present)
```

**Also search the S-BOS Activity Log** (app ID: `69dc55333fe841263503f235`) for prior skill records:
- Filter by System Area = "Claude Skills" (or search title for skill keywords)
- Look for any existing entries describing similar capabilities

**Present findings to the user before proceeding.** Format:

> "Before creating something new, here's what I found that might overlap:
> - **[Skill Name]** — [what it does, where it lives]
> - **[Activity Log Entry]** — [date, description]
>
> Should we enhance one of these instead, or is the new skill distinct enough to stand alone?"

**Decision gate:**
- If enhancing → copy existing skill to `/tmp/[skill-name]/`, modify, repackage per update instructions below
- If new → proceed to Step 2

The goal is one robust skill over a fragmented library. Push back gently but clearly if the request duplicates existing capability.

---

## Step 2: Skill Creation or Enhancement

Follow the standard skill creation workflow from the `skill-creator` skill. Key steps:

1. **Capture intent** — what should the skill do, when should it trigger, what's the output format
2. **Interview for edge cases** — inputs, outputs, dependencies, success criteria  
3. **Write the SKILL.md** — name, description (triggering mechanism), body instructions
4. **Test** — run 2–3 realistic prompts against the skill; review with user
5. **Iterate** — refine based on feedback until user is satisfied
6. **Package** — run `python -m scripts.package_skill <skill-folder>` to produce `.skill` file

**For updates to existing skills:**
- Preserve the original `name` in frontmatter — never rename
- Copy to `/tmp/[skill-name]/` before editing (installed paths are read-only)
- Package from the `/tmp/` copy
- Output filename must match original (e.g., `sb-duplicate-contact-merge.skill`, not `sb-duplicate-contact-merge-v2.skill`)

**Output location:** Save the final `.skill` file to `/mnt/user-data/outputs/` and present it to the user.

---

## Step 3: Log to S-BOS Activity Log

After the user confirms the skill is complete, create a record in the S-BOS Activity Log.

**App ID:** `69dc55333fe841263503f235`
**Softr Page:** https://app.stitserbuilt.com/claude-s-activity-log-in-s-bos

**Field mapping:**

| Field | Slug | Value |
|-------|------|-------|
| Title | `title` | "[New Skill: skill-name]" or "[Skill Enhancement: skill-name]" |
| Timestamp | `s84937a653` | Current date |
| System Area | `s6d0bb9e98` | "Claude Skills" |
| Action | `sae070235c` | "Created" or "Enhanced" |
| Summary | `s85fec4906` | 2–3 sentence description of what the skill does and why it was built |
| Before State | `s2ee788d3d` | If enhancement: what the old skill did / what gap existed. If new: "No prior skill existed for this use case." |
| After State | `s3f1b71b74` | What the skill now enables; any existing skills it consolidates or replaces |
| Reasoning | `sfddfe3ab3` | Why this skill vs. enhancing an existing one; deduplication check results |
| Tags | `s77241ee45` | "skill, claude, [relevant domain tags]" |
| HITL Admin Update Complete | `s529f11d74` | false (leave unchecked — admin completes this) |

**Note on attachment:** The `.skill` file attachment (`s1df9b8fe8`) must be uploaded manually by the user or admin — the API does not support direct file attachment via MCP. Instruct the user to attach it to the record after creation.

After creating the record, tell the user: "I've logged this to the S-BOS Activity Log. Please attach the `.skill` file to that record — the API can't do it directly."

---

## Step 4: Notify System Admin

After logging the record, send an alert email to the system admin.

**To:** andrea@stitserbuilt.com  
**Subject:** `[Action Required] New Claude Skill Ready for Project Upload — [skill-name]`

**Body template:**
```
Hi Andi,

A new Claude skill has been created and is ready to be added to the Claude project knowledge base.

Skill Name: [skill-name]
Type: [New / Enhancement]
Created by: [user name or "Clint's session"]
Summary: [2–3 sentence description]

What needs to happen:
1. Download the .skill file from the S-BOS Activity Log record (linked below)
2. Upload it to the appropriate /mnt/skills/ directory in the Claude project
3. If this is a protocol or reference document, also upload the .md file to the project knowledge base
4. Once complete, check the "HITL Admin Update Complete" field on the Activity Log record

S-BOS Activity Log Record: https://app.stitserbuilt.com/claude-s-activity-log-in-s-bos

Thanks,
S-BOS
```

Use Gmail MCP to send this email. Confirm with the user before sending: "Ready to send the admin notification email — should I go ahead?"

---

## Step 5: Confirm Completion

Tell the user:

> "Here's where things stand:
> ✅ Skill packaged and downloaded
> ✅ Activity Log record created in SmartSuite
> ✅ Admin notification sent to Andi
> ⏳ Waiting on: Andi to upload the skill file and check 'HITL Admin Update Complete'
>
> Once Andi checks that field, the skill will be live in the system and available in all future sessions."

---

## Reference: Activity Log Field Slugs

```
title              → Title (primary)
s84937a653         → Timestamp
s6d0bb9e98         → System Area
sae070235c         → Action
s530c4b7b3         → Pillar
s2ee788d3d         → Before State
s3f1b71b74         → After State
s85fec4906         → Summary
s7de5538c4         → Actor (linked People)
s60981efcb         → Approver (linked People)
sfddfe3ab3         → Reasoning
s0cef0cec3         → Source Record
s77241ee45         → Tags
sd8adfeb3e         → Related Project (linked)
sc784ccf52         → Related Department (linked)
s1df9b8fe8         → Attachments & Reference Docs (file — manual upload only)
s529f11d74         → HITL Admin Update Complete (yes/no)
```

---

## Guiding Principles

- **One robust skill over ten fragmented ones.** Always check before creating.
- **The `.skill` file is the source of truth** — not the conversation, not a doc. The packaged file is what gets installed.
- **Admin closes the loop.** Claude creates and logs; the human confirms installation. The `HITL Admin Update Complete` field is the handoff point.
- **Every skill session is traceable.** The Activity Log record is the permanent record of what was built, why, and when.
