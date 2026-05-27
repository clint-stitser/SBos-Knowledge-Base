---
name: brokerage-compliance-auditor
description: Run a real-estate brokerage compliance audit on a property contract — fetch the contract record from the SmartSuite Property Contracts table, walk the linked Google Drive Submittals folder, match every required document to its disclosure record in the Contract & Disclosure MGT System, update Status / Notes / G-Drive File Link, and create task records for any findings. Supports single-contract audits, batch sweeps across all open/in-flight contracts, and the 3-document outgoing-referral shortcut. Use this skill whenever the user says "audit [property/contract]", "run the compliance audit", "audit all open contracts", "do the close-of-escrow audit", "audit this referral", "link the Drive files for [contract]", or any phrasing that asks to evaluate the document package against the required-disclosures checklist.
---

# Brokerage Compliance Auditor

Audits a Stitser Properties (or any brokerage using the same SmartSuite + Google Drive setup) property contract against its required-document checklist. Reads the contract, walks the linked Drive Submittals folder, matches docs to disclosure records, marks each Audit Approved / Audit Disapproved / N/A / Not Started, links each record to its source file, and logs any findings as new task records.

This skill is the codified version of the workflow Clint Stitser developed and ran across 11+ audits between Apr–May 2026. It is designed to be invoked manually for a single contract, swept across a batch, or wrapped in a scheduled task for routine compliance checks.

---

## When to use

Trigger this skill when the user asks to:

- Audit a single property contract (by name, P# number, or A# number)
- Sweep all open / in-flight / pending-close contracts in one run
- Apply file-linking retroactively to a contract whose status was already set
- Run the close-of-escrow compliance review
- Audit an outgoing referral transaction (special 3-doc shortcut)

Do **not** use this skill for: contract creation, document drafting, agent commission accounting, or anything that doesn't end with disclosure records being updated.

---

## Required tools

The skill assumes these MCP connectors are connected on the team account:

- **SmartSuite** (`smartsuite_*`) — read/update Property Contracts (`67e47c67f965bafe2d1f6a6b`) and Contract & Disclosure MGT System (`67e9a329f3791ce6832979c2`)
- **Google Drive** — `search_files`, `browse_folder`, `get_file_metadata`, `read_file_content` to walk the Submittals folder
- (Optional) **Gmail** — only if a missing disclosure needs to be chased down via email thread context

If any of these are not connected, surface a one-line message naming the missing connector and stop.

---

## The two-step audit (single contract)

### Step 1 — Execute the audit

1. **Resolve the contract record.** Search the Property Contracts table by P#, A#, or property name. Confirm with the user if more than one match. Capture: contract record ID, the linked Google Drive folder URL, and the transaction type (Listing / Buyer / Seller Agency / Outgoing Referral / etc.).

2. **Pull the disclosure checklist.** Search the Contract & Disclosure MGT System for all records where the merged-formula contract name field (`sb221b2144`) equals the contract's display name. Do **not** filter by linked-record field `s23565bb3c` — that comparison errors out. Each result is one required document.

3. **Walk the Drive Submittals folder.** Open the Drive folder linked on the contract → `1.Submittals` → enumerate the four standard subfolders: `Listing`, `Contract`, `Disclosures & Reports`, `Closing`. Capture each file's name and `viewUrl`.

4. **Match documents to records.** For each disclosure record, find the Drive file whose name best maps to the `Required Document` (`sd39be3001`) value. Use fuzzy matching — listing agreements, agency disclosures, lead-based-paint, SRPDS/SRDS, well disclosure, settlement statements, etc. all have multiple naming conventions in practice.

5. **Evaluate additional required disclosures.** Cross-reference the contract's "Additional Required Disclosures" section. Confirm each appears in the package and is acknowledged/signed by all parties (initial blocks, signature pages, broker review).

6. **Capture findings.** Anything material that is not just a missing-document marker (e.g., property type changed mid-transaction, lead paint not acknowledged on a pre-1978 home, missing wire-fraud notice initials) becomes a finding. Findings become new task records in Step 2.

### Step 2 — Write the output

For each disclosure record, set:

- `Status` (`s1456ed1dd`) — see [status decision tree](references/status_decision_tree.md)
- `Additional Notes/Comments` (`s388c46ae0`) — short justification (always required if status is Audit Disapproved)
- `G-Drive File Link` (`s03be8f045`) — the matched file's `viewUrl` as a plain URL string; SmartSuite wraps it in an array

For each new finding, **create** a disclosure record with:

- `Where to File` (`s37c412398`) = `Task/Due Date Only` (option `qt7sg`)
- `Audit Stage` (`s84c8cccb0`) = `Close of Escrow` (option `jba1d`)
- `Property Contract File Name` field set to the contract's display name so the record threads under the same contract on upload

**API quirks to respect** (verified Apr–May 2026, see `references/smartsuite_reference.md`):

- Use `smartsuite_update_record` per record, **not** `smartsuite_bulk_update_records`. Bulk update returns success but does not persist changes. Parallelize per-record updates across tool calls instead — that's how the existing audits stayed fast.
- `Status` and other single-selects only support the `is` comparison operator; `is_one_of` errors out.
- For N/A items that have no matching file, leave the record entirely alone if it is already N/A. Don't touch its link or status.

---

## Batch mode

When the user asks for an "audit all" or "sweep open contracts" run:

1. Search Property Contracts where contract status is one of: `Active`, `Pending Close`, `In Escrow`, or other in-flight states (confirm the exact option labels with the user the first time).
2. For each contract, run the single-contract pipeline above — but do **not** mark required-but-missing items as Audit Disapproved. Use `Not Started` instead, because the package is in-flight by definition.
3. Aggregate a one-page summary at the end: contract name, # records audited, # approved, # N/A, # not-started, # findings logged, link to the contract record.

Parallelize contract-level work where possible (separate tool-call batches per contract).

---

## Outgoing-referral branch

If the contract's transaction type is "Outgoing Referral" (or the user explicitly says it's a referral), run the 3-document template instead of the full 77-item checklist. See [`references/referral_template.md`](references/referral_template.md). In short:

- Approve only the three docs that apply: Referral Fee Agreement, Settlement Statement, Commission Instructions
- Mark all other 74 records `N/A` with note `"N/A — Outgoing referral transaction."`

---

## Status decision tree (summary)

| Situation | Status |
|---|---|
| Required doc found in Drive, signed/initialed by all parties | **Audit Approved** (`Zn8iS`) + link |
| Required doc found in Drive, but missing signature/initial | **Audit Disapproved** (`Io61l`) + note explaining gap + link |
| Required doc not in Drive AND package is in-flight (user said incomplete or batch sweep) | **Not Started** (`c04lc`) |
| Required doc not in Drive AND package is closed/finalized | **Audit Disapproved** + note `"Required document not located in Submittals folder"` |
| Item genuinely doesn't apply (e.g., LBP for post-1978 home, Vacant Land Advisory for improved property, referral non-applicable items) | **N/A** (`UQTRH`) + brief justifying note |
| Item already set to N/A and no matching file | **leave alone** — no update |

Full reasoning and edge cases in [`references/status_decision_tree.md`](references/status_decision_tree.md).

---

## Walkthrough — a single audit, end to end

> User: "Audit Roaring Fork 2104 - Lindsay - P#124"

1. Search Property Contracts → single match, record `69cf18ab6d463b40fb155afa`. Note the Google Drive folder URL on the record.
2. Search Contract & Disclosure MGT System where `sb221b2144 = "Roaring Fork 2104 - Lindsay - P#124"` → 77 disclosure records returned.
3. Open the Drive folder → `1.Submittals` → enumerate Listing / Contract / Disclosures & Reports / Closing. Capture file names + viewUrls.
4. For each disclosure record, fuzzy-match the `Required Document` value to a Drive filename. Build a list of `{record_id, status, note, drive_url}` updates.
5. Issue `smartsuite_update_record` calls in parallel batches. Skip records that are already N/A and have no matching file.
6. Verify additional-disclosures section: SRPDS signed, LBP acknowledged (1995 build, so applicable), wire-fraud notice initialed. All present.
7. No new findings; no new task records to create.
8. Report back: `53 approved + 14 N/A; 10 already-N/A left alone`.

That is the actual run from Apr 28, 2026.

---

## Output to the user

End every audit run with:

- One-line summary: contract name, # approved / # disapproved / # N/A / # not started / # tasks created
- Direct `computer://` link to the SmartSuite contract record (if running in Cowork)
- List of any findings that became task records, with their record IDs
- If batch: a small table aggregating per-contract counts

Do not re-state the entire 77-row decision matrix in chat. Reference the SmartSuite view instead.

---

## References

- [`references/smartsuite_reference.md`](references/smartsuite_reference.md) — table IDs, field IDs, option codes, API quirks
- [`references/status_decision_tree.md`](references/status_decision_tree.md) — full status-selection logic
- [`references/referral_template.md`](references/referral_template.md) — outgoing-referral 3-doc shortcut
- [`references/known_audits.md`](references/known_audits.md) — audits already run, useful as test fixtures and to avoid re-auditing closed records

---

## Future plugin wrapper

When this skill is migrated to a plugin for routine/scheduled checks:

- Wrap "batch mode" in a scheduled task that runs weekly against contracts in Active / Pending Close status
- Emit a digest (Slack or email) listing any new findings since the last run
- Keep this `SKILL.md` as the plugin's primary skill; reference docs go under the plugin's `skills/brokerage-compliance-auditor/references/` path unchanged
