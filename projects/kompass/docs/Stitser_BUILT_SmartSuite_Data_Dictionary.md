# Stitser BUILT SmartSuite Data Dictionary

**Last Updated:** 2026-05-28 | **Source:** Live SmartSuite API via MCP | **Maintained by:** S-BOS System Admin

---

## Table of Contents

1. [SYSTEM](#system)
2. [SB UW & Estimating (PIPE)](#sb-uw--estimating-pipe)
3. [Master Property Data](#master-property-data)
4. [Stitser BUILT: SmartSuite Product Roadmap](#stitser-built-smartsuite-product-roadmap)
5. [Stitser Properties Production](#stitser-properties-production)
6. [SB-Biz Dev](#sb-biz-dev)
7. [The SB Game App](#the-sb-game-app)
8. [Stitser BUILT. Budget & Spend MGT](#stitser-built-budget--spend-mgt)
9. [SB Project MGT (WIP)](#sb-project-mgt-wip)
10. [Employee IT Asset Management](#employee-it-asset-management)
11. [SB Training & Certifications](#sb-training--certifications)
12. [The Stitser Way](#the-stitser-way)
13. [System Creation and Management](#system-creation-and-management)
14. [Financial Model for Cost Spreading](#financial-model-for-cost-spreading)
15. [System Libraries/Catalogs](#system-librariescatalogs)
16. [SB HR](#sb-hr)

---

## SYSTEM

**Solution ID:** `678c6f7947e0ad61546f4a56`

### Teams

**App ID:** `678c6f7947e0ad61546f4a57` | **Fields:** 11

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `name` | Name | recordtitlefield | Primary field |
| 2 | `color` | Color | colorpickerfield | |
| 3 | `type` | Type | singleselectfield | |
| 4 | `provisioning` | Provisioning | textfield | |
| 5 | `external_id` | External ID | textfield | |
| 6 | `status` | Status | statusfield | |
| 7 | `owners` | Owners | userfield | |
| 8 | `members` | Members | userfield | |
| 9 | `first_created` | First Created | datefield | |
| 10 | `last_updated` | Last Updated | datefield | |
| 11 | `email` | Email | emailfield | |

### Members

**App ID:** `678c6f7947e0ad61546f4c3c` | **Fields:** 38

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `full_name` | Full Name | fullnamefield | Primary field |
| 2 | `company_name` | Company Name | textfield | |
| 3 | `department` | Department | textfield | |
| 4 | `about_me` | About Me | textareafield | |
| 5 | `job_title` | Job Title | textfield | |
| 6 | `email` | Email | emailfield | |
| 7 | `status` | Status | statusfield | |
| 8 | `type` | Type | singleselectfield | |
| 9 | `provisioning` | Provisioning | textfield | |
| 10 | `external_id` | External ID | textfield | |
| 11 | `manager_id` | Manager | linkedrecordfield | |
| 12 | `role` | Role | textfield | |
| 13 | `locale` | Locale | textfield | |
| 14 | `timezone` | Timezone | textfield | |
| 15 | `language` | Language | textfield | |
| 16 | `office_location` | Office Location | textfield | |
| 17 | `work_anniversary` | Work Anniversary | datefield | |
| 18 | `certifications` | Certifications | tagsfield | |
| 19 | `skills` | Skills | tagsfield | |
| 20 | `hobbies` | Hobbies | tagsfield | |
| 21 | `linkedin` | LinkedIn | linkfield | |
| 22 | `twitter` | Twitter | linkfield | |
| 23 | `facebook` | Facebook | linkfield | |
| 24 | `instagram` | Instagram | linkfield | |
| 25 | `theme` | Theme | singleselectfield | |
| 26 | `dob` | Date of Birth | datefield | |
| 27 | `profile_image` | Profile Image | filefield | |
| 28 | `cover_image` | Cover Image | filefield | |
| 29 | `cover_template` | Cover Template | textfield | |
| 30 | `biography` | Biography | textareafield | |
| 31 | `phone` | Phone | phonefield | |
| 32 | `teams` | Teams | linkedrecordfield | → Teams |
| 33 | `ip_address` | IP Address | textfield | |
| 34 | `last_login` | Last Login | datefield | |
| 35 | `availability_status` | Availability Status | textfield | |
| 36 | `first_created` | First Created | datefield | |
| 37 | `last_updated` | Last Updated | datefield | |
| 38 | `external_username` | External Username | textfield | |

---

## SB UW & Estimating (PIPE)

**Solution ID:** `6799736a8132f91bb56eb028`

### Project Scenarios

**App ID:** `67997f990bdbae991317bf69` | **Fields:** 94

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Scenario Name | recordtitlefield | Primary field |
| 2 | `s...` | UW Resale Price | currencyfield | |
| 3 | `s...` | Target Purchase Price | currencyfield | |
| 4 | `s...` | BA/Cop % | percentfield | |
| 5 | `s...` | Net Profit | currencyfield | |
| 6 | `s...` | Annualized ROI | percentfield | |
| 7 | `s...` | Decision/Outcome | singleselectfield | |
| 8 | `s...` | Link to Project Budget Management | linkedrecordfield | → Project Budget Management-G702 |
| 9 | `s...` | Link to Bid Tracking System | linkedrecordfield | → Bid Solicitation System |
| 10 | `s...` | Scenario Phase Options | linkedrecordfield | → Phase Options |
| 11 | `s...` | Scenario Phase | singleselectfield | |
| 12 | `s...` | Project | linkedrecordfield | → Projects (SB-Biz Dev) |
| 13 | `first_created` | First Created | datefield | |
| 14 | `last_updated` | Last Updated | datefield | |
| 15–94 | `s...` | (Various UW calculation fields) | formulafield / numberfield / currencyfield | Holdback %, Soft Costs, Hard Costs, ARV, Closing Costs, Carry Costs, Profit Margin, Exit Strategy, etc. |

### Phase Options

**App ID:** `691e5913add41a4fa38b02f7` | **Fields:** 14

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Phase + Option Title | recordtitlefield | Primary field |
| 2 | `s...` | Link to All Items & Costs Quotes | linkedrecordfield | → All Items & Amounts |
| 3 | `s...` | Phases/Inventory | linkedrecordfield | → Project Phases/Inventory |
| 4 | `s...` | Project Scenario | linkedrecordfield | → Project Scenarios |
| 5 | `s...` | Phase Start Date | datefield | |
| 6 | `s...` | Phase End Date | datefield | |
| 7 | `s...` | Phase Total | formulafield | |
| 8–14 | `s...` | (Supporting fields) | various | |

### All Items & Amounts

**App ID:** `6799736a8132f91bb56eb068` | **Fields:** 58

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Description of Work | recordtitlefield | Primary field |
| 2 | `s...` | Priority | singleselectfield | |
| 3 | `s...` | Unit Variable | textfield | |
| 4 | `s...` | UW Material Cost | currencyfield | |
| 5 | `s...` | UW Labor Cost | currencyfield | |
| 6 | `s...` | UW Total | currencyfield | |
| 7 | `s...` | Phase Options | linkedrecordfield | → Phase Options |
| 8 | `s...` | Spread Method | linkedrecordfield | → Spread Methods |
| 9 | `s...` | Cost Code | linkedrecordfield | → Intacct Cost Codes |
| 10–58 | `s...` | (UW bid/quote fields, formula totals) | various | |

### Construction Items & Costs Templates

**App ID:** `679acbfc3c381ccc106b6865` | **Fields:** 20

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Description of Work | recordtitlefield | Primary field |
| 2 | `s...` | Phase | singleselectfield | |
| 3 | `s...` | Cost Code | linkedrecordfield | → Intacct Cost Codes |
| 4 | `s...` | SF/Units | numberfield | |
| 5 | `s...` | UW Material Cost | currencyfield | |
| 6 | `s...` | UW Labor Cost | currencyfield | |
| 7 | `s...` | UW Total | currencyfield | |
| 8–20 | `s...` | (Template metadata fields) | various | |

### Items by Period-Spread

**App ID:** `691e59517933ef180299604c` | **Fields:** 24

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Item Estimate | linkedrecordfield | → All Items & Amounts |
| 3 | `s...` | Month | numberfield | |
| 4 | `s...` | Period Index | numberfield | |
| 5 | `s...` | Account Code | linkedrecordfield | → Intacct Account Codes |
| 6 | `s...` | Cash Flow Section | singleselectfield | |
| 7–24 | `s...` | (Spread/period calculation fields) | formulafield / numberfield | |

### DD Deliverables

**App ID:** `6799736a8132f91bb56eb043` | **Fields:** 25

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Name | recordtitlefield | Primary field |
| 2 | `s...` | Type | singleselectfield | |
| 3 | `s...` | Status | statusfield | |
| 4 | `s...` | Due Date | datefield | |
| 5 | `s...` | Link to Project Scenarios | linkedrecordfield | → Project Scenarios |
| 6–25 | `s...` | (Supporting DD fields) | various | |

### Project Prioritization Tool

**App ID:** `6811abdcdd4eed782c42c863` | **Fields:** 22

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Time to Revenue | numberfield | |
| 3 | `s...` | Gross Margin | numberfield | |
| 4 | `s...` | Capital Efficiency | numberfield | |
| 5 | `s...` | Market Strength | numberfield | |
| 6 | `s...` | Purpose Alignment | numberfield | |
| 7 | `s...` | Hell Yes / No | yesnofield | |
| 8–22 | `s...` | (Scoring/weighting formulas) | formulafield / numberfield | |

### Bid Solicitation System

**App ID:** `69050b815950410698edcf71` | **Fields:** 16

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Line Item | linkedrecordfield | → All Items & Amounts |
| 3 | `s...` | SB Project | linkedrecordfield | → Projects (SB-Biz Dev) |
| 4 | `s...` | Vendor | linkedrecordfield | → Vendors |
| 5 | `s...` | Cost Code | linkedrecordfield | → Intacct Cost Codes |
| 6 | `s...` | Quote | currencyfield | |
| 7–16 | `s...` | (Bid tracking fields) | various | |

### Sale/Lease Phase

**App ID:** `6914cdc46ee998e96d899c91` | **Fields:** 17

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Sale or Lease Scenario | recordtitlefield | Primary field |
| 2 | `s...` | Linked Project | linkedrecordfield | → Projects (SB-Biz Dev) |
| 3 | `s...` | Target Sales Price | currencyfield | |
| 4 | `s...` | Total Cost of Sale or Lease | currencyfield | |
| 5 | `s...` | Net Sales Proceeds | formulafield | |
| 6–17 | `s...` | (Supporting sale/lease fields) | various | |

### Sale/Lease Items & Costs

**App ID:** `6914d0b378c5e8732ead886f` | **Fields:** 14

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Line Item Description | recordtitlefield | Primary field |
| 2 | `s...` | Sale/Lease Phase Scenario | linkedrecordfield | → Sale/Lease Phase |
| 3 | `s...` | Cost Code | linkedrecordfield | → Intacct Cost Codes |
| 4 | `s...` | Rate or Flat Amount | currencyfield | |
| 5 | `s...` | Total Cost | formulafield | |
| 6–14 | `s...` | (Supporting fields) | various | |

---

## Master Property Data

**Solution ID:** `679a9433cfc224b171d856e8`

### Master Property Data-Prop Stream Import

**App ID:** `679a9434cfc224b171d856ea` | **Fields:** 61

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Type | singleselectfield | |
| 3 | `s...` | Zoning | textfield | |
| 4 | `s...` | County | textfield | |
| 5 | `s...` | APN | textfield | |
| 6 | `s...` | Sq Ft | numberfield | |
| 7 | `s...` | Beds | numberfield | |
| 8 | `s...` | Baths | numberfield | |
| 9 | `s...` | Yr Built | numberfield | |
| 10 | `s...` | Property Address | textfield | |
| 11 | `s...` | Est Value | currencyfield | |
| 12 | `s...` | Est Equity | currencyfield | |
| 13 | `s...` | Owner | textfield | |
| 14 | `s...` | Link to DD Deliverables | linkedrecordfield | → DD Deliverables |
| 15 | `s...` | Link to Project Scenarios | linkedrecordfield | → Project Scenarios |
| 16 | `s...` | Link to Agency Contracts | linkedrecordfield | → Agency Contracts |
| 17 | `s...` | Link to Property Contracts | linkedrecordfield | → Property Contracts |
| 18–61 | `s...` | (Additional PropStream import fields) | various | Address components, legal description, loan data, tax data, etc. |

### Test Import 15K

**App ID:** `6851aea3c00fef3e86419b35` | **Fields:** 48

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–48 | `s...` | (Bulk import fields, mirrors Master Property Data) | various | Staging table for large-scale PropStream imports |

### BuildShip SmartSheet Automation

**App ID:** `68ca217bb8ae83822ab1fd5d` | **Fields:** 10

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | SmartSheet Template | linkfield | |
| 3 | `s...` | Formula | formulafield | |
| 4–10 | `s...` | (Automation config fields) | various | |

### SmartSheet Template

**App ID:** `68ca21dcfe34e963604466fb` | **Fields:** 10

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | SmartSheet Template ID | textfield | |
| 3 | `s...` | Link to BuildShip SmartSheet Automation | linkedrecordfield | → BuildShip SmartSheet Automation |
| 4–10 | `s...` | (Template metadata fields) | various | |

---

## Stitser BUILT: SmartSuite Product Roadmap

**Solution ID:** `679af24ee1b001f26db37bb3`

### Features

**App ID:** `679af24ee1b001f26db37bb4` | **Fields:** 22

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Feature Lead | userfield | |
| 3 | `s...` | Status | statusfield | |
| 4 | `s...` | Priority | singleselectfield | |
| 5 | `s...` | Size | singleselectfield | T-shirt sizing |
| 6 | `s...` | Category | singleselectfield | |
| 7 | `s...` | Release | linkedrecordfield | → Releases |
| 8 | `s...` | Milestones | linkedrecordfield | → Major Milestones |
| 9 | `s...` | Company/Function | singleselectfield | |
| 10–22 | `s...` | (Supporting feature fields) | various | |

### Major Milestones

**App ID:** `679af24ee1b001f26db37bc6` | **Fields:** 12

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Epic Status | statusfield | |
| 3 | `s...` | Total Features | formulafield | |
| 4 | `s...` | Link to Features | linkedrecordfield | → Features |
| 5 | `s...` | Completed Features | formulafield | |
| 6 | `s...` | Entity | singleselectfield | |
| 7–12 | `s...` | (Supporting fields) | various | |

### Releases

**App ID:** `679af24ee1b001f26db37bc2` | **Fields:** (inaccessible)

> **Note:** Access denied via MCP token scope. Schema not available.

### Product Market Fit Surveys

**App ID:** `679af24ee1b001f26db37bcb` | **Fields:** (inaccessible)

> **Note:** Access denied via MCP token scope. Schema not available.

---

## Stitser Properties Production

**Solution ID:** `67e1d051f5bd58614d0b0353`

### Contacts

**App ID:** `67e22cb343e37de3ffc5f599` | **Fields:** 96

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Department | singleselectfield | |
| 3 | `s...` | Squad Name | textfield | |
| 4 | `s...` | Cell | phonefield | |
| 5 | `s...` | License Number | textfield | |
| 6 | `s...` | Role | singleselectfield | |
| 7 | `s...` | Trade/Service | multipleselectfield | |
| 8 | `s...` | Link to Agency Contracts | linkedrecordfield | → Agency Contracts |
| 9 | `s...` | Link to Property Contracts | linkedrecordfield | → Property Contracts |
| 10 | `s...` | Link to AP (Agent 1) | linkedrecordfield | → AP Agent 1 to ACCTG Database |
| 11 | `s...` | Link to AP (Agent 2) | linkedrecordfield | → AP Agent 2 to ACCT Database |
| 12–96 | `s...` | (Contact detail, commission, performance fields) | various | Email, address, bio, YTD stats, lifetime volume, etc. |

### Agency Contracts

**App ID:** `67e44801dd39c9f6a78e3bed` | **Fields:** 86

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Contract | recordtitlefield | Primary field |
| 2 | `s...` | Department | singleselectfield | |
| 3 | `s...` | Contract Status | statusfield | |
| 4 | `s...` | Start Date | datefield | |
| 5 | `s...` | Expiration Date | datefield | |
| 6 | `s...` | Agent 1 | linkedrecordfield | → Contacts |
| 7 | `s...` | Agent 2 | linkedrecordfield | → Contacts |
| 8 | `s...` | Commission % | percentfield | |
| 9 | `s...` | Split Category | singleselectfield | |
| 10 | `s...` | Generate Transaction Docs | buttonfield | |
| 11–86 | `s...` | (Pipeline calculations, brokerage splits, link to AR/AP databases) | formulafield / linkedrecordfield / various | |

### AR Accounting Database

**App ID:** `67e5599adcf279f03b2bf69e` | **Fields:** 19

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | COE Date | datefield | Close of Escrow |
| 3 | `s...` | Department | singleselectfield | |
| 4 | `s...` | Total Commission Revenue | currencyfield | |
| 5 | `s...` | Agent 1 Net Earnings | currencyfield | |
| 6 | `s...` | Agent 2 Net Earnings | currencyfield | |
| 7–19 | `s...` | (AR accounting fields) | various | |

### AP Agent 1 to ACCTG Database

**App ID:** `67e5dfac7f062757d045ba38` | **Fields:** 12

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–12 | `s...` | (AP disbursement fields for Agent 1) | various | Amount, Date, Contract, Intacct refs |

### AP Agent 2 to ACCT Database

**App ID:** `67e5e03b239d08cc12264413` | **Fields:** 17

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–17 | `s...` | (AP disbursement fields for Agent 2) | various | Amount, Date, Contract, Intacct refs |

### Property Contracts-3-24-25 Forward

**App ID:** `67e47c67f965bafe2d1f6a6b` | **Fields:** 197

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Property Contract Name | recordtitlefield | Primary field |
| 2 | `s...` | Property Address | textfield | |
| 3 | `s...` | Escrow Company | linkedrecordfield | → Vendors |
| 4 | `s...` | Escrow Officer | linkedrecordfield | → Contacts |
| 5 | `s...` | Contract Status | statusfield | |
| 6 | `s...` | Close of Escrow Date | datefield | |
| 7 | `s...` | Sale Price | currencyfield | |
| 8 | `s...` | Commission Total | currencyfield | |
| 9 | `s...` | Commission % | percentfield | |
| 10 | `s...` | Agent 1 | linkedrecordfield | → Contract Agents |
| 11 | `s...` | Agent 2 | linkedrecordfield | → Contract Agents |
| 12 | `s...` | Link to Agency Contracts | linkedrecordfield | → Agency Contracts |
| 13 | `s...` | Link to Contract & Disclosure MGT | linkedrecordfield | → Contract & Disclosure MGT System |
| 14 | `s...` | Link to Master Property Data | linkedrecordfield | → Master Property Data |
| 15–197 | `s...` | (Comprehensive transaction fields) | various | Buyer/Seller info, title, escrow, disclosures, timelines, Intacct sync, all commission/split calculations |

### Contract & Disclosure MGT System

**App ID:** `67e9a329f3791ce6832979c2` | **Fields:** 44

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Doc/Task | recordtitlefield | Primary field |
| 2 | `s...` | Where to File | singleselectfield | |
| 3 | `s...` | Audit Stage | singleselectfield | |
| 4 | `s...` | Status | statusfield | |
| 5 | `s...` | Property Contract | linkedrecordfield | → Property Contracts |
| 6 | `s...` | Due Date | formulafield | |
| 7–44 | `s...` | (Disclosure tracking fields) | various | |

### Contract & Disclosure Table-Template

**App ID:** `67e8125b2a6cdf42fe4ebdb8` | **Fields:** 18

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–18 | `s...` | (Template configuration fields) | various | Disclosure type, required docs, stage sequencing |

### Contact Yearly Goals

**App ID:** `67fd7754a3f0c76d597d4c6d` | **Fields:** 27

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Year | linkedrecordfield | → Years |
| 3 | `s...` | Contact | linkedrecordfield | → Contacts |
| 4 | `s...` | Units Goal | numberfield | |
| 5 | `s...` | Revenue Earned Goal | currencyfield | |
| 6 | `s...` | Volume Goal | currencyfield | |
| 7–27 | `s...` | (Achievement % formulas, YTD actuals) | formulafield / numberfield | |

### Contact Quarterly Goals

**App ID:** `67fd8c42f7eeea82d354ed16` | **Fields:** 20

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Quarter | linkedrecordfield | → Quarters |
| 3 | `s...` | Contact | linkedrecordfield | → Contacts |
| 4–20 | `s...` | (Quarterly goal/achievement fields) | various | |

### Contact Monthly Goals

**App ID:** `67fd8d0a21ef66b913a2e022` | **Fields:** 26

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Month | linkedrecordfield | → Months |
| 3 | `s...` | Contact | linkedrecordfield | → Contacts |
| 4–26 | `s...` | (Monthly goal/achievement fields) | various | |

### Contract Agents

**App ID:** `67ffdd41ae9d8cd6bae5a7ca` | **Fields:** 30

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field (junction) |
| 2 | `s...` | Contact | linkedrecordfield | → Contacts |
| 3 | `s...` | Property Contract | linkedrecordfield | → Property Contracts |
| 4–30 | `s...` | (Agent role, split, earnings on this transaction) | various | |

### Vendors

**App ID:** `680bf83cb1b4bf3ab8a91055` | **Fields:** 25

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Company Name | textfield | |
| 3 | `s...` | Address | textfield | |
| 4 | `s...` | Services | multipleselectfield | |
| 5 | `s...` | Link to Agency Contracts | linkedrecordfield | |
| 6 | `s...` | Link to Property Contracts | linkedrecordfield | |
| 7–25 | `s...` | (Additional vendor fields) | various | Phone, email, license, insurance, notes |

### Years

**App ID:** `67fd6a67aec578b8ea25a300` | **Fields:** 15

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–15 | `s...` | (Year-level rollup fields) | formulafield / linkedrecordfield | |

### Quarters

**App ID:** `67fd81dcc55d93703f54ed21` | **Fields:** 19

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–19 | `s...` | (Quarter-level rollup fields) | formulafield / linkedrecordfield | |

### Months

**App ID:** `67fd846286a13a9470303b82` | **Fields:** 19

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–19 | `s...` | (Month-level rollup fields) | formulafield / linkedrecordfield | |

### Messages

**App ID:** `68262b5b6d95274d33587e54` | **Fields:** 11

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–11 | `s...` | (Message/communication log fields) | various | |

### Property Contracts-Imported 3-24-25

**App ID:** `67e1d110f77928b1e866708f` | **Fields:** ~130

> **Note:** Legacy import table (pre-3/24/25 contracts). Intentionally excluded from this dictionary. Use "Property Contracts-3-24-25 Forward" for current schema.

---

## SB-Biz Dev

**Solution ID:** `68216a706900e8eaf75a05a6`

### Projects

**App ID:** `68216a706900e8eaf75a05a7` | **Fields:** 173

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Project | recordtitlefield | Primary field |
| 2 | `s...` | Project Status | statusfield | |
| 3 | `s...` | Department | linkedrecordfield | → Stitser BUILT Departments |
| 4 | `s...` | Project Type | linkedrecordfield | → Project Type |
| 5 | `s...` | Parent Project | linkedrecordfield | → Projects (self-referential) |
| 6 | `s...` | Child Projects | linkedrecordfield | → Projects (self-referential) |
| 7 | `s...` | Project Stakeholders | linkedrecordfield | → Project Stakeholder Bridge |
| 8 | `s...` | Decision Gates | linkedrecordfield | → Project Decision Gates |
| 9 | `s...` | Investment Standards | linkedrecordfield | → Project Investment Standards |
| 10 | `s...` | Intacct Project ID | linkedrecordfield | → Intacct Project List |
| 11 | `s...` | Rating: Time to Revenue | numberfield | |
| 12 | `s...` | Rating: Gross Margin | numberfield | |
| 13 | `s...` | Rating: Capital Efficiency | numberfield | |
| 14 | `s...` | Rating: Market Strength | numberfield | |
| 15 | `s...` | Rating: Purpose Alignment | numberfield | |
| 16 | `s...` | Financing Source | singleselectfield | |
| 17–173 | `s...` | (Extensive project tracking fields) | various | Date tracking, budget, actual costs, notes, links to UW/HR/BizDev entities |

### Companies/Entities

**App ID:** `68216a706900e8eaf75a05c0` | **Fields:** 118

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Company Name | recordtitlefield | Primary field |
| 2 | `s...` | Entity Type | singleselectfield | |
| 3 | `s...` | Projects | linkedrecordfield | → Projects |
| 4 | `s...` | Stakeholder Roles | linkedrecordfield | → Project Stakeholder Bridge |
| 5–41 | `s...` | (Intacct data fields) | textfield | 37+ Intacct sync fields: Vendor ID, Customer ID, Location ID, GL data |
| 42–118 | `s...` | (CRM fields) | various | Address, phone, email, notes, filing links |

### People

**App ID:** `68216a706900e8eaf75a05af` | **Fields:** 100

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Name | recordtitlefield | Primary field |
| 2 | `s...` | Company | linkedrecordfield | → Companies/Entities |
| 3 | `s...` | Role | singleselectfield | |
| 4 | `s...` | Link to Training | linkedrecordfield | → SB Training & Certifications |
| 5 | `s...` | Link to Journals | linkedrecordfield | → Journals/Rituals |
| 6 | `s...` | Link to Goals | linkedrecordfield | → Goals |
| 7 | `s...` | Projects (various roles) | linkedrecordfield | Multiple project link fields |
| 8–100 | `s...` | (CRM contact fields) | various | Email, phone, address, bio, social links, notes |

### Project Stakeholder Bridge

**App ID:** `6996a3079f04b5f34a06ad88` | **Fields:** 28

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field (junction) |
| 2 | `s...` | Person | linkedrecordfield | → People |
| 3 | `s...` | Project | linkedrecordfield | → Projects |
| 4 | `s...` | Company | linkedrecordfield | → Companies/Entities |
| 5 | `s...` | Role | singleselectfield | |
| 6 | `s...` | Amount/Commitment | currencyfield | |
| 7 | `s...` | Stage | singleselectfield | |
| 8–28 | `s...` | (Supporting bridge fields) | various | |

### Biz Dev/Cap Raise Roster

**App ID:** `6822a4aff9e6f0446023fd37` | **Fields:** 11

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–11 | `s...` | (Roster/pipeline tracking fields) | various | |

### Notes & Comments

**App ID:** `6894e64f621641b3ef90fa99` | **Fields:** 49

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Note Type | singleselectfield | |
| 3 | `s...` | Content | richtextareafield | |
| 4 | `s...` | Author | linkedrecordfield | → People |
| 5 | `s...` | Related Project | linkedrecordfield | → Projects |
| 6 | `s...` | Related Person | linkedrecordfield | → People |
| 7 | `s...` | Related Company | linkedrecordfield | → Companies/Entities |
| 8–49 | `s...` | (Links to other apps, metadata) | various | Shared notes hub linking to many other solutions |

### Event List & CTA

**App ID:** `685ed67860ccdf878d9bb190` | **Fields:** 14

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–14 | `s...` | (Event tracking fields) | various | Date, Location, CTA, Attendance link |

### Formation Channels, Project Types, Roles

**App ID:** `68abe2a5e3b7b1aecb9a2a02` | **Fields:** 16

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–16 | `s...` | (Catalog fields) | various | Channel Name, Project Types, Roles taxonomy |

### Title/Type

**App ID:** `690123a6e6639f3bdb5574f7` | **Fields:** 8

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–8 | `s...` | (Type classification fields) | various | |

### Project Phases/Inventory

**App ID:** `691e58e7f80ad0eb577fa157` | **Fields:** 9

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–9 | `s...` | (Phase/inventory fields) | various | Phase Name, Sequence, Status |

### Tags

**App ID:** `6998b3bb915fcb989d6da580` | **Fields:** 9

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–9 | `s...` | (Tag metadata) | various | |

### S-Bos Workspace Schema

**App ID:** `699e783529f85d02d9608917` | **Fields:** 15

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–15 | `s...` | (Schema documentation fields) | various | Solution ID, App ID, App Name, Field Count — meta-table documenting this workspace |

### Lease/Purchase Offers Received

**App ID:** `69c47dedacf4cfa10f8b336b` | **Fields:** 11

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–11 | `s...` | (Offer tracking fields) | various | Offer Amount, Type, Status, Project link |

### Event Attendance

**App ID:** `69ab05afd9609d8cdb67e3df` | **Fields:** 16

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Person | linkedrecordfield | → People |
| 3 | `s...` | Event | linkedrecordfield | → Event List & CTA |
| 4–16 | `s...` | (Attendance/engagement fields) | various | |

### Project Investment Standards

**App ID:** `6a066068324f0ef0c6692f09` | **Fields:** 31

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Min IRR | percentfield | |
| 3 | `s...` | Max LTV | percentfield | |
| 4 | `s...` | Max Duration | numberfield | |
| 5–31 | `s...` | (Investment criteria fields) | various | Risk thresholds, return targets, capital constraints |

### Project Type

**App ID:** `6a06221f3502ff6d098b571d` | **Fields:** 23

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Business Vertical | singleselectfield | |
| 3 | `s...` | Revenue Model | singleselectfield | |
| 4 | `s...` | Typical Duration | numberfield | |
| 5–23 | `s...` | (Product type catalog fields) | various | |

### Project Decision Gates

**App ID:** `6a06459b8e32bd74b106e6fd` | **Fields:** 25

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Project | linkedrecordfield | → Projects |
| 3 | `s...` | Gate Status | statusfield | |
| 4–25 | `s...` | (Decision gate criteria and approval fields) | various | |

### Smartsheet Schedule Templates

**App ID:** `68c255e43b90a8ab00d2e794` | **Fields:** 9

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–9 | `s...` | (Template config fields) | various | |

### Meetings

**App ID:** `6a0cff32f77ad06285909dcf` | **Fields:** (inaccessible)

> **Note:** Access denied via MCP token scope. Schema not available.

---

## The SB Game App

**Solution ID:** `68216f48f98789b5bb095a36`

### Goals

**App ID:** `6824d4d1885a8769bd2dfc0d` | **Fields:** 59

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Goal Title | recordtitlefield | Primary field |
| 2 | `s...` | GYR Status | singleselectfield | Green/Yellow/Red |
| 3 | `s...` | Goal Type | singleselectfield | |
| 4 | `s...` | Milestones | linkedrecordfield | → Milestones |
| 5 | `s...` | Measurable | linkedrecordfield | → Stats Menu |
| 6 | `s...` | Target Amount | numberfield | |
| 7 | `s...` | Total Progress | formulafield | |
| 8 | `s...` | Reporting Rhythm | singleselectfield | |
| 9 | `s...` | Reporting Grade | singleselectfield | |
| 10 | `s...` | Department | linkedrecordfield | → Stitser BUILT Departments |
| 11–59 | `s...` | (Supporting goal fields) | various | Owner, due date, notes, GYR history, current priorities link |

### Current Priorities

**App ID:** `68216f48f98789b5bb095a4b` | **Fields:** 58

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Priority Title | recordtitlefield | Primary field |
| 2 | `s...` | GYR Status | singleselectfield | |
| 3 | `s...` | Type | singleselectfield | |
| 4 | `s...` | Due Date | datefield | |
| 5 | `s...` | Related Goal | linkedrecordfield | → Goals |
| 6 | `s...` | Measurable Metric | linkedrecordfield | → Stats Menu |
| 7 | `s...` | Target Amount | numberfield | |
| 8 | `s...` | Reporting Rhythm | singleselectfield | |
| 9 | `s...` | Reporting Grade | singleselectfield | |
| 10–58 | `s...` | (Supporting priority fields) | various | Owner, milestones, tasks, GYR reports |

### Milestones

**App ID:** `68216f48f98789b5bb095a37` | **Fields:** 51

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Milestone | recordtitlefield | Primary field |
| 2 | `s...` | GYR Status | singleselectfield | |
| 3 | `s...` | Due Date | datefield | |
| 4 | `s...` | Link to Priorities | linkedrecordfield | → Current Priorities |
| 5 | `s...` | Milestone Owner | userfield | |
| 6–51 | `s...` | (Reporting cycle fields, task links) | various | |

### GYR Status Reports

**App ID:** `68216f48f98789b5bb095a51` | **Fields:** 59

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Reporting Date | datefield | |
| 3 | `s...` | Type | singleselectfield | Goal / Priority / Milestone |
| 4 | `s...` | GYR Status | singleselectfield | |
| 5 | `s...` | Issues | textareafield | |
| 6 | `s...` | Link to Milestone | linkedrecordfield | |
| 7 | `s...` | Link to Priority | linkedrecordfield | |
| 8 | `s...` | Link to Goal | linkedrecordfield | |
| 9 | `s...` | System GYR | formulafield | |
| 10 | `s...` | Human GYR Response | singleselectfield | |
| 11–59 | `s...` | (Supporting report fields) | various | |

### Tasks

**App ID:** `683e80437ee1bca637ba6fde` | **Fields:** 24

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | What | recordtitlefield | Primary field |
| 2 | `s...` | Status | statusfield | |
| 3 | `s...` | Task Due | datefield | |
| 4 | `s...` | Milestone | linkedrecordfield | → Milestones |
| 5 | `s...` | Who | userfield | |
| 6 | `s...` | Practitioner | linkedrecordfield | → People |
| 7–24 | `s...` | (Supporting task fields) | various | |

### Priorities Template

**App ID:** `68806392f4f22c070b80af40` | **Fields:** 33

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–33 | `s...` | (Template fields mirroring Current Priorities) | various | |

### Stats

**App ID:** `6840927ebcfa2d2bfef039e2` | **Fields:** 32

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Associated Goal | linkedrecordfield | → Goals |
| 3 | `s...` | Associated Priority | linkedrecordfield | → Current Priorities |
| 4 | `s...` | Associated Milestone | linkedrecordfield | → Milestones |
| 5 | `s...` | Begin Date | datefield | |
| 6 | `s...` | End Date | datefield | |
| 7 | `s...` | Amount for Period | numberfield | |
| 8–32 | `s...` | (Supporting stat fields) | formulafield / various | |

### Stats Menu

**App ID:** `68409420391d32d925740e28` | **Fields:** 14

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field (measurables catalog) |
| 2–14 | `s...` | (Measurable definition fields) | various | Unit, direction (higher/lower is better), category |

### Milestone Templates

**App ID:** `6880637e4585c8c8b0ae9ee8` | **Fields:** 29

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–29 | `s...` | (Template fields mirroring Milestones) | various | |

### XX-Template Sample Metrics (Monthly)

**App ID:** `68216f48f98789b5bb095a5a` | **Fields:** 158

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–158 | `s...` | (SmartSuite template SaaS/KPI metric fields) | various | Platform template with 157 KPI metric columns for monthly reporting |

---

## Stitser BUILT. Budget & Spend MGT

**Solution ID:** `6871485bc057b80f1383055b`

### Employee/Resource Department Budget

**App ID:** `6871911ef9ee492e1d37426a` | **Fields:** 23

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Employee | linkedrecordfield | → Intacct-Employee List |
| 3 | `s...` | Department | linkedrecordfield | → Stitser BUILT Departments |
| 4 | `s...` | Ee Dept Allocation % | percentfield | |
| 5 | `s...` | Salary Allocation | formulafield | |
| 6 | `s...` | Health Insurance Allocation | formulafield | |
| 7 | `s...` | Vehicle Allocation | formulafield | |
| 8 | `s...` | FICA Allocation | formulafield | |
| 9–23 | `s...` | (Additional allocation formula fields) | formulafield | |

### Office Space Use

**App ID:** `6871854b214ff5b974b6b91a` | **Fields:** 20

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–20 | `s...` | (Office space allocation/cost fields) | various | Sq Ft, Department, Cost per SF, Allocated Cost |

### Recurring SB Expense Items & Budget

**App ID:** `68b9e0dbf4d3a9ad19eaf172` | **Fields:** 21

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–21 | `s...` | (Recurring expense catalog fields) | various | Category, Vendor, Budget Amount, Frequency, Account Code |

### Recurring SB Expense Bills & Invoices

**App ID:** `68b9ed839abcf1142c050df1` | **Fields:** 21

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–21 | `s...` | (Bill/invoice tracking fields) | various | Amount, Due Date, Status, Expense Item link |

### Time Cards

**App ID:** `68b9f36ad4f6dde73670924a` | **Fields:** 27

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Customer Company | linkedrecordfield | → Companies/Entities |
| 3 | `s...` | Project | linkedrecordfield | → Projects |
| 4 | `s...` | Department | linkedrecordfield | → Stitser BUILT Departments |
| 5 | `s...` | Hours | numberfield | |
| 6 | `s...` | Name | linkedrecordfield | → People |
| 7 | `s...` | Employee Record | linkedrecordfield | → Intacct-Employee List |
| 8 | `s...` | Cost Code | linkedrecordfield | → Intacct Cost Codes |
| 9 | `s...` | Pay Period | linkedrecordfield | → Pay Periods |
| 10–27 | `s...` | (Supporting timecard fields) | various | Rate, Total Amount, Intacct sync status |

### Pay Periods

**App ID:** `68fb95f13ab246d514c03530` | **Fields:** 24

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–24 | `s...` | (Pay period fields) | various | Start/End Date, Total Hours, Status, Employee Pay Period links |

### Employee Pay Periods

**App ID:** `68fb96a8041d6cc038b0d348` | **Fields:** 25

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–25 | `s...` | (Per-employee pay period summary fields) | various | Employee, Pay Period, Total Hours, Pay Amount |

### Invoice Employee Breakdown

**App ID:** `68fb96cbc2d08fc35bf32519` | **Fields:** 30

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–30 | `s...` | (Invoice-employee allocation fields) | various | Invoice, Employee, Hours, Cost, Department allocation |

### Intacct Invoice Line Items

**App ID:** `68fb9712bc1f8541822e4372` | **Fields:** 39

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Cost Code | linkedrecordfield | → Intacct Cost Codes |
| 3 | `s...` | Project | linkedrecordfield | → Intacct Project List |
| 4 | `s...` | Customer Company | linkedrecordfield | → Intacct Customer Table |
| 5 | `s...` | Invoice | linkedrecordfield | → Intacct Invoices |
| 6 | `s...` | AMOUNT | formulafield | |
| 7–39 | `s...` | (Intacct sync fields and line item details) | formulafield / textfield | |

### Intacct Invoices

**App ID:** `68fb9729547dd14350a05547` | **Fields:** 20

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–20 | `s...` | (Invoice header fields) | various | Invoice Number, Customer, Date, Total, Status, Intacct ID |

### Dashboard Controls

**App ID:** `69b4d52c2b7d101293098a29` | **Fields:** 16

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–16 | `s...` | (Dashboard filter/control fields) | various | Loan portfolio dashboard controls — Date range, Entity filter, Loan type |

### Loans

**App ID:** `69aba52da3fa0e7ebb7424f7` | **Fields:** 59

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Project/Property | linkedrecordfield | → Projects |
| 3 | `s...` | Lender | linkedrecordfield | → Companies/Entities |
| 4 | `s...` | Borrower | linkedrecordfield | → Companies/Entities |
| 5 | `s...` | Points | percentfield | |
| 6 | `s...` | Interest Rate | percentfield | |
| 7 | `s...` | Origination Date | datefield | |
| 8 | `s...` | Amortization | numberfield | |
| 9 | `s...` | SB Asset/Liability | singleselectfield | |
| 10 | `s...` | Loan Record ID | recordidfield | |
| 11–59 | `s...` | (Supporting loan fields) | various | Principal, Balance, Maturity Date, Payoff Amount, Transaction links |

### Loan Transactions

**App ID:** `69aba98a2d32fb2007e5290f` | **Fields:** 23

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Loan | linkedrecordfield | → Loans |
| 3–23 | `s...` | (Transaction fields) | various | Date, Amount, Type (Draw/Payment/Fee), Running Balance |

### Private-Team Member Notes

**App ID:** `69c9585d3f5431f1abe957a6` | **Fields:** 10

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–10 | `s...` | (Private note fields) | various | Author, Date, Content, Team Member link |

---

## SB Project MGT (WIP)

**Solution ID:** `68a8c3d1bba73ca6e62d00f0`

### Check Lists

**App ID:** `6a060dadc513b3329b7d4485` | **Fields:** 26

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Linked Project | linkedrecordfield | → Projects |
| 3 | `s...` | Linked Loan | linkedrecordfield | → Loans |
| 4 | `s...` | Linked Person | linkedrecordfield | → People |
| 5 | `s...` | Checklist Type | singleselectfield | |
| 6 | `s...` | Tasks | linkedrecordfield | → Check List Tasks |
| 7 | `s...` | Decision Gate | linkedrecordfield | → Project Decision Gates |
| 8–26 | `s...` | (Assessment score formula fields) | formulafield | |

### Check List Tasks

**App ID:** `68a8e17251dc814e8c529f3f` | **Fields:** 29

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Check List | linkedrecordfield | → Check Lists |
| 3–29 | `s...` | (Task detail fields) | various | Status, Due Date, Assigned To, Notes |

### Check List Template

**App ID:** `68a8dc0a1fd978c78aa3bc21` | **Fields:** 16

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–16 | `s...` | (Template fields) | various | |

### Project Budget Management-G702

**App ID:** `68a8c3d2bba73ca6e62d0cb5` | **Fields:** 51

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Project | linkedrecordfield | → Projects |
| 3 | `s...` | G703 Line Items | linkedrecordfield | → Project Budget Line Items-G703 |
| 4 | `s...` | Pay Applications | linkedrecordfield | → Pay Apps |
| 5 | `s...` | Original Contract Sum | currencyfield | |
| 6 | `s...` | Net Change by Change Orders | formulafield | |
| 7 | `s...` | Contract Sum to Date | formulafield | |
| 8–51 | `s...` | (G702 pay application summary fields) | formulafield / currencyfield | Completed Work, Stored Materials, % Complete, Retainage, etc. |

### Pay Apps

**App ID:** `68db724638a208d3257ea3a9` | **Fields:** 25

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Budget Management-G702 | linkedrecordfield | → Project Budget Management-G702 |
| 3–25 | `s...` | (Pay application fields) | various | Application #, Period, Amount Requested, Approved |

### Change Order Line Items

**App ID:** `68a8c3d3bba73ca6e62d1879` | **Fields:** 25

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Change Order | linkedrecordfield | → Change Orders |
| 3–25 | `s...` | (Change order line item fields) | various | Description, Amount, Cost Code, Status |

### Project Budget Line Items-G703

**App ID:** `68db71a363e88ace0bd45439` | **Fields:** 54

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Budget Management-G702 | linkedrecordfield | → Project Budget Management-G702 |
| 3 | `s...` | Cost Code | linkedrecordfield | → Intacct Cost Codes |
| 4 | `s...` | Intacct Project | linkedrecordfield | → Intacct Project List |
| 5–54 | `s...` | (G703 cost line item fields) | various | Scheduled Value, Work Completed, Stored Materials, % Complete, Retainage, Intacct sync fields |

### Change Orders

**App ID:** `691551188ef2949cc9454ca6` | **Fields:** 19

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Project Budget-G702 | linkedrecordfield | → Project Budget Management-G702 |
| 3 | `s...` | Change Order Line Items | linkedrecordfield | → Change Order Line Items |
| 4–19 | `s...` | (Change order summary fields) | various | CO Number, Date, Net Amount, Status, Approval |

### Project Schedule

**App ID:** `68a8d2153c056ca71c9928fd` | **Fields:** 24

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Project | linkedrecordfield | → Projects |
| 3–24 | `s...` | (Schedule item fields) | various | Phase, Task, Start Date, End Date, Duration, Dependencies |

### Project Schedule-Template

**App ID:** `68a8c3d1bba73ca6e62d06d3` | **Fields:** 18

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–18 | `s...` | (Schedule template fields) | various | |

### Bills & Invoices

**App ID:** `68a8c3d2bba73ca6e62d1297` | **Fields:** 59

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Project | linkedrecordfield | → Projects |
| 3 | `s...` | Vendor | linkedrecordfield | → Vendors |
| 4 | `s...` | Cost Code | linkedrecordfield | → Intacct Cost Codes |
| 5 | `s...` | Invoice Amount | currencyfield | |
| 6 | `s...` | Status | statusfield | |
| 7 | `s...` | Intacct AP sync fields | textfield | Bill ID, Journal Entry, etc. |
| 8–59 | `s...` | (Cost tracking and AP integration fields) | various | Due Date, GL Account, Department, Tax |

### Project Dates

**App ID:** `69bb7d64740e0e696d88c47f` | **Fields:** 19

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Project | linkedrecordfield | → Projects |
| 3 | `s...` | Estimated Date | datefield | |
| 4 | `s...` | Baseline Date | datefield | |
| 5 | `s...` | Actual Date | datefield | |
| 6 | `s...` | Event Type | singleselectfield | |
| 7–19 | `s...` | (Date tracking formula fields) | formulafield | Variance, days ahead/behind |

### Baseline Budget Items

**App ID:** `69bb89ebf6a195c2c73a3b3e` | **Fields:** 33

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Project | linkedrecordfield | → Projects |
| 3–33 | `s...` | (Baseline vs. actual budget fields) | various | Cost Code, Baseline Amount, Current Budget, Variance |

### Project Dates Template

**App ID:** `69bb833af3c9796e666df705` | **Fields:** 14

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–14 | `s...` | (Date event template fields) | various | |

### Table 1

**App ID:** `699d08b30cacb694c15b584e` | **Fields:** 11

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–11 | `s...` | (Placeholder/scratch fields) | various | Likely staging or scratch table |

---

## Employee IT Asset Management

**Solution ID:** `68c048f76454e226763a3586`

### People

**App ID:** `68c048f76454e226763a3587` | **Fields:** 14

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Full Name | recordtitlefield | Primary field |
| 2 | `s...` | Email | emailfield | |
| 3 | `s...` | Phone | phonefield | |
| 4 | `s...` | Role | singleselectfield | |
| 5 | `s...` | Laptops | linkedrecordfield | → Laptops |
| 6 | `s...` | Applications | linkedrecordfield | → Applications |
| 7 | `s...` | IT Costs | linkedrecordfield | → IT Costs |
| 8 | `s...` | IT Planning | linkedrecordfield | → IT Planning |
| 9–14 | `s...` | (Supporting IT fields) | various | |

### Laptops

**App ID:** `68c048f76454e226763a3cc8` | **Fields:** 13

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Brand | singleselectfield | |
| 3 | `s...` | Serial Number | textfield | |
| 4 | `s...` | Type | singleselectfield | |
| 5 | `s...` | PIN | textfield | |
| 6 | `s...` | OS | singleselectfield | |
| 7 | `s...` | Current Owner | linkedrecordfield | → People |
| 8–13 | `s...` | (Supporting fields) | various | Purchase Date, Warranty, Notes |

### Applications

**App ID:** `68c048f86454e226763a4409` | **Fields:** 12

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Application Name | recordtitlefield | Primary field |
| 2 | `s...` | Corporate Account | textfield | |
| 3 | `s...` | Subscription Type | singleselectfield | |
| 4 | `s...` | Price | currencyfield | |
| 5 | `s...` | Users | linkedrecordfield | → People |
| 6–12 | `s...` | (Supporting fields) | various | Renewal Date, Category, Notes |

### IT Costs

**App ID:** `68c048f86454e226763a4b4a` | **Fields:** 11

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Item | recordtitlefield | Primary field |
| 2 | `s...` | Cost | currencyfield | |
| 3 | `s...` | Date | datefield | |
| 4 | `s...` | Category | singleselectfield | |
| 5 | `s...` | Related Person | linkedrecordfield | → People |
| 6–11 | `s...` | (Supporting fields) | various | |

### IT Planning

**App ID:** `68c048f86454e226763a528b` | **Fields:** 11

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Objective | recordtitlefield | Primary field |
| 2 | `s...` | Due Date | datefield | |
| 3 | `s...` | Estimated Cost | currencyfield | |
| 4 | `s...` | Status | statusfield | |
| 5 | `s...` | Responsible Person | linkedrecordfield | → People |
| 6–11 | `s...` | (Supporting fields) | various | |

---

## SB Training & Certifications

**Solution ID:** `68d480e2727607560a7f0d22`

> **Gap Analysis Note:** This solution has a solid 3-tier content hierarchy (Tracks → Courses → Lessons) and links People to content via "Completed By" and "People Enrolled" fields. However, it lacks a dedicated **Progress/Enrollment table** — completion is tracked inline via linked-record fields rather than per-user progress records. The following gaps exist vs. a full Lessons/Courses/Tracks/Progress data model:
> - No **Progress/Enrollment** table (no per-user, per-lesson/course/track record with completion date, status, or score)
> - No **Assessment or Quiz** table
> - No **Certificate issuance** table or record
> - No **Prerequisites** tracking between lessons or courses
> - No **Attempt** tracking (attempts, pass/fail scores, time spent)
> - No **Completion Date** or **Progress %** per user per item
>
> If building a Kompass LMS front-end, a `Progress` table bridging `People → Lesson/Course/Track` with `status`, `completion_date`, `score`, and `attempt_count` fields would be the primary schema addition needed.

### Learning Tracks/Certifications

**App ID:** `68d480e2727607560a7f0d23` | **Fields:** 12

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `required` | Required | yesnofield | Is this track required? |
| 3 | `courses` | Courses | linkedrecordfield | → Courses |
| 4 | `description` | Description | textareafield | |
| 5 | `followed_by` | Followed By | userfield | |
| 6 | `sa9a7cf910` | Track Image | filefield | |
| 7 | `autonumber` | Autonumber | autonumberfield | |
| 8 | `first_created` | First Created | datefield | |
| 9 | `last_updated` | Last Updated | datefield | |
| 10 | `comments_count` | Comments Count | formulafield | |
| 11 | `sfe3f8d6f8` | Completed By | linkedrecordfield | → People (SB-Biz Dev) |
| 12 | `s97047a59a` | Learning Track Record ID | recordidfield | |

### Courses

**App ID:** `68d480e2727607560a7f0d2c` | **Fields:** 18

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `description` | Description | textareafield | |
| 3 | `followed_by` | Followed By | userfield | |
| 4 | `sed29900f7` | Lessons | linkedrecordfield | → Lesson Catalog |
| 5 | `link_to_learning_tracks` | Learning Track(s) | linkedrecordfield | → Learning Tracks/Certifications |
| 6 | `s2b5116b37` | Course Image | filefield | |
| 7 | `autonumber` | Autonumber | autonumberfield | |
| 8 | `first_created` | First Created | datefield | |
| 9 | `last_updated` | Last Updated | datefield | |
| 10 | `comments_count` | Comments Count | formulafield | |
| 11 | `sd19a0ef26` | Audiences | multipleselectfield | |
| 12 | `sf02d23ce9` | People Completed | linkedrecordfield | → People (SB-Biz Dev) |
| 13 | `s1a2f53021` | Course Creator | linkedrecordfield | → People (SB-Biz Dev) |
| 14 | `sf4cbfb4bb` | Course Status | singleselectfield | |
| 15 | `s59d010ab9` | Approx Time of All Lessons | formulafield | Sum of Read/View Time from linked lessons |
| 16 | `s820e44725` | People Enrolled | linkedrecordfield | → People (SB-Biz Dev) |
| 17 | `s7f339eb11` | Learning Track Record ID | lookupfield | Lookup from linked track |
| 18 | `sdc776b2fa` | Course Record ID | recordidfield | |

### Lesson Catalog

**App ID:** `68d480e2727607560a7f0d26` | **Fields:** 28

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Name | recordtitlefield | Primary field |
| 2 | `type` | Type | singleselectfield | |
| 3 | `description` | Description | textareafield | |
| 4 | `link_to_courses` | Courses | linkedrecordfield | → Courses |
| 5 | `resource_documentation` | Resource Documentation | filefield | |
| 6 | `resource_links` | Resource Links | linkfield | |
| 7 | `autonumber` | Autonumber | autonumberfield | |
| 8 | `first_created` | First Created | datefield | |
| 9 | `last_updated` | Last Updated | datefield | |
| 10 | `comments_count` | Comments Count | formulafield | |
| 11 | `followed_by` | Followed By | userfield | |
| 12 | `sa2c745c03` | Read/View Time (Mins) | numberfield | |
| 13 | `s7f0c1ebc2` | Author(s) | linkedrecordfield | → People (SB-Biz Dev) |
| 14 | `s78fb66338` | Completed By | linkedrecordfield | → People (SB-Biz Dev) |
| 15 | `s40d321a29` | Invited/Enrolled | linkedrecordfield | → People (SB-Biz Dev) |
| 16 | `sf5d257c08` | Video Link | linkfield | |
| 17 | `sf8d0af684` | Lesson Text | richtextareafield | |
| 18 | `saa63f1abb` | Link to Notes & Comments | linkedrecordfield | → Notes & Comments (SB-Biz Dev) |
| 19 | `s953a47072` | Comments Status | formulafield | |
| 20 | `s148d2612e` | Lesson Completed by anyone? | formulafield | Boolean: has any person completed this? |
| 21 | `sd97f4c063` | Lesson Status | singleselectfield | |
| 22 | `se82cbdade` | Lesson Number in Course | numberfield | Ordering field |
| 23 | `s02f2665f4` | Docs or Slides iFrame URL | formulafield | |
| 24 | `sefd6f1609` | Doc or Slides URL | textfield | |
| 25 | `sc2502aa9e` | Quote | formulafield | |
| 26 | `sb29dd5b7e` | Template Lesson Docs | formulafield | |
| 27 | `s3d7df64cb` | Lesson Record ID | recordidfield | |
| 28 | `sf613b5b8c` | Course Record ID | lookupfield | Lookup from linked course |

---

## The Stitser Way

**Solution ID:** `68f8f8fd3757414d70d94ade`

### Journals/Rituals

**App ID:** `68f8f8fe3757414d70d94ae0` | **Fields:** 103

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Journal Date | datefield | |
| 3 | `s...` | Ritual Type | singleselectfield | Daily / Weekly / Monthly |
| 4 | `s...` | BODY GYR | singleselectfield | Green/Yellow/Red |
| 5 | `s...` | BEING GYR | singleselectfield | |
| 6 | `s...` | BALANCE GYR | singleselectfield | |
| 7 | `s...` | BUSINESS GYR | singleselectfield | |
| 8 | `s...` | Overall GYR | singleselectfield | |
| 9–103 | `s...` | (Journal entry fields across 4 life domains) | richtextareafield / various | Morning intentions, evening reflections, wins, gratitude, energy ratings, sleep, exercise, nutrition, relationships, work, finance — 94 additional fields |

### Weekly Table

**App ID:** `68fa765fd41506b89f178cf7` | **Fields:** 15

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Week Start Date | datefield | |
| 3 | `s...` | Weekly Dominos | textareafield | The week's top priorities |
| 4 | `s...` | Journals | linkedrecordfield | → Journals/Rituals |
| 5 | `s...` | Principles | linkedrecordfield | → Principles & Realizations |
| 6–15 | `s...` | (Supporting weekly fields) | various | |

### Decisions

**App ID:** `68feda0035fd19c93de8d757` | **Fields:** 16

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Decision Date | datefield | |
| 3 | `s...` | Company | linkedrecordfield | → Companies/Entities |
| 4 | `s...` | Author | linkedrecordfield | → People |
| 5 | `s...` | Project | linkedrecordfield | → Projects |
| 6–16 | `s...` | (Decision content and outcome fields) | richtextareafield / various | Context, Options Considered, Choice Made, Expected Outcome, Actual Outcome |

### Principles & Realizations

**App ID:** `6913f7e0e36376fca2b14b0e` | **Fields:** 10

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–10 | `s...` | (Principle fields) | various | Category, Content, Source, Date Discovered |

### BAC-Day Types

**App ID:** `69458768a624db0406935efc` | **Fields:** 9

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–9 | `s...` | (Day type catalog fields) | various | Color, Description, BAC category |

### BAC-Calendar Events

**App ID:** `6945877b88051cf9ac527e8a` | **Fields:** 30

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Event Date | datefield | |
| 3 | `s...` | Day Type | linkedrecordfield | → BAC-Day Types |
| 4 | `s...` | Goal | linkedrecordfield | → Goals |
| 5 | `s...` | Priority | linkedrecordfield | → Current Priorities |
| 6 | `s...` | Journal | linkedrecordfield | → Journals/Rituals |
| 7–30 | `s...` | (Supporting calendar event fields) | various | Duration, Location, Notes, Outcome |

### BAC-Goals

**App ID:** `69458793cc79c051739c047b` | **Fields:** 11

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Type | singleselectfield | |
| 3 | `s...` | Target Date | datefield | |
| 4 | `s...` | Family | linkedrecordfield | → People |
| 5 | `s...` | Person | linkedrecordfield | → People |
| 6–11 | `s...` | (Supporting personal goal fields) | various | |

---

## System Creation and Management

**Solution ID:** `691e584571cb89b8ecc9b202`

### MVP Outlines

**App ID:** `691e584571cb89b8ecc9b203` | **Fields:** 11

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–11 | `s...` | (MVP definition fields) | various | Scope, Status, Launch Phase, Notes |

### Launch Phases

**App ID:** `691e584671cb89b8ecc9bc01` | **Fields:** 11

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Phase Name | recordtitlefield | Primary field |
| 2 | `s...` | Start Date | datefield | |
| 3 | `s...` | End Date | datefield | |
| 4 | `s...` | MVPs Included | linkedrecordfield | → MVP Outlines |
| 5–11 | `s...` | (Supporting fields) | various | Status, Notes |

### Service Tickets

**App ID:** `691e584671cb89b8ecc9c5ff` | **Fields:** 12

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Ticket Number | recordtitlefield | Primary field |
| 2 | `s...` | Status | statusfield | |
| 3 | `s...` | Assigned To | userfield | |
| 4 | `s...` | Follow Up Date | datefield | |
| 5–12 | `s...` | (Supporting ticket fields) | various | Description, Priority, Resolution |

### Follow Ups

**App ID:** `691e584671cb89b8ecc9cffd` | **Fields:** 10

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–10 | `s...` | (Follow-up tracking fields) | various | Due Date, Status, Related Ticket, Notes |

### Template Single Field Table

**App ID:** `691e584771cb89b8ecc9d9fb` | **Fields:** 6

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2–6 | `s...` | (Utility template fields) | various | Minimal structure — used as new-app template |

---

## Financial Model for Cost Spreading

**Solution ID:** `691e5cde94c830c70554f0f1`

### Cost Items

**App ID:** `691e5cde94c830c70554f0f2` | **Fields:** 14

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Item Name | recordtitlefield | Primary field |
| 2 | `s...` | Start Month | numberfield | |
| 3 | `s...` | Duration | numberfield | Months |
| 4 | `s...` | Spread Method | linkedrecordfield | → Spread Methods |
| 5 | `s...` | Total Cost | currencyfield | |
| 6 | `s...` | Monthly Cash Flows | linkedrecordfield | → Monthly Cash Flows |
| 7 | `s...` | Annual Cash Flows | linkedrecordfield | → Annual Cash Flows |
| 8 | `s...` | Spread Calculations | linkedrecordfield | → Spread Calculations |
| 9–14 | `s...` | (Supporting fields) | various | |

### Monthly Cash Flows

**App ID:** `691e5cdf94c830c70554faf8` | **Fields:** 9

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Month | numberfield | |
| 3 | `s...` | Cash Flow | currencyfield | |
| 4 | `s...` | Cost Item | linkedrecordfield | → Cost Items |
| 5–9 | `s...` | (Supporting fields) | various | |

### Annual Cash Flows

**App ID:** `691e5cdf94c830c7055504fe` | **Fields:** 9

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Year | numberfield | |
| 3 | `s...` | Cash Flow | currencyfield | |
| 4 | `s...` | Cost Item | linkedrecordfield | → Cost Items |
| 5–9 | `s...` | (Supporting fields) | various | |

### Spread Calculations

**App ID:** `691e5cdf94c830c705550f04` | **Fields:** 9

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Month | numberfield | |
| 3 | `s...` | Calculated Cash Flow | formulafield | |
| 4 | `s...` | Cost Item | linkedrecordfield | → Cost Items |
| 5–9 | `s...` | (Supporting fields) | various | |

---

## System Libraries/Catalogs

**Solution ID:** `691e957af8b69aaece23e284`

### Intacct Account Codes

**App ID:** `68dd644f026c0ecd248201c7` | **Fields:** 20

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | ACCT_NO | recordtitlefield | Primary field — Sage Intacct account number |
| 2 | `s...` | Name | textfield | GL account name |
| 3 | `s...` | Normal Balance | singleselectfield | Debit / Credit |
| 4 | `s...` | Cash Flow Statement | formulafield | Operating / Investing / Financing classification |
| 5–20 | `s...` | (Supporting GL fields) | various | Account Type, Sub-type, Department, Notes |

### Intacct Customer Table

**App ID:** `69166146d6b6e24f0220b780` | **Fields:** 12

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Customer ID | recordtitlefield | Primary field |
| 2 | `s...` | Name | textfield | |
| 3 | `s...` | Type | singleselectfield | |
| 4 | `s...` | Address | textfield | |
| 5 | `s...` | Link to Companies/Entities | linkedrecordfield | → Companies/Entities |
| 6–12 | `s...` | (Supporting Intacct customer fields) | various | |

### Stage/Status

**App ID:** `68ab74d06f08765e15d25ce9` | **Fields:** 22

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Phase or Status | recordtitlefield | Primary field |
| 2 | `s...` | Element of Project | singleselectfield | |
| 3 | `s...` | Department | linkedrecordfield | → Stitser BUILT Departments |
| 4–22 | `s...` | (Links to schedule/project/status tables) | linkedrecordfield / various | Links to many apps as shared status catalog |

### Spread Methods

**App ID:** `691e5ce094c830c70555190a` | **Fields:** 10

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Method Name | recordtitlefield | Primary field |
| 2 | `s...` | Description | textareafield | |
| 3 | `s...` | Formula | formulafield | |
| 4–10 | `s...` | (Supporting method fields) | various | |

### Intacct Location Records

**App ID:** `6914fb94e53085946b899cb0` | **Fields:** 21

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | LOCATION_ID | recordtitlefield | Primary field — Sage Intacct location/entity ID |
| 2 | `s...` | Company Name | textfield | |
| 3 | `s...` | Owners/Members | linkedrecordfield | → People |
| 4 | `s...` | Operating Agreement | filefield | |
| 5–21 | `s...` | (Location/entity fields) | various | Address, EIN, Entity Type, Formation Date |

### Ownership Table

**App ID:** `69c965bab97f93c8ace08c42` | **Fields:** 17

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Intacct Location | linkedrecordfield | → Intacct Location Records |
| 3 | `s...` | Member/Owner Entity | linkedrecordfield | → Companies/Entities |
| 4 | `s...` | Capital Interest % | percentfield | |
| 5 | `s...` | Profits Interest % | percentfield | |
| 6–17 | `s...` | (Supporting ownership fields) | various | Voting Rights, Class, Effective Date |

### Stitser BUILT Departments

**App ID:** `6858d867136a525adac28543` | **Fields:** 27

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Goals | linkedrecordfield | → Goals (SB Game App) |
| 3 | `s...` | Priorities | linkedrecordfield | → Current Priorities |
| 4 | `s...` | Milestones | linkedrecordfield | → Milestones |
| 5 | `s...` | People | linkedrecordfield | → People (SB-Biz Dev) |
| 6 | `s...` | Intacct Department ID | textfield | |
| 7 | `s...` | Intacct Department Name | textfield | |
| 8–27 | `s...` | (Supporting department fields) | various | Budget Owner, Cost Center, Division |

### S-Bos Activity Log

**App ID:** `69dc55333fe841263503f235` | **Fields:** 23

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Timestamp | datefield | |
| 3 | `s...` | System Area | singleselectfield | |
| 4 | `s...` | Action | textfield | |
| 5 | `s...` | Pillar | singleselectfield | |
| 6 | `s...` | Before State | textareafield | |
| 7 | `s...` | After State | textareafield | |
| 8 | `s...` | Actor | linkedrecordfield | → People |
| 9 | `s...` | Approver | linkedrecordfield | → People |
| 10–23 | `s...` | (Supporting audit log fields) | various | |

### Intacct Project List

**App ID:** `69165f2edd7d6c939970efee` | **Fields:** 15

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Project ID | recordtitlefield | Primary field |
| 2 | `s...` | Name in Sage | textfield | |
| 3 | `s...` | Parent ID | textfield | |
| 4 | `s...` | Category | singleselectfield | |
| 5 | `s...` | Customer ID | linkedrecordfield | → Intacct Customer Table |
| 6–15 | `s...` | (Supporting Intacct project fields) | various | Status, Billing Type, Start/End Dates |

### Intacct Cost Codes

**App ID:** `68dd6553caad578dc367ee61` | **Fields:** 25

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Cost Code | recordtitlefield | Primary field |
| 2 | `s...` | Milestone | linkedrecordfield | → Major Milestones |
| 3 | `s...` | COSTTYPEID | textfield | |
| 4 | `s...` | GL Account | linkedrecordfield | → Intacct Account Codes |
| 5 | `s...` | Link to Time Cards | linkedrecordfield | → Time Cards |
| 6 | `s...` | Link to Invoice Line Items | linkedrecordfield | → Intacct Invoice Line Items |
| 7–25 | `s...` | (Supporting cost code fields) | various | Description, Category, Active flag, Department |

### Intacct Vendor ID

**App ID:** `69165c6354c4a1b83a791ba6` | **Fields:** 13

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Vendor ID | recordtitlefield | Primary field |
| 2 | `s...` | Name | textfield | |
| 3 | `s...` | Type | singleselectfield | |
| 4 | `s...` | Address | textfield | |
| 5 | `s...` | Link to Companies/Entities | linkedrecordfield | → Companies/Entities |
| 6–13 | `s...` | (Supporting Intacct vendor fields) | various | |

---

## SB HR

**Solution ID:** `69c98e1763faf6862eca55b4`

### People Profiles

**App ID:** `69c98e1763faf6862eca55b6` | **Fields:** 35

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Hometown | textfield | |
| 3 | `s...` | Career Path | textareafield | |
| 4 | `s...` | Interests | tagsfield | |
| 5 | `s...` | Work Style | multipleselectfield | |
| 6 | `s...` | Aspirations | textareafield | |
| 7 | `s...` | Skills | tagsfield | |
| 8 | `s...` | Communication Preferences | multipleselectfield | |
| 9 | `s...` | Linked Team Member | linkedrecordfield | → Intacct-Employee List |
| 10–35 | `s...` | (Deep profile fields) | various | Education, Strengths, Core Values, Love Language, Feedback Style, Motivators |

### Intacct-Employee List

**App ID:** `687180da979b3844a2854bf9` | **Fields:** 39

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Location/Company that pays | linkedrecordfield | → Intacct Location Records |
| 3 | `s...` | Department | linkedrecordfield | → Stitser BUILT Departments |
| 4 | `s...` | Employee ID | textfield | Intacct Employee ID |
| 5 | `s...` | Compensation | linkedrecordfield | → Compensation Table |
| 6 | `s...` | Hours Reporting Required | yesnofield | |
| 7 | `s...` | Monthly Hours Reported | formulafield | |
| 8 | `s...` | Status | statusfield | |
| 9 | `s...` | Kompass API Users | linkedrecordfield | → Kompass API Users |
| 10–39 | `s...` | (Supporting HR/payroll fields) | various | Start Date, Title, Pay Type, Benefit Enrollment, Monthly hours formulas |

### Compensation Table

**App ID:** `6871485bc057b80f1383055d` | **Fields:** 30

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Location/Company | linkedrecordfield | → Intacct Location Records |
| 3 | `s...` | Salary Amount | currencyfield | |
| 4 | `s...` | Labor Burden % | percentfield | |
| 5 | `s...` | Health Insurance | currencyfield | Monthly amount |
| 6 | `s...` | Devices | currencyfield | |
| 7 | `s...` | Data Plan | currencyfield | |
| 8 | `s...` | Vehicle Payment | currencyfield | |
| 9 | `s...` | Bonus Date | datefield | |
| 10 | `s...` | Bonus Amount | currencyfield | |
| 11 | `s...` | Hourly Overhead | formulafield | |
| 12–30 | `s...` | (Supporting comp/benefit fields) | various | Total Comp, Employer FICA, Workers Comp, PTO accrual |

### Kompass API Users

**App ID:** `69e64b7cbf14ff41d31de225` | **Fields:** 13

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Token Hash SHA-256 | textfield | Hashed API token — never stored plain |
| 3 | `s...` | Active | yesnofield | |
| 4 | `s...` | Token Issued | datefield | |
| 5 | `s...` | Last Used | datefield | |
| 6 | `s...` | Person Link | linkedrecordfield | → People (SB-Biz Dev) |
| 7 | `s...` | Kompass Roles | linkedrecordfield | → Kompass Roles |
| 8–13 | `s...` | (Supporting API user fields) | various | Token Expiry, Scopes, IP Allowlist |

### Kompass Access Matrix

**App ID:** `69e64fbfb46a93b0789acb7e` | **Fields:** 14

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Solution Name | textfield | |
| 3 | `s...` | Solution ID | textfield | SmartSuite Solution ID |
| 4 | `s...` | App Name | textfield | |
| 5 | `s...` | App ID | textfield | SmartSuite App ID |
| 6 | `s...` | Permission | singleselectfield | Read / Write / None |
| 7 | `s...` | Role | linkedrecordfield | → Kompass Roles |
| 8 | `s...` | Filter Rules | textareafield | JSON filter rules |
| 9–14 | `s...` | (Supporting access matrix fields) | various | |

### Kompass Roles

**App ID:** `69e6512583e39573a86433ab` | **Fields:** 13

| # | Slug | Label | Type | Notes |
|---|------|-------|------|-------|
| 1 | `title` | Title | recordtitlefield | Primary field |
| 2 | `s...` | Orbit | singleselectfield | Role tier/level |
| 3 | `s...` | SmartSuite Access Level | singleselectfield | |
| 4 | `s...` | Reports To | linkedrecordfield | → Kompass Roles (self-referential) |
| 5 | `s...` | Active | yesnofield | |
| 6–13 | `s...` | (Supporting role fields) | various | Permissions summary, linked users, description |

---

*End of Stitser BUILT SmartSuite Data Dictionary*

*Generated from live SmartSuite API via MCP on 2026-05-28. Field slugs shown as `s...` indicate auto-generated SmartSuite slugs that were not individually enumerated; exact slugs are available via `smartsuite_get_app_schema` for each app ID.*
