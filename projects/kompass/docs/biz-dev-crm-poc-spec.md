# Biz Dev CRM — POC Migration Spec
> **Last Updated:** 2026-06-09  
> **Purpose:** Complete specification for rebuilding the Biz Dev CRM module from SmartSuite + Softr onto Supabase + Cloudflare Pages. Users must notice zero change.

---

## 1. Stack Decision

| Layer | Current | New POC |
|---|---|---|
| Database | SmartSuite (SB-Biz Dev + SB Project MGT solutions) | Supabase (Postgres) |
| Schema/formula mgmt | SmartSuite no-code UI | Claude via SQL (migrations, computed columns, triggers) |
| User portal | Softr at `app.stitserbuilt.com` | Cloudflare Pages (Next.js) at same domain |
| Auth | Softr auth | Cloudflare Access (Google OAuth / magic link) |
| API / dashboards | Railway | Railway (unchanged) |
| Claude data access | SmartSuite MCP | Supabase MCP |
| Admin layer | N/A | `/admin/*` routes in Cloudflare Pages |

**Critical constraint:** Users must not know about the change. Same URL, same login experience, pixel-perfect UI match.

---

## 2. Supabase Schema — 9 Tables

### 2.1 people
```sql
CREATE TABLE people (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  full_name     text NOT NULL,
  title         text,
  email         text,
  phone         text,
  photo_url     text,
  company_id    uuid REFERENCES companies(id),
  primary_sb_contact text,           -- Clint/Kurt/Conrad
  sb_collaborators   text[],
  sb_audience_segment text[],        -- multi-select pills
  referred_by   text,
  depth_of_relationship smallint CHECK (depth_of_relationship BETWEEN 1 AND 5),
  crm_tags      text[],
  event_invites_tag text[],
  newspaper_recipient text,          -- PDF/Printed/null
  fub_link      text,
  date_of_recent_interaction timestamptz,
  created_at    timestamptz DEFAULT now(),
  updated_at    timestamptz DEFAULT now()
);
```

### 2.2 companies
```sql
CREATE TABLE companies (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  company_name  text NOT NULL,
  sector        text[],              -- multi-select tags
  website       text,
  street_address text,
  city          text,
  state         text,
  zip           text,
  vendor_id     text,
  intacct_location_id text,
  intacct_vendor  text,
  intacct_customer_id text,
  created_at    timestamptz DEFAULT now(),
  updated_at    timestamptz DEFAULT now()
);
```

### 2.3 projects
```sql
CREATE TABLE projects (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  project_name  text NOT NULL,
  company_id    uuid REFERENCES companies(id),
  project_type  text,
  status        text,                -- Active in Closeout, etc.
  priority      text,                -- High / Medium / Low
  department    text,
  property_record_id uuid,
  kurts_categories text[],
  cover_photo_url text,
  created_at    timestamptz DEFAULT now(),
  updated_at    timestamptz DEFAULT now()
);
```

### 2.4 notes_comments
```sql
CREATE TABLE notes_comments (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id    uuid REFERENCES projects(id),
  person_id     uuid REFERENCES people(id),
  company_id    uuid REFERENCES companies(id),
  title         text,
  body          text,
  status_line   text,                -- bold status shown in comment card
  source        text,                -- attribution
  created_by    text,
  created_at    timestamptz DEFAULT now(),
  updated_at    timestamptz DEFAULT now()
);
```

### 2.5 stakeholder_bridge  
*(People ↔ Projects many-to-many with role/stage)*
```sql
CREATE TABLE stakeholder_bridge (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id    uuid REFERENCES projects(id),
  person_id     uuid REFERENCES people(id),
  stage         text,
  role          text,
  created_at    timestamptz DEFAULT now()
);
```

### 2.6 project_budget_items
```sql
CREATE TABLE project_budget_items (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id    uuid REFERENCES projects(id),
  company_id    uuid REFERENCES companies(id),
  title         text,
  account_code  text,
  cash_flow_section text,
  cash_flow_direction text,          -- In / Out
  estimated_budget numeric(14,2),
  baseline_budget  numeric(14,2),
  change_orders    numeric(14,2) DEFAULT 0,
  created_at    timestamptz DEFAULT now(),
  updated_at    timestamptz DEFAULT now()
);
```

### 2.7 project_dates
```sql
CREATE TABLE project_dates (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id    uuid REFERENCES projects(id),
  label         text,                -- e.g. "LOI Signed", "Close of Escrow"
  date_value    date,
  notes         text,
  created_at    timestamptz DEFAULT now()
);
```

### 2.8 check_lists
```sql
CREATE TABLE check_lists (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  project_id    uuid REFERENCES projects(id),
  loan_id       uuid,                -- future FK
  person_id     uuid REFERENCES people(id),
  title         text,
  linked_decision_gate text,
  final_decision text,
  created_at    timestamptz DEFAULT now(),
  updated_at    timestamptz DEFAULT now()
);
```

### 2.9 check_list_tasks
```sql
CREATE TABLE check_list_tasks (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  checklist_id  uuid REFERENCES check_lists(id),
  project_id    uuid REFERENCES projects(id),
  what_to_do    text NOT NULL,
  phase         text,
  status        text DEFAULT 'Backlog', -- Backlog / In Progress / Complete
  due_date      date,
  assigned_to   text,
  created_at    timestamptz DEFAULT now(),
  updated_at    timestamptz DEFAULT now()
);
```

### Indexes & Triggers
```sql
-- updated_at auto-stamp (apply to all tables)
CREATE OR REPLACE FUNCTION set_updated_at()
RETURNS TRIGGER AS $$ BEGIN NEW.updated_at = now(); RETURN NEW; END; $$ LANGUAGE plpgsql;

-- Apply to each mutable table:
CREATE TRIGGER trg_people_updated BEFORE UPDATE ON people FOR EACH ROW EXECUTE FUNCTION set_updated_at();
-- (repeat for companies, projects, notes_comments, project_budget_items, check_lists, check_list_tasks)

-- Performance indexes
CREATE INDEX idx_projects_company ON projects(company_id);
CREATE INDEX idx_stakeholder_project ON stakeholder_bridge(project_id);
CREATE INDEX idx_stakeholder_person ON stakeholder_bridge(person_id);
CREATE INDEX idx_budget_project ON project_budget_items(project_id);
CREATE INDEX idx_tasks_checklist ON check_list_tasks(checklist_id);
CREATE INDEX idx_notes_project ON notes_comments(project_id);
```

---

## 3. Cloudflare Pages — Next.js Route Structure

```
/
├── portal/
│   ├── sb-crm-home              ← replaces app.stitserbuilt.com/sb-crm-home
│   │   ├── #tab1 (Projects)
│   │   ├── #tab2 (People)
│   │   └── #tab3 (Companies)
│   └── sb-crm-projects-list-details
│       └── ?recordId=...        ← Project detail page (8 tabs)
└── admin/
    ├── schema                   ← View/edit table schemas
    ├── data                     ← Raw table browser
    ├── automations              ← Trigger management
    └── users                    ← Cloudflare Access user list
```

Auth: Cloudflare Access sits in front of `/portal/*` — Google OAuth or magic link email. `/admin/*` is restricted to clint@stitserbuilt.com only.

---

## 4. Pixel-Perfect UI Specification

### 4.1 Design System

| Token | Value |
|---|---|
| Primary / active color | `#1a6b6b` (dark teal) |
| Action button | teal bg, white text, rounded |
| Special action (link/create) | `#F97316` orange |
| Sidebar width | ~230px, fixed |
| Sidebar bg | white with left-border active indicator |
| Logo | Stitser Built top-left |
| Font | Inter (or system-ui fallback) |
| Content bg | white |
| Table row hover | show "Open" button + "..." overflow |

**Status badge colors:**
- Active/In Progress → blue pill
- Backlog → dark charcoal pill
- Complete → teal pill  
- On Track → green pill
- High priority → orange pill

**Tag pill colors (Title/Type on People):**
- HNW Individual Investor → purple/dark
- Commercial Broker → blue
- Developer → dark teal
- Facilities & Construction → teal
- Sales and/or Leasing → green
- Org/Co Executive → dark gray
- Sub-Contractor → dark gray/charcoal

### 4.2 Projects List (`/portal/sb-crm-home#tab1`)
- Page title: "Projects List" + "Use Filters to Work"
- Search bar (full width, light border)
- Filter dropdowns: SB Company / Project Type / Status / Priority / Kurt's Categories / Last Updated
- Right-side buttons: "Ask AI" (teal, ✦ icon) | "New Project" (teal)
- Table columns: SB Company | Project | Project Type | Status (badge) | Kurt's Categories | Last Updated | Attachments | Priority (badge) | ⋯

### 4.3 Project Detail (`/portal/sb-crm-projects-list-details?recordId=...`)
- Full-page layout (not modal/slide-out)
- Cover photo area at top (placeholder if empty)
- Project name as h1
- **8 horizontal tabs** with underline active indicator:
  1. **Details** — key fields grid: SB Company, Status, Priority, Department, Property Record, Project Type; orange action buttons; "Links & Biz Dev Dates" section
  2. **Decisions/Ratings** — checklist section (Linked Person / Decision Gate / Final Decision filters + "Add Decision Gate"); "All Project Comments" rich cards (title, bold status line, body, source)
  3. **Reporting/Planning** — "Choose Priorities Template" button; card grid (image, title, green badge, date)
  4. **Team** — table (People/Stage/Role/Company filters); "Add Person to Project" + "Add Person to CRM" buttons
  5. **Tasks/Checklists** — upper: Check Lists (filters); lower: task table (What to Do / Phases / Status / When Due / Who) + "Add Task" + "Quick Add Blank" buttons
  6. **Budget(s) & Pay App(s)** — Pay Apps section (empty + "Add 702"); "Key Financial Stats" table (Title/SB Company/Account Code/Cash Flow Section/Direction/Estimated/Baseline/Change Orders)
  7. **Project CRM** — two stat cards side-by-side (DEBT FACILITY blue border, EQUITY RAISE green border; 0% Committed / Target / Raised / Pipeline); Pipeline section with search + filters

### 4.4 People List (`/portal/sb-crm-home#tab2`)
- Page title: "People List" + "Use Filters to Work"
- Filter dropdowns: Audience Segment / Primary SB Contact / CRM Tags / Event Invites-Tag / Newspaper Recipient
- Right: "Ask AI" | "New Contact" (teal)
- Table columns: Name | Primary SB Contact | Title/Type (colored multi-select pills) | Date of Recent Interaction | SB Audience Segment | Company | CRM Tags | Mail Address

### 4.5 People Detail (right slide-out panel)
- Opens as right-side drawer (NOT modal, NOT new page)
- Top: photo placeholder circle, name (h2), title tag pills, phone, email, company name, FUB link
- Two-column grid of fields: Primary SB Contact | SB Collaborators | SB Audience Segment | Referred/Introduced By | Depth of Relationship (⭐ star rating, 1–5) | CRM Tags | Date of Recent Interaction | Last Updated | Event Invites-Tag
- Section: "Linked Projects" list
- Edit / Delete action buttons at bottom

### 4.6 Companies List (`/portal/sb-crm-home#tab3`)
- Page title: "Companies List" + "Use Filters to Work"
- Filter dropdown: People
- Right: "Ask AI" | "New Company" (teal)
- Table columns: Company Name | All People (avatar chips) | Projects | Last Updated | Sector (tags) | Attachments | Vendor ID | Link to Intacct Location

### 4.7 Company Detail (center modal overlay)
- Opens as center modal (NOT slide-out)
- Left panel (scrollable): photo placeholder, company name, sector tag(s), All People links, Link to People 1, Last Updated, Website (clickable), Street Address, Address-City, Address-State, Address-Zip, Intacct Location ID, Intacct Vendor, Intacct Customer ID
- Center panel: Search + "New Comment/Note" button, comment feed (or empty state)
- Right panel: "Related Projects" with Status filter, project list rows
- Edit + Delete buttons at bottom of left panel

---

## 5. POC Build Phases

### Phase 1 — Supabase Setup (Week 1)
- [ ] Create Supabase project
- [ ] Run schema migrations (9 tables above)
- [ ] Seed with existing SmartSuite data (pull via MCP, insert via Supabase MCP)
- [ ] Set up Row Level Security (RLS) policies
- [ ] Install Supabase MCP for Claude access

### Phase 2 — Cloudflare Pages + Auth (Week 1–2)
- [ ] Create Next.js app, deploy to Cloudflare Pages
- [ ] Configure custom domain: `app.stitserbuilt.com` → Cloudflare Pages
- [ ] Set up Cloudflare Access: Google OAuth for all `/portal/*`, restrict `/admin/*` to clint@stitserbuilt.com
- [ ] Implement session/JWT passthrough to Supabase RLS

### Phase 3 — Portal UI (Week 2–3)
- [ ] Projects list + filters
- [ ] Project detail page (8 tabs, all sections)
- [ ] People list + slide-out drawer
- [ ] Companies list + center modal
- [ ] Design system components (badges, pills, tables, buttons)

### Phase 4 — Admin Layer (Week 3–4)
- [ ] `/admin/data` — table browser (read/write)
- [ ] `/admin/schema` — view current schema, run migrations
- [ ] `/admin/automations` — list/trigger Supabase Edge Functions

### Phase 5 — Validation (Week 4)
- [ ] Side-by-side comparison with live Softr
- [ ] User acceptance: team uses new portal, cannot tell difference
- [ ] Performance baseline (page load, query times)
- [ ] Cutover plan: DNS swap, redirect old Softr URLs

---

## 6. Sidebar Navigation (All Pages)

```
[Stitser Built Logo]
─────────────────────
Launch pad
Projects-People-Companies  ← active section (teal left border)
  Dashboards ▾
My Items
Internal Goals & Targets
Time Card
Accounting
Training & Education
SB-Credit Desk
Claude's Activity
─────────────────────
[Avatar] Clint Stitser
         clint@stitserbuilt.com  [▼]
```

---

## 7. SmartSuite → Supabase Field Mapping

| SmartSuite Field Type | Postgres Equivalent |
|---|---|
| Text | `text` |
| Number | `numeric` |
| Date | `date` / `timestamptz` |
| Single Select | `text` with CHECK constraint |
| Multi Select | `text[]` with GIN index |
| Linked Record | `uuid` FK |
| Formula (computed) | `GENERATED ALWAYS AS` or DB function |
| Rating (1–5) | `smallint CHECK (val BETWEEN 1 AND 5)` |
| File/Attachment | `text` (Supabase Storage URL) |
| User/Member | `text` (name) or FK to `users` table |
| URL | `text` |
| Phone | `text` |
| Email | `text` |
| Rich Text | `text` (HTML/Markdown) |

---

## 8. Open Questions / Decisions

1. **Data migration timing:** One-time seed from SmartSuite → Supabase, or live sync during parallel run?
2. **SmartSuite automations:** 17 automations in SB-Biz Dev. Need to audit and replicate as Supabase Edge Functions or Postgres triggers before cutover.
3. **Railway dashboards:** Currently iframed into Softr with user permissions. Same iframe pattern works in Cloudflare Pages — just embed in the relevant tab.
4. **Search:** Softr uses SmartSuite's built-in search. Supabase full-text search (`tsvector`) or Postgres `ILIKE` for MVP.
5. **File attachments:** SmartSuite stores files. Migration path: copy to Supabase Storage, update URLs.
