# Weekly Goal Review by Product Line

## Header
| Field | Value |
|---|---|
| **Workflow ID** | mgmt-all-weekly-001 |
| **Product Lines** | All (Construction live — Disposition, Asset Management, Development, Brokerage planned) |
| **Situation** | Ongoing management — runs throughout active scoring period |
| **Shared or Specific** | Shared framework; product-line-specific instances |
| **Owner Role** | Principal / Product Line Lead |
| **Current Owner** | Clint Stitser |
| **Status** | Live — Construction active as of June 4, 2026 |
| **Last Updated** | June 4, 2026 |
| **Version** | 1.0 |

---

## Overview

This workflow produces a weekly GYR (Green/Yellow/Red) Status Report for each active product line, filed automatically in S-BOS against the 2026 Revenue Target goal. The report combines live dashboard data with S-BOS activity from the past seven days and a Claude-generated narrative to produce a point-in-time performance journal entry.

The output lives in two places: a GYR Status Report record in SmartSuite (linked to the goal and priority records) and an HTML snapshot file stored in Google Drive. Together these form the permanent, searchable record of how each product line performed week-over-week throughout the scoring period.

The workflow is automated via CoWork. No manual reporting is required beyond keeping S-BOS current before 2 PM each Monday.

---

## Trigger

- **Recurring:** Every Monday, automated
- **12:00 PM PT** — Reminder email sent to product line team: complete all S-BOS updates before 2 PM
- **2:00 PM PT** — CoWork runs the full GYR workflow automatically

Manual trigger: In CoWork, say "run the construction GYR report" (or equivalent for other product lines) to generate an ad-hoc report at any time.

---

## System Components

### Skills (Playbooks)
A **skill** is a set of instructions stored as a Markdown file that tells Claude *how* to do something. It is the detailed workflow — step by step, with all field slugs, logic, and templates embedded.

| Skill | Purpose |
|---|---|
| `construction-gyr-reporter` | Full 8-step GYR workflow for the Construction product line |
| `construction-reminder` | Monday 12 PM email to the construction team |

Skill files live at:
```
~/Library/Application Support/Claude/
  local-agent-mode-sessions/.../outputs/
    construction-gyr-reporter/
      skills/
        construction-gyr-reporter/SKILL.md    ← main workflow
        construction-reminder/SKILL.md         ← reminder email
        construction-gyr-reporter/references/
          field-map.md                          ← all SmartSuite IDs
          gyr-status-logic.md                   ← G/Y/R computation
          html-template.md                      ← HTML snapshot format
```

### Routines (Timers)
A **routine** is a scheduled trigger that fires at a set time and runs a prompt. The prompt tells CoWork which skill to execute. Routines are dumb — they just wake up and say "go do this." Skills are smart — they know every step.

| Routine | Schedule | What It Does |
|---|---|---|
| `construction-gyr-reminder` | Monday 12:00 PM PT | Sends team email |
| `construction-gyr-report` | Monday 2:00 PM PT | Runs full 8-step workflow |

Routine files live at: `~/.claude/scheduled-tasks/`

To find files in Finder: press `Cmd + Shift + G` and paste either path above.

---

## Steps (What Happens at 2 PM)

### Step 1 — Fetch Live Dashboard Data
| Field | Value |
|---|---|
| **Who** | CoWork (automated) |
| **What** | Calls the Railway API for the product line's dashboard data |
| **Tool / System** | Railway backend (`sb-planning-tools-production.up.railway.app`) |
| **Endpoint** | `/api/construction-data` (Construction) |
| **Output** | projects[], score{bucketA, bucketB, bucketC, projectedTotal}, scoringPeriod |

**Score buckets:**
- **[A]** G-702 billings submitted within the scoring window — hard actuals
- **[B]** WIP Balance to Finish × time proration to Dec 31 — firm estimate (progress billing)
- **[C]** Pipeline contract value × billing fraction × confidence rating — soft estimate

---

### Step 2 — Compute GYR Status
| Field | Value |
|---|---|
| **Who** | CoWork (automated) |
| **What** | Computes pace vs. expected progress given elapsed time in scoring period |
| **Formula** | pace = projectedTotal ÷ (goal × % of period elapsed) |

| Pace | Status | SmartSuite Value |
|---|---|---|
| ≥ 110% | Exceeding Target | `complete` |
| 90–109% | On Track | `backlog` |
| 75–89% | At Risk | `in_progress` |
| < 75% | Critical | `ready_for_review` |

---

### Step 3 — Pull S-BOS Activity (Last 7 Days)
| Field | Value |
|---|---|
| **Who** | CoWork (automated) |
| **What** | Reviews what changed in S-BOS in the past week |
| **Tool / System** | SmartSuite MCP |
| **Apps Checked** | Projects, Baseline Budget Items, G-702, Notes & Comments |
| **Output** | Summary of status changes, G-702 submissions, pipeline entries, notes added |

---

### Step 4 — Write the Narrative
| Field | Value |
|---|---|
| **Who** | Claude (AI-generated) |
| **What** | Writes a structured GYR narrative using dashboard data + S-BOS activity |
| **Output** | Under 400 words, covering: On Track / Needs Attention / Off Track / This Week in S-BOS / Recommended Action |
| **SmartSuite Field** | `s675abeba3` (System GYR Evidence) on GYR Status Report record |

The narrative names specific jobs, dollar amounts, and days remaining. It does not soften bad news. Missing data (no G-702s linked, unrated pipeline jobs, stale budgets) is flagged as a signal in itself.

---

### Step 5 — Generate HTML Snapshot
| Field | Value |
|---|---|
| **Who** | CoWork (automated) |
| **What** | Generates a styled HTML file with narrative + metrics + job table |
| **Format** | Dark-background dashboard style, includes actuals vs. goals table, situation breakdown, active jobs sorted by contribution |

---

### Step 6 — Upload to Google Drive
| Field | Value |
|---|---|
| **Who** | CoWork (automated) |
| **What** | Uploads the HTML snapshot to Drive |
| **Tool / System** | Google Drive MCP |
| **Folder Path** | `My Drive / Goal Tracking / {Product Line} / GYR Reports /` |
| **Filename** | `{ProductLine}-GYR-W{weekNum}-{YYYY-MM-DD}.html` |
| **Output** | Shareable Drive link for the file |

---

### Step 7 — Create GYR Status Report in SmartSuite
| Field | Value |
|---|---|
| **Who** | CoWork (automated) |
| **What** | Creates a GYR Status Report record linked to the goal and priority |
| **Tool / System** | SmartSuite (GYR Status Reports app `68216f48f98789b5bb095a51`) |

Key fields written:

| Field | Slug | Value |
|---|---|---|
| GYR Status | `s3638e84d5` | Computed G/Y/R status |
| System GYR | `s8ow7due` | Same — machine-computed |
| System GYR Evidence | `s675abeba3` | Claude narrative |
| System GYR Date | `se3873553c` | Today |
| Link to Annual Goals | `sfwf9528` | Goal record ID |
| Current Priority | `s3511304b0` | Priority record ID |

---

### Step 8 — Log to Activity Log
| Field | Value |
|---|---|
| **Who** | CoWork (automated) |
| **What** | Adds an entry to the S-BOS Activity Log with the report summary and links |
| **App** | S-Bos Activity Log (`69dc55333fe841263503f235`) |

---

## Key S-BOS Records (Construction)

| Item | Record ID | App ID |
|---|---|---|
| BUILT. 2026 Revenue Target (Goal) | `698b7239aac6a0dc52279428` | `6824d4d1885a8769bd2dfc0d` |
| Construction/Development: Complete Projects (Priority) | `698b72593f3ed73d2981c738` | `68216f48f98789b5bb095a4b` |
| GYR Status Reports app | — | `68216f48f98789b5bb095a51` |
| Activity Log app | — | `69dc55333fe841263503f235` |

---

## How to Add a New Product Line

1. Identify the Goal record and Priority record IDs in SmartSuite for the new product line
2. Identify the dashboard API endpoint (e.g., `/api/disposition-data`)
3. Determine the scoring model:
   - **Progress billing** (Construction): revenue accrues daily from mobilization to Dec 31
   - **Event-based** (Disposition): revenue recognized on single close event
   - **Periodic** (Asset Management): NOI recognized monthly
4. Duplicate the skill folder and update all IDs, endpoint, and scoring logic
5. Create two new Monday routines in CoWork (12 PM reminder, 2 PM report)
6. Create the Drive folder: `My Drive / Goal Tracking / {Product Line} / GYR Reports /`

---

## Maintenance

| What | When | How |
|---|---|---|
| Update field slugs | If SmartSuite schema changes | Edit `references/field-map.md` in the skill |
| Update goal/priority IDs | New year or new goal | Edit `references/field-map.md` |
| Update scoring period dates | New scoring period | Edit `SCORING_PERIOD` in `server.js` on Railway |
| Change email recipients | Team changes | Edit `skills/construction-reminder/SKILL.md` |
| Change schedule time | As needed | Edit via Scheduled sidebar in CoWork |

---

## Troubleshooting

| Symptom | Likely Cause | Fix |
|---|---|---|
| "Snapshot failed" error | Body size limit hit or API timeout | Re-run manually; if persistent, check Railway server status |
| GYR record not created | SmartSuite field slug changed | Check field-map.md against live schema |
| Drive upload failed | Drive MCP not authorized | Re-authenticate Drive in CoWork connectors |
| Routine didn't fire | CoWork was closed at scheduled time | Runs on next launch; click "Run now" to execute immediately |
| Score shows $0 | No G-702s linked to budget rows | Expected early in period; score will update as pay apps are submitted |

---

## Related Documents
- `dashboards/PRINCIPLES.md` in `sb-planning-tools` repo — dashboard design principles governing all product line scoreboards
- `docs/cowork-gyr-reporter-knowledge.md` in `sb-planning-tools` repo — technical reference (file paths, all field slugs, detailed architecture)
