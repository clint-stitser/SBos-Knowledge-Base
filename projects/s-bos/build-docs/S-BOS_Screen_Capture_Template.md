# S-BOS Screen Capture Template

> **Purpose:** Walk the system screen by screen and describe what each page *currently does* — the fastest, most grounded way to fill **PDD §4 Core Features** (and feed §5 User Workflows + the Access model). Clint narrates in plain English; Claude translates each capture into precise §4 entries with success criteria.
> **Method:** back-fill from the running app. Mark facts as observed. The **New features / automations / AI** block is where the future gets designed — it's the Kompass assistant + Feed scoped to each page.
> **Related:** `S-BOS_Product_Design_Doc.md` §4/§5/§6/§9 · the System Atlas (`sb-crm-poc/docs/atlas/`) · the Softr Registry.

---

## Per-screen template (copy one block per page)

```
### <Screen name>  (URL / route)

**What it does today:**
(plain English — the functions this page performs)

**Data shown:**
(key fields, columns, tabs, sections)

**Reads / Writes:**
(which records it pulls from / updates — People, Companies, Project, Tasks, Budget, Notes, GYR, …)

**Actions:**
(buttons / things a user can do — create, link, complete, upload, print, approve, …)

**Who uses it:**
(Customer · Investor · Vendor · Production Staff · Production Manager · Dept Head · Senior Management · Admin)

**Keep / Change / Drop:**
(migration verdict + what should change on the new stack)

**Phase:** 1 or 2

➕ **New features / automations / AI integrations**
- New features: (what you wish this page did that it doesn't)
- Automations: (when X → do Y — triggers: status change, date reached, record created, invoice added → actions: notify, create task, update field, generate doc)
- AI integrations: (what the assistant should do here — search / draft / summarize / decide-support / auto-fill from blueprint / check compliance)
- Example trigger: (one concrete line — e.g. "'Renew GL Insurance' → assistant searches Gmail by vendor → reports status → offers to draft the email")
```

---

## Field guide

| Field | What to write |
|---|---|
| **What it does today** | The page's job, in plain language — no need for technical terms. |
| **Data shown** | The visible fields/columns/tabs; what a user reads here. |
| **Reads / Writes** | Which entities/tables it displays vs. changes. |
| **Actions** | Every button/interaction (create, link, complete, upload, print, approve…). |
| **Who uses it** | The group(s) that need it — drives the Access model (RLS). |
| **Keep / Change / Drop** | Since we're rebuilding: keep as-is, keep-but-change (say how), or drop. |
| **Phase** | 1 (People/Companies/Project execution) or 2 (Brokerage, Credit Desk). |
| **New features / automations / AI** | The future. Verbs for AI: **search · draft · summarize · decide-support · auto-fill · check.** Automations = trigger → action. |

---

## Phase-1 pages to walk (tracker)

| Screen | URL | Captured? |
|---|---|---|
| Launch Pad | `/` | ☐ |
| CRM Home — Projects tab | `/sb-crm-home` | ☐ |
| CRM Home — People tab | `/sb-crm-home` | ☐ |
| CRM Home — Companies tab | `/sb-crm-home` | ☐ |
| CRM Home — Formation Projects tab | `/sb-crm-home` | ☐ |
| Person detail | (from People) | ☐ |
| Company detail | (from Companies) | ☐ |
| Project detail — header + Details | `/sb-crm-projects-list-details` | ☐ |
| Project — Decisions/Ratings | (tab) | ☐ |
| Project — Reporting/Planning | (tab) | ☐ |
| Project — Team | (tab) | ☐ |
| Project — Project Drive | (tab) | ☐ |
| Project — Schedule | (tab) | ☐ |
| Project — Tasks/Checklists | (tab) | ☐ |
| Project — Budget(s) & Pay App(s) | (tab) | ☐ |
| Project — Project CRM | (tab) | ☐ |
| The Game | `/the-game-homepage` | ☐ |
| My Responsibilities | `/sb-my-responsibilities` | ☐ |
| Account / Authority Pyramid | `/sb-crm-home` (tab) | ☐ |
| Time Card | `/sb-time-card-form` | ☐ |
| Accounting (cross-cutting, light) | `/sb-mgt-accounting` | ☐ |
| Knowledge Base (cross-cutting, light) | `/knowledge-base` | ☐ |
| Claude's Activity (cross-cutting, light) | `/claude-s-activity-log-in-s-bos` | ☐ |

**Phase 2 (skip for now):** Stitser Properties / brokerage screens (`/sp-home`, contract stages), Credit Desk (`/sb-credit-desk`).

---

*Claude has pre-drafted the Phase-1 screens in PDD §4 from the live app (marked 🔎, with a proposed AI/automation line each). Clint's captures using this template layer on top — Claude keeps §4 in sync.*
