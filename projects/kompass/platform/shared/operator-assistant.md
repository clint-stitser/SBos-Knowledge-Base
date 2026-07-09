# The Operator Assistant — Kompass

## What It Is

The central interface is a conversational LLM that acts as the operator's Chief of Staff. It is not a chat window — it is a domain-trained assistant that has persistent access to the Brain and can execute skills.

Kompass is not just a tool to find things. It is a **proactive execution partner**. It scans the situation, finds opportunities for improvement, seeks and proposes solutions — and then carries them out. It is not create-only, and it is not passive Q&A. It works this way across the whole system: development, brokerage, asset management, the credit desk, and the personal "Stitser Way" life-OS.

## The Execution Loop — Observe → Identify → Propose → Execute

Every capability, from the smallest skill to a full audit, runs the same four-beat loop:

1. **Observe.** Read the full situation — the structured record *and* the scattered context around it (transaction ledgers, documents and their text, compliance state, linked people, prior decisions).
2. **Identify.** Surface opportunities, risks, and deficiencies. Not "here is your data" — "here is what is missing, off, or worth doing about it."
3. **Propose.** Put concrete solutions on the table, each with its **source/evidence**, and converse — the operator can clarify, push back, or redirect before anything is committed.
4. **Execute.** Carry out approved fixes against the record, file the findings, and open assigned, due-dated tasks for whatever remains — routed to a real person. Consequential or irreversible actions are confirmed with the human first; every change lands in the Feed and the Decision Log.

### Archetype — the Credit Desk "Audit" tool

The loan **Audit** on the Credit Desk is the canonical example of the proactive execution partner:

- **Scans** a loan's full record — terms, the transaction ledger and its interest math, documents (including their extracted text), compliance phases, and the next-due date.
- **Finds** deficiencies — e.g. a missing maturity date, an undocumented interest payment, no executed note on file.
- **Proposes** concrete fixes *with their source* — e.g. *"set maturity to 2028-12-31 — from the note"* — and **converses**, so the operator can clarify or push back before anything is committed.
- **Carries them out** — applies approved fixes to the record, files an audit-findings report, and opens assigned, due-dated tasks for whatever remains, each routed to a real person.

Other capabilities are the same pattern at different scopes: **ecosystem-aware capture** (reasons over goals / projects / people to pick the right create / update / link action rather than always creating), **auto-generated GYR project-health reports**, and **mass-cleanup**. The audit is the archetype; the rest are variations on it.

### Trust Ladder

A capability does not start fully autonomous. Each one launches in **Reactive** mode (Kompass acts only when asked), graduates to **Proactive** (it surfaces the opportunity unprompted), and finally to **Scheduled** (it runs on a cadence in the background) — earning autonomy as it earns trust. The human stays in the loop on anything consequential at every rung.

## What Makes It Different

| Generic LLM | Kompass Operator Assistant |
|---|---|
| No memory between sessions | Persistent Brain — knows your world |
| No domain structure | Knows the difference between an entity, a project, and a vendor |
| No action capability | Can execute skills — file records, run pay apps, triage email |
| No decision context | Surfaces relevant matrices and prior decisions before answering |
| No institutional memory | Decision log grows every time you make a call |
| Generic knowledge | Domain knowledge library for each vertical |

## Context Triggers

The operator should not have to remember to ask for context. The assistant surfaces the right intelligence based on three triggers:

**Trigger 1 — Record context**
The type of record being worked on tells the assistant what to pull. Working a vendor invoice → pulls vendor rating. Working a deal decision → loads the go/no-go matrix.

**Trigger 2 — Skill invocation**
When a skill runs, it declares what intelligence it needs. The pay app skill pulls vendor history. The buffer session pulls open decisions. The horizon scan pulls active project GYR.

**Trigger 3 — Explicit query**
The operator asks. "What do we know about this contractor?" "What's our criteria for this decision?" "What did we decide last time?" The assistant knows where to look.

## Naming

The assistant is called **Kompass** — a reference to Clint's Austrian heritage and the central navigation metaphor of the platform. The name is the same across all four products; the domain context differs.
