---
name: pay-app-audit-checklist
version: 1.3
description: >
  Drives the per-invoice PM audit for any Pay App at Stitser BUILT. Reads all
  sub-contractor invoices on a Pay App via the Kompass MCP, creates 7 standard
  audit tasks directly linked to each invoice in Check List Tasks, surfaces
  value-approval decisions conversationally, generates a 3-page CoWork document
  package (draft email + G-5 Application for Payment + NRS conditional lien
  waiver), deposits per-invoice packages into the PA Drive folder for PM
  review and sending, tracks send and return, and confirms the signed package
  is filed back to the invoice Drive field. The task list IS the audit history
  log — every state transition is timestamped in the task description. The
  invoice record is the parent. No intermediate checklist record.

trigger: >
  "run the audit on PA-N", "audit the invoices for [project]",
  "which invoices need waivers", "generate waiver packages",
  "what's the doc status on PA-N", "process the pay app audit",
  "generate the doc packages", "create the CoWork packages",
  or any phrase moving invoices toward PA lock.

changelog:
  v1.3:
    - "BREAKING: GC allocations (Remove for Export) now skipped with ZERO tasks
      created — previously created tasks then marked NA, causing dirty data"
    - "BREAKING: Pass-through receipts now set Tasks 4-7 to status=rz4lv (N/A)
      with rating cleared — previously set to complete/NSH2u, which was wrong"
    - "BREAKING: Corrected live task title mismatches — all 7 titles now match
      what actually exists in S-BOS (Tasks 5 and 6 renamed)"
    - "NEW: Phase C now generates 3-page CoWork document packages instead of
      Gmail drafts — Page 1 is draft email, Page 2 is G-5, Page 3 is waiver"
    - "NEW: Waiver type logic — G-1 Conditional for outgoing packages (payment
      not yet received), G-2 Unconditional for post-payment (PM escalates)"
    - "NEW: Dual recipient logic — To: AR/AP contact, CC: Authorized Signer
      (both pulled from Companies/Entities fields s4rct6xh and s044084fbd)"
    - "NEW: Drive folder deposit — packages go into PA-level subfolder in the
      project Drive folder, invoice s5fd21f93c updated with file link"
    - "NEW: Subject line format confirmed — '[Project] — PA-#N Lien Waiver &
      Application — [Vendor Name]'"
  v1.2:
    - "7-task model replacing 5-task model, no intermediate Check List record"
    - "Task dedup via suxr9w93 backlink"
    - "Budget context on Task 1 creation"
    - "G-703 data pull for form pre-fill"
    - "Legacy task cleanup note"
  v1.1:
    - "Initial 5-task model, Check List record as parent"
---

# Pay App Invoice Audit Checklist Skill

## Core Principle: The Task List Is the Audit Log

Every task has a description field that accumulates timestamped log entries as
state changes occur. When a PM approves value, Claude appends a log line.
When a package is generated, Claude appends the filename and timestamp.
When docs are sent, Claude appends the recipient and send date. When signed
docs return and are filed, Claude appends the Drive URL and filing date.

This means at any point you can open any task and read the full history of
what happened and when — without reconstructing it from emails or memory.

The invoice record is the parent. Tasks link to it via `s952ca3b56`.
`suxr9w93` on the invoice shows all tasks. No intermediate checklist record.

---

## The 7 Standard Audit Tasks

Every auditable sub-contractor invoice gets exactly these 7 tasks.
Titles are fixed strings — idempotency keys for dedup on re-run.

**⚠️ CRITICAL: Use these exact title strings. They must match what is in the
live S-BOS database. Tasks 5 and 6 were renamed in v1.3 to match live data.**

| # | Title (exact string) | Gate | Auto-eval? |
|---|---|---|---|
| 1 | `VALUE: Confirm billing amount vs budget` | PM decision | No |
| 2 | `WORK: Confirm work completed / super sign-off` | PM / super | No |
| 3 | `DOCS: Invoice PDF in Drive` | Drive link present | Yes |
| 4 | `DOCS: Doc package created (G-5 + waiver)` | PDF generated | No — Claude updates |
| 5 | `DOCS: Lien waiver sent for signature` | Email confirmed sent | No — PM confirms |
| 6 | `DOCS: Signed waiver + application returned` | Sub returns signed docs | No — PM confirms |
| 7 | `DOCS: Signed package filed to Drive + invoice linked` | Drive filed + `s5fd21f93c` updated | No — Claude updates |

### Default creation state per task

| Task | Default Rating | Default Status | Priority |
|---|---|---|---|
| 1 | `J0NLz` | `in_progress` | `urgent` if over-budget, else `normal` |
| 2 | `J0NLz` | `in_progress` | `normal` |
| 3 | Auto (see below) | Auto | `normal` |
| 4 | `J0NLz` | `backlog` | `normal` |
| 5 | `J0NLz` | `backlog` | `normal` |
| 6 | `J0NLz` | `backlog` | `normal` |
| 7 | `J0NLz` | `backlog` | `normal` |

**Task 3 auto-evaluation:**
- `s5fd21f93c` populated → `NSH2u` / `complete`, description = "INIT — Drive link present — auto-approved."
- `s5fd21f93c` empty → `J0NLz` / `in_progress`, description = "INIT — No Drive link found. PM to upload source invoice PDF and update s5fd21f93c."

**Pass-through receipts** (KCSH001 vendor + not "GC Alloc" invoice_name):
- Tasks 4–7: status = `rz4lv` (N/A), rating = `""` (cleared), description = "N/A — pass-through receipt, no lien waiver required."
- **DO NOT set to complete/NSH2u.** `rz4lv` is the N/A status slug. Softr filters these out of PM view.

**GC Allocations** (`s3fdf4e04c` = "Remove for Export"):
- **Skip entirely — create ZERO tasks.** Do not create any tasks and do not update any existing tasks on these invoices.

---

## Assessment Rating Values

- `NSH2u` = 2-Approve (green) — done, clear
- `GLsXI` = 1-Conditional (amber) — in flight, action pending
- `J0NLz` = 0-Fail (red) — blocked, needs action
- `""` (empty) = unrated — used for N/A tasks

---

## Status Values

`backlog` → `in_progress` → `ready_for_review` → `complete`

**Special:** `rz4lv` = N/A (not applicable) — use for tasks that do not apply to an invoice type. This is NOT the same as `complete`. Softr filters `rz4lv` tasks out of PM checklist views.

---

## Description Log Format

Every task description accumulates append-only log entries. Format:

```
[YYYY-MM-DD HH:MM] ACTION — {detail}
```

Examples:
```
[2026-05-15 14:22] INIT — Budget context loaded. Gross: $15,000 | Scheduled: $168,500 | Over budget: YES (+$8,820). FLAG: Billing 5x original contract — CO 10 approved $8,500, verify remainder.
[2026-05-15 14:35] VALUE APPROVED — PM confirmed billing acceptable pending CO verification. Rated 1-Conditional.
[2026-05-16 09:10] PDF CREATED — PA7_NNM_HVAC_Inv2100_G1.pdf generated. Pages: 3 (email draft + G-5 + NRS G-1 Conditional). Filed to Drive folder: [URL].
[2026-05-16 09:12] SENT — Waiver package sent to office@nnmhvac.com (Darcy Valdez, AP/AR) + spolan@nnmhvac.com (Scott Polan, Authorized Signer). Subject: "677 Virginia Restaurant TI — PA-7 Lien Waiver & Application — Northern Nevada Mechanical".
[2026-05-18 11:04] RETURNED — Signed package received from office@nnmhvac.com.
[2026-05-18 11:06] FILED — Signed PDF uploaded to Drive. URL: https://drive.google.com/... Invoice s5fd21f93c updated with signed package URL. Audit complete.
```

Claude appends to the description on every state change. Never overwrites —
always appends so the full history is preserved.

---

## Data Model

### Apps

| App | ID |
|---|---|
| Pay Apps | `68db724638a208d3257ea3a9` |
| Bills & Invoices | `68a8c3d2bba73ca6e62d1297` |
| G-703 Line Items | `68db71a363e88ace0bd45439` |
| Companies/Entities | `68216a706900e8eaf75a05c0` |
| People | `68216a706900e8eaf75a05af` |
| Check List Tasks | `68a8e17251dc814e8c529f3f` |

### Bills & Invoices — key fields

| Field | Slug | Notes |
|---|---|---|
| Tasks backlink | `suxr9w93` | Reverse of `s952ca3b56` — idempotency check |
| Drive file URL | `s5fd21f93c` | Updated twice: source invoice + signed return |
| Remove for Export | `s3fdf4e04c` | "Remove for Export" = GC alloc, skip entirely |
| Split children | `sad3605fad` | Non-empty = split parent, skip |
| Amount | `amount` | Gross this period |
| Retention % | `sf74e1bbd4` | Whole number (5 = 5%) |
| Vendor | `s0bcc3e24a` | → Companies |
| G-703 line | `s0ef26247a` | → G-703 |
| Memo | `s83e0052b4` | Contains PA# and cost code |
| Cost code lookup | `sbed918344` | |
| Budget check | `s94d2830bf` | "OVER BUDGET" = urgent |
| Scheduled value lookup | `sa284e6b1e` | |
| Gross to date lookup | `sa92d2595d` | |
| Balance to finish lookup | `s94ca7fec4` | |
| Vendor code lookup | `s21078d2e9` | KCSH001 = possible pass-through |
| Invoice number | `s3e3663e67` | Used in filename and form |

### Companies/Entities — key fields (vendor contacts)

| Field | Slug | Notes |
|---|---|---|
| Authorized Signer | `s044084fbd` | → People — signs G-5 and waiver |
| Accounting AP/AR Contact | `s4rct6xh` | → People — receives the email |

**Recipient logic for outgoing packages:**
- **To:** AR/AP Contact (`s4rct6xh`) — the person who receives payment correspondence
- **CC:** Authorized Signer (`s044084fbd`) — the person whose name goes on the signature line
- If the two fields resolve to the same Person record, send to that person only (no self-CC)
- If either field is empty, log a BLOCKED note on Task 5 and flag for manual follow-up

### People — key fields

| Field | Slug | Notes |
|---|---|---|
| Email | `s70d9e051a` | Primary email |
| First name | `s3d6f88a8c` | Used in email salutation |
| Last name | `s1223b581d` | |
| Full name | `title` | Pre-composed |

### Check List Tasks — key fields

| Field | Slug |
|---|---|
| Title | `title` |
| Status | `status` |
| Priority | `priority` |
| Assessment Rating | `s3dcc09bbd` |
| Linked to invoice (parent) | `s952ca3b56` |
| Link to Project | `se5f41aa17` |
| Description / audit log | `description` |

---

## Phase A — Initialization

### A.1 — Identify the Pay App

- "audit PA-7 on 677 Virginia" → search Pay Apps by title containing "PA-7"
  and "677 Virginia"
- Softr/S-BOS URL → parse `recordId` parameter

Fetch Pay App. Read `s5bea81732` for all invoice IDs.

### A.2 — Classify invoices

**Skip with ZERO tasks:**
- `s3fdf4e04c` = `"Remove for Export"` → GC allocation — create no tasks at all
- `sad3605fad` non-empty → split parent — create no tasks (children are auditable)
- `amount` = 0 or null — skip

**Pass-through (Tasks 4–7 = N/A status `rz4lv`):**
- Vendor code `s21078d2e9` = `KCSH001` AND `invoice_name` not starting with `"GC Alloc"`
- Create all 7 tasks, but Tasks 4–7 get status `rz4lv`, rating cleared, description = "N/A — pass-through receipt, no lien waiver required."

**Auditable sub-contractor invoice:**
- Everything else with amount > 0 — full 7 tasks, all defaults per table above

Report classification before proceeding:
> "PA-7: 18 GC allocations skipped (zero tasks), 4 pass-throughs (Tasks 4-7 = N/A), 28 sub-contractor invoices → full 7-task audit."

### A.3 — Idempotency check

Read `suxr9w93` on each invoice. Fetch those tasks and compare titles to the
7 standard strings. Create only missing tasks. Re-run produces zero new records
if all 7 are already present.

**Legacy task detection:** If tasks found have non-standard titles (not matching
the 7 exact strings) → these are legacy ad-hoc tasks. Leave them in place.
Create the 7 standard tasks alongside them. Report:
> "Found N legacy audit tasks on invoice X. Preserved as notes. Created 7 standard tasks."

### A.4 — Create the 7 tasks

Sequential `smartsuite_create_record` calls. Set description on Task 1 at
creation with full budget context (see A.5).

```python
is_over_budget = "OVER BUDGET" in invoice.get("s94d2830bf", "")
has_drive = bool(invoice.get("s5fd21f93c"))
vendor_code = (invoice.get("s21078d2e9") or [[[""]]])[0][0][0]
is_passthrough = vendor_code == "KCSH001" and not invoice.get("invoice_name","").startswith("GC Alloc")
now = datetime.utcnow().strftime("%Y-%m-%d %H:%M")

# N/A description for pass-through tasks 4-7
na_desc = {"data":{"type":"doc","content":[{"type":"paragraph","content":[{"type":"text","text":"N/A — pass-through receipt, no lien waiver required."}]}]}}

tasks = [
  # 1 — VALUE
  {"title": "VALUE: Confirm billing amount vs budget",
   "status": {"value": "in_progress"},
   "priority": "urgent" if is_over_budget else "normal",
   "s3dcc09bbd": "J0NLz",
   "s952ca3b56": [invoice_id],
   "se5f41aa17": [project_id],
   "description": build_value_description(invoice, now)},

  # 2 — WORK
  {"title": "WORK: Confirm work completed / super sign-off",
   "status": {"value": "in_progress"},
   "priority": "normal",
   "s3dcc09bbd": "J0NLz",
   "s952ca3b56": [invoice_id],
   "se5f41aa17": [project_id],
   "description": {"data":{"type":"doc","content":[{"type":"paragraph","content":[{"type":"text","text":f"[{now}] INIT — Awaiting PM / super confirmation that scope is installed as billed."}]}]}}},

  # 3 — DOCS: Drive link (auto-evaluated)
  {"title": "DOCS: Invoice PDF in Drive",
   "status": {"value": "complete" if has_drive else "in_progress"},
   "priority": "normal",
   "s3dcc09bbd": "NSH2u" if has_drive else "J0NLz",
   "s952ca3b56": [invoice_id],
   "se5f41aa17": [project_id],
   "description": {"data":{"type":"doc","content":[{"type":"paragraph","content":[{"type":"text","text":
     f"[{now}] INIT — {'Drive link present — auto-approved.' if has_drive else 'No Drive link found. PM to upload source invoice PDF and update s5fd21f93c.'}"}]}]}}},

  # 4 — DOCS: Package created
  {"title": "DOCS: Doc package created (G-5 + waiver)",
   "status": {"value": "rz4lv" if is_passthrough else "backlog"},
   "priority": "normal",
   "s3dcc09bbd": "" if is_passthrough else "J0NLz",
   "s952ca3b56": [invoice_id],
   "se5f41aa17": [project_id],
   "description": na_desc if is_passthrough else {"data":{"type":"doc","content":[{"type":"paragraph","content":[{"type":"text","text":f"[{now}] INIT — Blocked on Task 1 (VALUE approval). Run Phase C once value is approved."}]}]}}},

  # 5 — DOCS: Lien waiver sent for signature
  {"title": "DOCS: Lien waiver sent for signature",
   "status": {"value": "rz4lv" if is_passthrough else "backlog"},
   "priority": "normal",
   "s3dcc09bbd": "" if is_passthrough else "J0NLz",
   "s952ca3b56": [invoice_id],
   "se5f41aa17": [project_id],
   "description": na_desc if is_passthrough else {"data":{"type":"doc","content":[{"type":"paragraph","content":[{"type":"text","text":f"[{now}] INIT — Blocked on Task 1 (VALUE approval). Run Phase C once value is approved."}]}]}}},

  # 6 — DOCS: Signed waiver + application returned
  {"title": "DOCS: Signed waiver + application returned",
   "status": {"value": "rz4lv" if is_passthrough else "backlog"},
   "priority": "normal",
   "s3dcc09bbd": "" if is_passthrough else "J0NLz",
   "s952ca3b56": [invoice_id],
   "se5f41aa17": [project_id],
   "description": na_desc if is_passthrough else {"data":{"type":"doc","content":[{"type":"paragraph","content":[{"type":"text","text":f"[{now}] INIT — Blocked on Task 4 (package creation). Run Phase C once value is approved."}]}]}}},

  # 7 — DOCS: Filed to Drive + invoice linked
  {"title": "DOCS: Signed package filed to Drive + invoice linked",
   "status": {"value": "rz4lv" if is_passthrough else "backlog"},
   "priority": "normal",
   "s3dcc09bbd": "" if is_passthrough else "J0NLz",
   "s952ca3b56": [invoice_id],
   "se5f41aa17": [project_id],
   "description": na_desc if is_passthrough else {"data":{"type":"doc","content":[{"type":"paragraph","content":[{"type":"text","text":f"[{now}] INIT — Final step: signed PDF must be uploaded to Drive, Drive URL must replace or append to s5fd21f93c on the invoice record."}]}]}}},
]
```

### A.5 — VALUE task description (initial log entry)

```python
def build_value_description(invoice, now):
    gross = float(invoice.get("amount") or 0)
    scheduled = float((invoice.get("sa284e6b1e") or [[0]])[0][0] or 0)
    gross_to_date = float((invoice.get("sa92d2595d") or [[0]])[0][0] or 0)
    prev = gross_to_date - gross
    balance = float((invoice.get("s94ca7fec4") or [[0]])[0][0] or 0)
    budget_flag = invoice.get("s94d2830bf", "")
    memo = invoice.get("s83e0052b4", "")
    flags = [line for line in memo.split(" - ") if "FLAG" in line.upper()]
    flag_text = " | ".join(flags) if flags else "None"
    vendor_code = (invoice.get("s21078d2e9") or [[[""]]])[0][0][0]
    cost_code = (invoice.get("sbed918344") or [[""]])[0][0]
    inv_no = invoice.get("s3e3663e67") or invoice.get("invoice_name","")
    pa_match = re.search(r"PA#(\d+)", memo)
    pa_num = pa_match.group(1) if pa_match else "?"

    text = (
        f"[{now}] INIT — "
        f"Vendor: {vendor_code} | Invoice: {inv_no} | CC: {cost_code} | PA#{pa_num}\n"
        f"Gross this period: ${gross:,.2f} | Scheduled: ${scheduled:,.2f} | "
        f"Prev billed: ${prev:,.2f} | Gross to date: ${gross_to_date:,.2f} | "
        f"Balance: ${balance:,.2f}\n"
        f"Budget: {budget_flag}\n"
        f"FLAGS: {flag_text}\n"
        f"PM ACTION: Set rating 2-Approve or 0-Fail. Append decision note below."
    )
    return {"data":{"type":"doc","content":[{"type":"paragraph","content":[{"type":"text","text":text}]}]}}
```

---

## Phase B — PM Value Review

### B.1 — Read pending VALUE tasks

Search Check List Tasks: title = `"VALUE: Confirm billing amount vs budget"`,
status ≠ `complete`, project = current project. Sort: urgent first.

### B.2 — Present to PM (max 5 per exchange)

```
[URGENT] {Vendor} — CC {code} — Invoice #{inv_no}
  Gross: ${amount} | Scheduled: ${scheduled} | To date: ${gross_to_date}
  Balance: ${balance} | {‼️ OVER BUDGET / ⚠️ Budget available / ✓}
  FLAGS: {any FLAG text}
→ Approve / Conditional (with note) / Reject?
```

### B.3 — Update task + append log entry

**Approve:**
```python
update_fields = {
    "s3dcc09bbd": "NSH2u",
    "status": {"value": "complete"},
}
# Append: f"[{now}] VALUE APPROVED — PM confirmed amount acceptable. Rated 2-Approve."
```

**Conditional:**
```python
update_fields = {
    "s3dcc09bbd": "GLsXI",
    "status": {"value": "ready_for_review"},
}
# Append: f"[{now}] VALUE CONDITIONAL — PM note: {pm_note}. Rated 1-Conditional."
```

**Reject:**
```python
update_fields = {
    "s3dcc09bbd": "J0NLz",
    "status": {"value": "in_progress"},
}
# Append: f"[{now}] VALUE REJECTED — Reason: {pm_note}. Invoice held."
```

**To append to a rich-text description field** — fetch the existing task,
read `description.data.content`, append a new paragraph node, then update
the record with the merged content array.

---

## Phase C — Documentation Package Generation (CoWork)

This phase generates a 3-page PDF document package per invoice and deposits
it into a Drive subfolder inside the PA's Drive folder. The PM then reviews
and sends. Claude does not send the email directly — it creates the package
and gives the PM a ready-to-send draft.

### C.1 — Identify ready invoices

For each invoice in the PA, read `suxr9w93` tasks and find:
- Task 1 (`VALUE:...`) → rating = `NSH2u` or `GLsXI` (approved or conditional)
- Task 4 (`DOCS: Doc package created...`) → status = `backlog`

Both conditions → ready for doc generation.

### C.2 — Pull vendor contacts (dual-recipient logic)

1. `s0bcc3e24a[0]` → Companies record ID
2. Fetch Companies record, read:
   - `s4rct6xh[0]` → AR/AP Contact People record ID (email recipient — **To:**)
   - `s044084fbd[0]` → Authorized Signer People record ID (signs forms — **CC:** if different)
3. Fetch each People record:
   - `s70d9e051a[0]` = email
   - `s3d6f88a8c` = first name, `s1223b581d` = last name

**If AR/AP and Authorized Signer are the same person:** Send to one address,
no CC. No self-CC.

**If either field is empty:** Append BLOCKED note to Task 5 description:
```
[{now}] BLOCKED — {AR/AP contact OR authorized signer} missing from Companies record.
Manual delivery required. PM: populate s4rct6xh and s044084fbd on vendor record, then re-run Phase C.
```
Set Task 5 rating = `GLsXI`. Skip package generation for this invoice.

### C.3 — Pull G-703 data

Fetch `s0ef26247a[0]` (G-703 line item record):
- `s40aa4fa79` = scheduled value (original contract amount)
- `sf128c090d` = gross to date (all PAs cumulative)
- `s1dkc1mk` = balance to finish
- `s2dc89hk` = % complete

### C.4 — Compute form values

```python
gross         = float(invoice["amount"])
ret_pct       = float(invoice.get("sf74e1bbd4") or 0)
retention     = round(gross * (ret_pct / 100), 2)
net           = round(gross - retention, 2)
gross_to_date = float(g703.get("sf128c090d") or 0)
prev_billings = round(gross_to_date - gross, 2)
scheduled     = float(g703.get("s40aa4fa79") or 0)
balance       = float(g703.get("s1dkc1mk") or 0)
pct_complete  = float(g703.get("s2dc89hk") or 0)
pa_num        = re.search(r"PA#(\d+)", invoice["s83e0052b4"]).group(1)
cost_code     = invoice.get("sbed918344", [[""]])[0][0]
vendor_code   = invoice.get("s21078d2e9", [[[""]]])[0][0][0]
inv_no        = invoice.get("s3e3663e67") or invoice.get("invoice_name","")
pa_date       = pay_app.get("sd28f22e3a", {}).get("date", "")[:10]  # billing period through
```

### C.5 — Determine waiver type

| Condition | Waiver | NRS Reference | Form Label |
|---|---|---|---|
| **Default: outgoing package, payment not yet received** | **G-1 Conditional Progress** | **NRS 108.2457(5)(a)** | **CONDITIONAL** |
| PM escalates after confirming payment sent | G-2 Unconditional Progress | NRS 108.2457(5)(b) | UNCONDITIONAL |
| Final PA + 100% complete + releasing retention | G-3 Conditional Final | NRS 108.2457(5)(c) | |
| Post-payment final release | G-4 Unconditional Final | NRS 108.2457(5)(d) | |

**DEFAULT FOR ALL OUTGOING PHASE C PACKAGES = G-1 CONDITIONAL PROGRESS.**

The G-1 Conditional is the correct form to send to subs before payment has
been received. It becomes effective only upon the sub's receipt of the net
payment. The G-2 Unconditional is used AFTER payment is confirmed — PM
manually updates Task 5 description and re-generates if needed.

**Payment amount on waiver = net due this period (gross − retention).**

### C.6 — Generate 3-page PDF (reportlab)

Install: `pip install reportlab --break-system-packages`

The package is a single 3-page PDF:
- **Page 1:** Draft outreach email (plain text, formatted for PM copy/send)
- **Page 2:** G-5 Subcontractor Application for Payment (all SOV fields pre-filled)
- **Page 3:** G-1 Conditional Lien Waiver (NRS 108.2457(5)(a), all fields pre-filled, signature blocks blank)

**Filename convention:**
`PA{N}_{VendorCode}_{CostCode}_Inv{InvNo}_DocPackage.pdf`

Example: `PA7_NNM_HVAC_15800_Inv2100_DocPackage.pdf`

#### Page 1 — Draft Email

```
═══════════════════════════════════════════════════════════════
DRAFT EMAIL — For PM review and sending
PM: Copy this text, attach Pages 2-3 of this PDF, and send.
═══════════════════════════════════════════════════════════════

To:      {ar_ap_email} ({ar_ap_first} {ar_ap_last})
CC:      {signer_email} ({signer_first} {signer_last})   [omit if same as To:]
Subject: {project_name} — PA-{N} Lien Waiver & Application — {vendor_name}

Hi {ar_ap_first},

Please review and execute the attached documents for Pay Application #{N}:

  Project:                        {project_name}
  Cost code:                      {cost_code}
  Invoice #:                      {inv_no}
  Billing period through:         {pa_date}

  Gross billed this period:       ${gross:,.2f}
  Retention ({ret_pct}%):        (${retention:,.2f})
  Net due this period:            ${net:,.2f}
  % Complete to date:             {pct:.1f}%
  Balance to finish:              ${balance:,.2f}

Page 2: Subcontractor Application for Payment (G-5) — please confirm the
amounts are correct and sign/date where indicated.

Page 3: NRS 108.2457(5)(a) Conditional Progress Lien Waiver — please sign
and return to this email along with the signed G-5.

Note: This is a CONDITIONAL waiver. It becomes effective only upon your
receipt of the net payment amount of ${net:,.2f}. Do not sign if the amounts
are in dispute or if you have not agreed to the billing period amounts shown.

Please return signed documents to this email.

{pm_first} {pm_last} | BUILT. | (775) 737-3301
═══════════════════════════════════════════════════════════════
```

#### Page 2 — G-5 Subcontractor Application for Payment

Layout mirrors the uploaded NNM HVAC sample. Key fields pre-filled:

**Header block:**
- BUILT. logo (top left), letterhead address
- FROM: `{vendor_name}` / `{signer_first} {signer_last}` / `{signer_phone}` / `{signer_email}`
- Date: `{today}`
- Pay Request No.: `{pa_num}`
- Job Number: `WL-CONS` (or project job number)
- Project: `{project_name}`
- Invoice #: `{inv_no}`
- Billing Period Through: `{pa_date}`
- Cost Code: `{cost_code}`

**Schedule of Values Summary table:**
| Description | Amount |
|---|---|
| Original Contract Amount | `${scheduled:,.2f}` |
| Approved Change Orders | `—` (or CO detail if available) |
| Adjusted Contract Amount | `${scheduled:,.2f}` |
| Gross Amount Completed — Prior Applications | `${prev_billings:,.2f}` |
| Current Gross Billing (this period) | `${gross:,.2f}` |
| Gross Amount Completed to Date | `${gross_to_date:,.2f}` |
| Less {ret_pct}% Retainage — This Period | `(${retention:,.2f})` |
| Current Net Billing (this period) | `${net:,.2f}` |
| % Complete to Date | `{pct:.1f}%` |
| Balance to Finish (excluding retention held) | `${balance:,.2f}` |

**Signature block (blank for sub):**
- Company Name: `{vendor_name}` (pre-printed)
- Authorized Signature: `_______________________`
- Title: `_______________________`
- Date: `_______________________`

#### Page 3 — G-1 Conditional Progress Lien Waiver

**Header:** `G-1` (top right), `NRS 108.2457(5)(a) CONDITIONAL WAIVER AND RELEASE UPON PROGRESS PAYMENT`

**Pre-filled fields:**
- Property Name: `{project_name}`
- Property Location: `{project_address}` (e.g., "677 South Virginia Street, Reno, NV 89501")
- Undersigned's Customer: `KCS Homes LLC dba BUILT.`
- Invoice / Payment Application No.: `PA-{N} / Invoice {inv_no}`
- Payment Amount: `${net:,.2f}` (net due this period — gross minus retention)

**Waiver body text (NRS 108.2457(5)(a)):**
```
The undersigned has been paid a progress payment, or has received a promise of
payment in the above-referenced Payment Amount for all work, materials and
equipment the undersigned furnished to the Customer for the above-described
Property and, upon receipt of the Payment Amount, does hereby waive and release
any notice of lien, any private bond right, any claim for payment and any rights
under any similar ordinance, rule or statute related to payment rights that the
undersigned has on the above-described Property to the following extent:

This release covers a progress payment for the work, materials and equipment
furnished by the undersigned to the Property or to the Undersigned's Customer
which are the subject of the Invoice or Payment Application, but only to the
extent of the Payment Amount or such portion of the Payment Amount as the
undersigned is actually paid, and does not cover any retention withheld, any
items, modifications or changes pending approval, disputed items and claims, or
items furnished that are not paid. The undersigned warrants that he or she either
has already paid or will use the money received from this progress payment
promptly to pay in full all laborers, subcontractors, materialmen and suppliers
for all work, materials or equipment that are the subject of this waiver and
release.
```

**Signature block (blank for sub):**
- Dated: `_______________________`
- Company Name: `{vendor_name}` (pre-printed)
- By (Authorized Signature): `_______________________`
- Date Signed: `_______________________`
- Its (Title): `_______________________`

**Notice footer:**
```
Notice: This document waives rights conditionally and states that you have been
paid or that you have received a promise of payment in exchange for releasing
those rights. This document is enforceable against you if you sign it even if
you have not been paid. If you have not been paid, use a conditional release
form carefully.
```

### C.7 — Drive folder deposit

The generated PDF is uploaded to a subfolder inside the PA's Drive folder.

**PA-level Drive folder:** Retrieved from the Pay App record's G-Drive Link field,
or use the known PA-7 folder ID: `1rhpverx411mpwBmisURTj00Zx6uDnU9E`.

**Subfolder name:** `Invoice Doc Packages` (create once per PA if it doesn't exist).

**File placement:** `Invoice Doc Packages/{filename}.pdf`

After upload:
1. Get the Drive file URL
2. Update `s5fd21f93c` on the invoice record with the package URL
3. Update Task 4:

```python
# Task 4 — DOCS: Doc package created
update_fields = {
    "s3dcc09bbd": "NSH2u",
    "status": {"value": "complete"},
}
# Append to description:
f"[{now}] PDF CREATED — {filename} generated (3 pages: email draft + G-5 + NRS G-1 Conditional). Filed to Drive: {package_url}. PM: review Pages 2-3, attach to email on Page 1, send."
```

### C.8 — Update Task 5 when PM confirms email sent

PM confirms: "sent the waiver to [vendor]" or manually updates Task 5.

Claude updates Task 5:
```python
update_fields = {
    "s3dcc09bbd": "GLsXI",     # Conditional — in flight
    "status": {"value": "in_progress"},
}
# Append:
f"[{now}] SENT — Waiver package sent to {ar_ap_email} ({ar_ap_name}, AP/AR) + {signer_email} ({signer_name}, Authorized Signer). Subject: '{subject}'. Awaiting signed return."
```

If only one recipient: log that person.

---

## Phase D — Return and Filing

### D.1 — PM confirms signed docs received

PM says "signed docs back from {vendor}" or marks Task 6 manually.

Update Task 6:
```python
update_fields = {
    "s3dcc09bbd": "NSH2u",
    "status": {"value": "complete"},
}
# Append:
f"[{now}] RETURNED — Signed package received from {ar_ap_email}."
```

### D.2 — File signed PDF to Drive + update invoice

PM uploads the signed combined PDF to the project Drive folder (same
`Invoice Doc Packages` subfolder, or the `2 - PM Approved` subfolder
for the Pay App). Claude then:

1. Takes the Drive URL from PM
2. Updates `s5fd21f93c` on the invoice with the signed package URL
3. Updates Task 7:

```python
# Update invoice
smartsuite_update_record(
    app_id="68a8c3d2bba73ca6e62d1297",
    record_id=invoice_id,
    fields={"s5fd21f93c": [signed_drive_url]}
)

# Task 7
update_fields = {
    "s3dcc09bbd": "NSH2u",
    "status": {"value": "complete"},
}
# Append:
f"[{now}] FILED — Signed PDF filed to Drive. URL: {signed_drive_url}. Invoice s5fd21f93c updated. Audit complete for this invoice."
```

### D.3 — Invoice is fully audited

When all 7 tasks are `NSH2u` / `complete` (and N/A tasks are `rz4lv`):
- Value confirmed by PM
- Work confirmed by PM / super
- Source invoice in Drive
- G-5 + lien waiver package created and deposited to Drive
- Waiver sent to vendor (both AR/AP and Authorized Signer)
- Signed package returned
- Signed package filed and linked on invoice record

---

## Phase E — Audit Status Report

On demand: "where does PA-7 stand?" or at session end.

Read all invoice IDs from PA `s5bea81732`. For each non-skipped invoice,
read `suxr9w93` tasks and evaluate states.

### Status buckets

| Bucket | Criteria |
|---|---|
| ✅ Fully complete | All 7 tasks `NSH2u`/`complete` (N/A tasks = `rz4lv`) |
| 📁 Filed, pending PM lock review | Task 7 complete, Task 1 or 2 still conditional |
| 📤 Sent, awaiting return | Task 5 `in_progress`, Task 6 `backlog` |
| 📄 Package created, not yet sent | Task 4 complete, Task 5 `backlog` |
| 🟡 Value approved, docs not started | Task 1 `NSH2u`, Task 4 `backlog` |
| 🔴 Value not yet approved | Task 1 rating = `J0NLz` |
| 🔵 Pass-through (no waiver needed) | Tasks 4–7 all `rz4lv` |
| ⛔ Flagged / no contact | Task 5 description contains "BLOCKED" |
| 📋 Not yet initialized | `suxr9w93` empty |

### Report format

```
PA-7 Audit Status — 677 Virginia Restaurant TI     {date}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ Fully complete:                    N    $X,XXX.XX
📁 Filed, awaiting final PM lock:     N    $X,XXX.XX
📤 Sent / awaiting signed return:     N    $X,XXX.XX  [vendor list]
📄 Package created, not yet sent:     N    $X,XXX.XX  [vendor list]
🟡 Value approved / docs not started: N    $X,XXX.XX  ← run Phase C
🔴 Value approval pending:            N    $X,XXX.XX  ← PM action needed
🔵 Pass-through (no waiver):          N    $X,XXX.XX
⛔ Blocked (no contact):              N               [vendor list]
📋 Not yet initialized:               N

PA-7 Total:                          $X,XXX.XX
Value approved & documented:         $X,XXX.XX
Pending PM value decision:           $X,XXX.XX
In-flight (sent, not returned):      $X,XXX.XX
```

---

## Idempotency Rules

1. **Task dedup:** check `suxr9w93` before creating. Exact title match = skip.
2. **GC alloc check:** `s3fdf4e04c` = "Remove for Export" → skip entirely, do not create or modify any tasks.
3. **PDF dedup:** Task 4 status ≠ `backlog` → package already created. Confirm before regenerating.
4. **Send dedup:** Task 5 status ≠ `backlog` → already sent or in flight. Confirm before resending.
5. **Filing dedup:** Task 7 complete → already filed. Confirm before overwriting Drive link.
6. **Re-run safe:** second run on same PA produces zero new records if all tasks exist per invoice.

---

## Error Handling

| Condition | Action |
|---|---|
| No G-703 (`s0ef26247a` empty) | FLAG in Task 1 description. Priority = urgent. Skip Phase C. |
| No AR/AP contact on vendor | BLOCKED note on Task 5. Set `GLsXI`. Skip package generation. |
| No Authorized Signer on vendor | BLOCKED note on Task 5. Set `GLsXI`. Skip package generation. |
| No email on People record | Same as above. |
| AR/AP and Signer are same person | Send to one address only — no self-CC. Log both names. |
| Split parent (`sad3605fad` non-empty) | Skip entirely — children are auditable units. |
| Rate limit 429 | Wait 3s, retry once. Log in task description, continue. |
| Amount = 0 | Skip. |
| Task 7 complete but `s5fd21f93c` still shows old URL | Flag discrepancy. Re-run D.2. |
| Drive upload fails | Log error in Task 4 description. Retry or flag for manual upload. |

---

## Field Reference

```
Check List Tasks:   68a8e17251dc814e8c529f3f
  s952ca3b56        Linked to invoice  ← parent link
  se5f41aa17        Link to Project
  sz08dosv          Link to Check Lists ← leave EMPTY in this flow
  status            backlog / in_progress / ready_for_review / complete / rz4lv (N/A)
  priority          urgent / high / normal / low
  s3dcc09bbd        NSH2u=Approve  GLsXI=Conditional  J0NLz=Fail  ""=unrated (N/A tasks)
  description       rich text — append-only audit log

Bills & Invoices:   68a8c3d2bba73ca6e62d1297
  suxr9w93          Tasks backlink — idempotency check
  s5fd21f93c        Drive URL — updated at Task 4 (package) and Task 7 (signed)
  s3fdf4e04c        "Remove for Export" → skip entirely (GC allocation)
  sad3605fad        Children → skip if non-empty (split parent)
  s94d2830bf        Budget flag → "OVER BUDGET" = urgent Task 1
  s21078d2e9        Vendor code → KCSH001 = pass-through check
  sa284e6b1e        Scheduled value (lookup)
  sa92d2595d        Gross to date (lookup)
  s94ca7fec4        Balance to finish (lookup)
  s3e3663e67        Invoice number (for filename and forms)
  sf74e1bbd4        Retention % (whole number)

Companies/Entities: 68216a706900e8eaf75a05c0
  s044084fbd        Authorized Signer → People (signs G-5 + waiver)
  s4rct6xh          Accounting AP/AR Contact → People (receives email)

People:             68216a706900e8eaf75a05af
  s70d9e051a        Email
  s3d6f88a8c        First name
  s1223b581d        Last name

Pay Apps:           68db724638a208d3257ea3a9
  s5bea81732        Invoice ID array → skill entry point
  sd28f22e3a        PA date (billing period through)

G-703 Line Items:   68db71a363e88ace0bd45439
  s40aa4fa79        Scheduled value
  sf128c090d        Gross to date
  s1dkc1mk          Balance to finish
  s2dc89hk          % complete
```

---

## Live S-BOS Correction Note (v1.2 → v1.3 migration)

In the v1.2 live deployment (PA-7, 677 Virginia), the following errors occurred
in task initialization that were corrected manually before v1.3 release:

1. **GC allocations had tasks created then incorrectly marked complete.** 78 tasks
   across 26 invoices were corrected to `rz4lv` (N/A). Going forward, GC allocs
   receive ZERO tasks at initialization.

2. **Pass-through receipts (DOCS tasks 4-7) were set to `complete`/`NSH2u`.**
   Corrected to `rz4lv`. Going forward, pass-throughs get `rz4lv` at creation.

3. **Task titles 5 and 6 in the skill did not match live S-BOS.** The titles in
   the live database are authoritative:
   - Task 5 (live): `DOCS: Lien waiver sent for signature`
   - Task 6 (live): `DOCS: Signed waiver + application returned`
   These are now the canonical titles in v1.3.

4. **Invoice PDF in Drive (Task 3) was incorrectly set to `complete`/`NSH2u`
   for GC alloc invoices.** Fixed in the N/A correction sweep. GC allocs get no
   tasks at all going forward.

The v1.3 skill correctly handles all three invoice types from initialization.

---

## Existing Task Cleanup Note (v1.1 → v1.2 migration)

Prior sessions created single-task records with non-standard titles like
"PA-7 Audit: 08200 Overhead Door Co. #57623 — $7,500 | VALUE FLAG + RETENTION FLAG".
These are legacy ad-hoc tasks. On first run of v1.2+:

1. Read `suxr9w93` on each invoice
2. If tasks found have non-standard titles (not matching the 7 exact strings)
   → these are legacy tasks. Leave them in place (they contain useful audit notes).
   Create the 7 standard tasks alongside them.
3. Report: "Found N legacy audit tasks. Preserved as notes. Created 7 standard tasks per invoice."
4. Optional: update legacy task titles to prefix with `[LEGACY] ` so they sort separately.
