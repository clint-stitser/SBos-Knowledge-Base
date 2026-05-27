---
name: sb-duplicate-contact-merge
description: >
  Use this skill whenever a user needs to merge, deduplicate, or consolidate duplicate
  records in S-Bos — covering both People records (SmartSuite People table) and Project
  records (SmartSuite Projects table). Triggers include: "there are duplicate contacts",
  "merge these two records", "someone is in S-Bos twice", "clean up a duplicate",
  "remove a duplicate person", "these two projects are duplicates", "merge these projects",
  or any time a user pastes two S-Bos People or Project record URLs and asks for them
  to be consolidated. Always use this skill before attempting any manual record
  manipulation — it ensures the merge is auditable, reversible, and logged to the
  Decision Ledger.
---

# S-Bos Duplicate Record Merge

A structured, auditable workflow for merging duplicate records in S-Bos — covering
both **People** and **Project** records. Every merge is presented for human approval
before execution and logged to the S-Bos Activity Log (Decision Ledger).

---

## Record Type Detection

First, determine what type of record is being merged:

- If the user references people, contacts, or persons → follow the **People Merge** path
- If the user references projects, project records, or job records → follow the **Project Merge** path
- If unclear, ask: "Are these People records or Project records?"

---

---
# PEOPLE MERGE
---

## People — Step 1: Request the Duplicate Links

If the user has not already provided two S-Bos record URLs, prompt them:

> "Please paste the two S-Bos People record URLs you'd like me to merge. The URL
> format looks like:
> `https://app.stitserbuilt.com/sb-crm-home?modal=%2Fsb-crm-people-detail-screen%3FrecordId%3D<RECORD_ID>...`
>
> I'll extract the record IDs, pull both profiles, and show you a side-by-side
> comparison before touching anything."

Extract the `recordId` parameter from each URL.

---

## People — Step 2: Fetch Both Records

Use `smartsuite_get_record` on app `68216a706900e8eaf75a05af` (People) for both
record IDs.

Identify the following fields for comparison:

| Field | Slug |
|---|---|
| Name | `title`, `s3d6f88a8c` (First), `s1223b581d` (Last) |
| Email | `s70d9e051a` |
| Mobile Phone | `s8737d1b25` |
| FUB Link | `saca986cc1` (formula) / `s0c5153641` (Person # in FUB) |
| Primary SB Contact | `s0b6b2dedc` |
| SB Audience Segment | `s8b3ef417a` |
| Project Owner link | `s4esorjl` |
| Notes/Background | `sfa5b6cdc6` |
| Link to Notes & Comments | `sfw5qxl4`, `sqvxhdpg`, `si0w76u8` |
| Address | `s0c45a29b6` |
| Auto Number | `autonumber` |
| First Created | `first_created` |

---

## People — Step 3: Present Side-by-Side Comparison

Present a clear table showing both records before any action is taken.
Label them **Record A** (older, lower autonumber) and **Record B** (newer).

```
DUPLICATE MERGE — SIDE BY SIDE REVIEW
══════════════════════════════════════════════════════════════
FIELD                  RECORD A (keep/delete?)   RECORD B (keep/delete?)
──────────────────────────────────────────────────────────────
Record ID              [id_a]                    [id_b]
Auto Number            #[n]                      #[n]
Created                [date]                    [date]
──────────────────────────────────────────────────────────────
Name                   [value]                   [value]
Email                  [value or —]              [value or —]
Mobile                 [value or —]              [value or —]
FUB Person #           [value or —]              [value or —]
Primary SB Contact     [value or —]              [value or —]
Audience Segment       [value or —]              [value or —]
Project Owner Link     [value or —]              [value or —]
Notes/Background       [truncated to 100 chars]  [truncated to 100 chars]
──────────────────────────────────────────────────────────────
Linked Notes Count     [n]                       [n]
(sqvxhdpg / sfw5qxl4 / si0w76u8)
══════════════════════════════════════════════════════════════
```

Below the table, state your proposed merge plan clearly:

> **Proposed action:**
> - **Keep:** Record [A/B] — reason: [has FUB link / more complete / has linked notes]
> - **Discard:** Record [A/B] — reason: [missing phone / no FUB / no project link]
> - **Data to migrate before discard:** [list specific fields/linked records]
>
> **Please confirm:** Does this look right, or would you like any changes before I proceed?

**Do not proceed until the user explicitly confirms.**

---

## People — Step 4: Execute the Merge (After Approval)

### 4a — Migrate linked notes

Identify all note record IDs from these three fields on the **discard** record:
- `sfw5qxl4`, `sqvxhdpg`, `si0w76u8`

Combine with existing linked note IDs on the **keep** record (deduplicated).
Update the keep record via `smartsuite_update_record`.

### 4b — Migrate any unique field values

For any field that exists on the **discard** record but is empty on the **keep**
record, write those values to the **keep** record.

### 4c — Confirm the keep record is complete

Re-fetch the keep record and verify all expected fields are populated.

### 4d — Flag the discard record for deletion

The S-Bos MCP does not support record deletion. Instead:
- Provide the user with the direct S-Bos link to the discard record using this format:
  `https://app.stitserbuilt.com/sb-crm-home?modal=%2Fsb-crm-people-detail-screen%3FrecordId%3D<RECORD_ID>&modalSize=undefined&modalPlacement=end#tab2`
- State clearly that all data has been migrated and the record is safe to delete.
- Also draft a deletion email to Andrea Westrich (andrea@stitserbuilt.com) if the
  user wants admin to handle the deletion.

---

## People — Step 5: Log & Confirm

Follow the **Logging & Confirmation** section at the bottom of this skill.

---

## People Edge Cases

**Both records have different email addresses:** Always retain all unique emails.
S-Bos has two email fields:
- `s70d9e051a` — Primary Email (single-value)
- `sff039363e` — Secondary Email(s) (accepts multiple values)

Determine primary email by: (1) Gmail correspondence frequency, (2) ask Primary SB
Contact, (3) default to personal over work email.

**Both records have different phone numbers:** Surface both to the user and ask
which to keep before proceeding.

**Both records have different FUB Person # values:** Flag this — two FUB profiles
may exist. Ask the user to verify in FUB before merging.

**Both records have substantively different Notes/Background text:** Show both in
full and ask the user whether to keep one, the other, or merge the text.

**More than two duplicates:** Handle as a chain — merge the two most complete
records first, then repeat.

---
---
# PROJECT MERGE
---

## Projects — Step 1: Identify the Records

The user will either paste S-Bos project URLs or identify the projects by name.
S-Bos project URL format:
`https://app.stitserbuilt.com/sb-crm-projects-list-details?recordId=<RECORD_ID>`

Extract the record IDs. If the user provides names instead, use
`smartsuite_search_records` on app `68216a706900e8eaf75a05a7` (Projects) with a
title-contains filter to locate them.

---

## Projects — Step 2: Fetch Both Records & Check for Comments

Use `smartsuite_get_record` on app `68216a706900e8eaf75a05a7` (Projects) for both
record IDs.

**Always verify the discard candidate is clear of linked data before proceeding.**
Check the following fields on the record you intend to discard:

| Check | Field | Slug | Safe to discard if... |
|---|---|---|---|
| Open comments | comments_count | `comments_count` | = 0 |
| Linked Notes & Comments | Notes & Comments | `sjaf09yf` | empty array |
| Linked Decisions | Link to Decisions | `s82diibb` | empty array |
| Linked Invoices | Link to Intacct Invoice Line Items | `sacjjeqq` | empty array |
| Time Cards | Link to Time Cards | `s4abdo3y` | empty OR migrated |
| Milestones | Link to Milestones | `s6eaab3ab0` | empty OR migrated |
| Priorities | Link to Priorities Table | `sccf4ec32a` | empty OR migrated |

**If any of these contain data, do not proceed to discard until it is migrated or
explicitly acknowledged by the user.**

---

## Projects — Step 3: Present Side-by-Side Comparison

Present a comparison table before taking any action. Label them **Record A** (older,
lower autonumber) and **Record B** (newer).

```
DUPLICATE MERGE — PROJECT RECORDS
══════════════════════════════════════════════════════════════
FIELD                        RECORD A                RECORD B
──────────────────────────────────────────────────────────────
Record ID                    [id_a]                  [id_b]
Auto Number                  #[n]                    #[n]
Created                      [date]                  [date]
──────────────────────────────────────────────────────────────
Title                        [value]                 [value]
Status                       [value]                 [value]
Priority                     [value]                 [value]
Baseline Revenue             [value]                 [value]
Duration                     [value]                 [value]
Est. Const. Start            [value or —]            [value or —]
Project Manager              [value or —]            [value or —]
Project Estimator            [value or —]            [value or —]
Owner Company                [value or —]            [value or —]
Priorities count             [n]                     [n]
Milestones count             [n]                     [n]
Time Cards count             [n]                     [n]
Notes & Comments count       [n]                     [n]
Intacct Invoice Lines        [n]                     [n]
──────────────────────────────────────────────────────────────
comments_count               [n]                     [n]
══════════════════════════════════════════════════════════════
```

Below the table, state your proposed merge plan:

> **Proposed action:**
> - **Keep:** Record [A/B] — reason: [has more data / has revenue / has milestones]
> - **Discard:** Record [A/B] — reason: [empty shell / no financial data / created in error]
> - **Data to migrate before discard:** [list specific linked records/fields to move]
>
> **Please confirm:** Does this look right? Also, would you like to rename the keep
> record to clean up the title?

**Do not proceed until the user explicitly confirms.**

---

## Projects — Step 4: Execute the Merge (After Approval)

### 4a — Migrate linked records

For each data type present on the discard record, update the **keep** record to
include those IDs (merged/deduplicated with any existing IDs):

| Data type | Field slug |
|---|---|
| Time Cards | `s4abdo3y` |
| Milestones | `s6eaab3ab0` |
| Priorities | `sccf4ec32a` |
| Notes & Comments | `sjaf09yf` |
| Decisions | `s82diibb` |

Use `smartsuite_update_record` on the keep record with the merged arrays.

### 4b — Migrate unique field values

For any field populated on the discard record but empty on the keep record (e.g.,
G-Drive link, Sage Job ID, Intacct Project Record), copy those values to the keep
record.

### 4c — Confirm parent-child relationships

If either record has a Parent Project (`s3aaab6c14`) or Child Projects (`sae9008a11`),
ensure the keep record has those relationships set correctly. The discard record's
parent link should be removed and the keep record should inherit it if the keep
record doesn't already have one.

### 4d — Rename if needed

If the user approved a title cleanup, update the keep record's `title` and
`s937f1d342` (Project Name) fields.

### 4e — Provide the S-Bos deletion link

The S-Bos MCP does not support record deletion. Give the user the direct S-Bos link
to the discard record so they can delete it manually:

`https://app.stitserbuilt.com/sb-crm-projects-list-details?recordId=<DISCARD_RECORD_ID>`

State clearly that all data has been migrated and the record is safe to delete.

---

## Projects — Step 5: Log & Confirm

Follow the **Logging & Confirmation** section below.

---

## Projects — Edge Cases

**Discard record has active time cards:** Always migrate them to the keep record
before flagging for deletion. Never leave time cards orphaned.

**Both records have Intacct Project Records linked:** Flag this to the user — two
Intacct projects may exist. Do not proceed without user direction on which to keep.

**Both records have different G-Drive folders:** Surface both links to the user and
ask which to keep as primary (Link #1 / G-Drive Link field). Do not silently drop a
folder link.

**Both records have milestones or priorities:** Always migrate all IDs to the keep
record. These are linked records in other tables — they don't need to be recreated,
just re-pointed via the keep record's linked field.

**Records are in different pipeline stages:** Note the discrepancy and ask the user
which status/stage the keep record should reflect before executing.

---
---
# LOGGING & CONFIRMATION (Both Record Types)
---

## Log to the Decision Ledger

Write a record to the **S-Bos Activity Log** app (`69dc55333fe841263503f235`).

| Field | Slug | Value |
|---|---|---|
| Title | `title` | `Duplicate Merge — [Record Name] — [Date]` |
| Description | `description` | Full narrative of what was merged and why |
| Timestamp | `s84937a653` | Current date/time |
| System Area | `s6d0bb9e98` | `People` or `Projects` |
| Action | `sae070235c` | `Merge` |
| Before State | `s2ee788d3d` | Summary of both records pre-merge (key fields) |
| After State | `s3f1b71b74` | Summary of the keep record post-merge |
| Summary | `s85fec4906` | One-line summary of the action taken |
| Reasoning | `sfddfe3ab3` | Why this record was kept vs. discarded |
| Source Record | `s0cef0cec3` | S-Bos URLs of both the kept and discarded records |
| Actor | `s7de5538c4` | Gino Perano People record ID: `684081c7f5ffbca04d694a4a` |
| Approver | `s60981efcb` | Same as actor unless overridden |
| Related Project | `sd8adfeb3e` | Linked project record ID if applicable |

---

## Confirm to the User

Report back with:
1. What was kept and what was discarded
2. Which fields/records were migrated
3. The S-Bos link to the discard record for manual deletion
4. Confirmation that the Decision Ledger entry was created

Example summary:

> ✅ **Merge complete — [Record Name]**
>
> **Kept:** Record `[id]` — now contains all data from both records
> **Discard (safe to delete):** [S-Bos link]
>
> **Migrated:** [list what was moved]
>
> **Decision Ledger:** Entry created — [title of log record]
