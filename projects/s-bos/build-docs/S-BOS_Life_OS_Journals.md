# S-BOS 2.0 — Life OS: The Stitser Way Journaling

**Doc class:** design · **Captured:** 2026-07-04 · **Status:** built (migration 048); skill re-pointed to Supabase; 453 historical journals imported

## Context
The same platform runs the business *and* life (The Stitser Way): morning/evening rituals, weekly route reviews, structured "stacks" to process emotions and decisions, and the Spiraling process (real facts → raw feelings → relevant plans → results achieved) tied to goals/progress.

## Decision — radically simplify the journal
The old SmartSuite journal table carried dozens of form-specific fields because it had to render specific guided-journal forms. With AI now walking the user through the ritual/stack from a **markdown instruction set** and capturing the response, **the journal is just markdown.** So the table is deliberately minimal.

`journals` (migration 048):
- `journal_type` — Morning/Evening Ritual, Weekly Review, Irritation/Anger/Gratitude/etc. Stack, Decision, Discovery, Principle, Free Write…
- `journal_date`, `title`, **`body_md`** (the structured journal), `written_by`, `entity_id` (personal/family/business), `support_files` jsonb, `metadata` jsonb, `status`, `smartsuite_record_id`.

The Spiraling sections are headings **inside** `body_md`, produced by the AI per type — not columns. `journal_type` selects which MD instruction template the skill follows.

## Universal connection
`journal_links` (polymorphic `parent_type` + `parent_id`) connects a journal to any spine object — project, goal, milestone, priority, department, person, entity — mirroring the `tasks.parent_type/parent_id` pattern. A morning ritual can link to the goals it advances; a project debrief links to the project.

## Process layer (the skill)
The `stitser-way` skill holds the walkthrough instructions and currently files to SmartSuite. Migration off SmartSuite = point the skill's create-record step at this `journals` table (+ `journal_links`) instead. The MD-instruction-per-type approach means new ritual/stack types are added by writing an instruction template, not by altering schema.

## Boundaries
Time-like personal activities (a kid logging hours for allowance) are **time_entries**, not journals — kept separate and obvious. Journals are reflective/structured writing; no forced convergence with the stats scoreboard.

## Next
Point the skill at Supabase; build a review UI (browse by type/date/linked object); optionally backfill historical journals from SmartSuite.
