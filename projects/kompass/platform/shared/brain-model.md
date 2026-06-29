# The Brain — Domain-Structured Memory

## What It Is

The Brain is the moat. It is the accumulated, organized intelligence about the operator's world — the thing that makes every Kompass interaction more useful than a generic chat session.

Unlike a flat key/value memory store, the Kompass Brain is **hierarchically structured** around the operator's actual domain model.

---

## Two Parallel Tracks

The Brain holds two distinct layers of work. They can connect — but don't have to.

### Strategic Layer — Working ON the Business
Where you're going. The compass. Broken down from vision into executable sprints.

```
Business / Person / Teams
  └── Goal
        └── Phases / Priorities / Sprints
              └── Milestones / Mini-Sprints
```

### Operational Layer — Working IN the Business
What needs to get done. The chores. Projects exist independently — they don't require a strategic parent to justify their existence.

```
Projects
  └── Outcome Item / Phase
        └── Tasks · Budget · Schedule · Checklists · People
```

---

## How the Layers Connect — Roll-Up, Not Parent-Child

Projects are **not owned by** priorities or milestones. They **contribute to** them.

The difference matters:

| Parent-Child (wrong) | Roll-Up (correct) |
|---|---|
| Project *belongs to* a Priority | Project *contributes to* a Priority |
| Delete the Priority → project is orphaned | Priority is just a lens that aggregates |
| A project can only serve one Priority | A project can contribute to multiple Priorities across time |
| Rigid, hierarchical | Flexible, additive |

**Example:** A Q1 Revenue priority doesn't own the projects generating that revenue. It watches them and adds up what lands in Q1. A single project might generate revenue in Q1 *and* Q2 — it contributes to both priorities without belonging to either.

The link between layers is a **tag or contribution flag** on the project record — not a structural dependency.

```
STRATEGIC LAYER
Goal → Priorities / Sprints → Milestones
              ↑
         aggregates contributions from
              ↓
OPERATIONAL LAYER
Projects → [optionally tagged to a Priority]
  └── Tasks · Budget · Schedule · Checklists · People
```

---

## For a 3rd Grader

Your goal is to save $100 by summer. Your Q1 priority is saving $25 by April.

You mow three neighbors' lawns. Each job pays money across different weeks — some in March, some in April, some in May. The lawn jobs aren't *owned by* Q1. They just *count toward* Q1 when the money lands in that window.

The jobs exist whether or not you're tracking the $25 goal. The goal just watches and adds up what comes in.

---

## How the Brain Grows

1. **Conversation** — facts extracted from every chat, filed automatically
2. **Quick Capture** — voice notes, share sheet inputs, on-the-go entries
3. **Imports** — documents, transcripts, PDFs, CSV exports
4. **Skill execution** — every skill run writes what it found back to the Brain

---

## Brain Setup Score

A score that shows the operator how complete their context is and what to fill in next. Higher score = sharper, more autonomous assistant responses.

Dimensions scored:
- Entity and project data completeness
- People profiles (team + key counterparties)
- Vendor ratings (% of active vendors rated)
- Decision matrices (% of recurring decision types configured)
- Knowledge library (% of domain areas seeded)
- Integration connections (Gmail, Drive, Smartsheet, QuickBooks)

---

## Tech Stack

- **Primary data layer:** SmartSuite (Entity → Strategic Layer + Operational Layer, linked by tag)
- **Reference docs and context:** GitHub (`clint-stitser/Clint-s-Kompass`)
- **File storage:** Google Drive (linked to project records)
- **Integrations:** Gmail, Smartsheet, QuickBooks, Oura, Strava
