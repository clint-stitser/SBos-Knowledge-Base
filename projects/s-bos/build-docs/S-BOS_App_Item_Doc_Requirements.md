# S-BOS App Item — Required-Doc Coverage by Kind

> **Purpose:** Defines, for each **Kind of App Item**, which of Ryan's project-level docs must cover it before it's build-ready. This is the source of the **"Build Docs" checklist** attached to every App Item on the roadmap.
> **Use:** When a new App Item is created (or an existing one is being readied for build), generate its checklist from the row matching its Kind.

---

## The principle (why there's no "doc set per module")

Ryan's system is a **single project-level doc set** — one PDD, one DB Schema, one Tech Spec, etc. for the whole project. A module/feature does **not** get its own folder of docs. Instead it is:

1. **decomposed as an `M-XX` entry in the Module Breakdown**, and
2. **represented as rows/sections inside the shared docs** (e.g., a Feature appears as entries in the API Contract, Component/Service Map, and Testing Strategy, all keyed to its `M-XX` id).

So "what docs does this item need?" really means **"which shared docs must contain a section/row covering this item before it can be built."** That coverage list depends on the item's **Kind**. This doc defines it.

---

## App Item Kinds (SmartSuite field `sf7ec194d1`)

| Kind | Code | Meaning |
|---|---|---|
| Feature | `qrt21` | User-facing capability (a screen/flow the team uses) |
| Workflow | `WkstT` | Automation / multi-step process |
| Data Layer | `EOw0c` | Schema, tables, computed fields, data rules |
| Integration | `n89Yl` | External system / MCP / API connection |
| Skill/Instruction | `Nn6M0` | A Claude skill |
| Training/Education | `JGMYu` | Team-facing how-to / enablement |

---

## Required-doc coverage by Kind → checklist tasks

**Baseline (every Kind):** Module Breakdown entry · Build Decisions Log (ongoing) · Progress Checklist entry.

| Kind | Required doc coverage (these become the checklist tasks) |
|---|---|
| **Feature** | PDD (Feature + Workflow entry) · UI/UX (screens) · UI Strings · API Contract · Component/Service Map · Module Breakdown · Testing Strategy (UT/IT/E2E) |
| **Data Layer** | DB Schema · Sample Data · Database Migration Checklist · Decisions Log (ADR) · Testing Strategy (data/constraint tests) |
| **Integration** | Tech Spec — Dependencies & Integrations · Tech Spec — Events & Side Effects · API Contract · Decisions Log (ADR) · Deployment Config (env/secrets) · Testing Strategy (integration tests) |
| **Workflow** | Tech Spec — Events & Side Effects + state machine · Workflow Diagram · Module Breakdown · Testing Strategy |
| **Skill/Instruction** | SKILL.md · API Contract (tools it calls) · Decisions Log (ADR) |
| **Training/Education** | Training doc · UI Strings · UI/UX (if it has a surface) |

> A given item adds the **state-machine** sections of the Tech Spec whenever it has a `status`/lifecycle with 3+ states (per the PDD red flags), regardless of Kind.

---

## How this maps to the roadmap (live)

- Each App Item is a **Project record** (IT/Systems dept) on the v2.4 roadmap, tagged with its Kind (`sf7ec194d1`), Stages (`sfdac4b613`), and Inclusions (`s1dbcd776a`).
- Each App Item has a **Check List** ("<Name> — Build Docs") whose **tasks are the rows above** for that item's Kind.
- Completing a task = that shared doc now contains the section/row covering this item. When all tasks are done, the item is **doc-complete** and ready for the Coding Kickoff gate.

**Created 2026-06-17:** 6 App Items + 6 checklists + 38 tasks (Features ×7, Integrations ×6, Data Layer ×5).

---

## Maintaining this

If a Kind's coverage list changes, update the table here first, then regenerate affected checklists. This doc is the single source for the Kind→docs convention — the roadmap checklists are derived from it.
