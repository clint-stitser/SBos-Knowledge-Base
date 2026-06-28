# The Operator Assistant — Kompass

## What It Is

The central interface is a conversational LLM that acts as the operator's Chief of Staff. It is not a chat window — it is a domain-trained assistant that has persistent access to the Brain and can execute skills.

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
