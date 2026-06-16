# Architecture Decision Log: [App Name]

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized (light pass — design-phase log)
> **Fill order:** Continuously populated during design phase as architecture decisions are made. Read continuously during build to look up "why did we do it this way?"
>
> **Source docs:** Whichever design doc is being filled when a decision surfaces. Most AD-XX entries surface during Tech Spec, DB Schema, or UI/UX work.
>
> **Downstream docs that consume this one:**
> - `[AppName]_Build_Decisions_Log.md` — BD ⚖️ Reconciliation entries may reference AD-XX when a build-phase reconciliation revisits a design-phase architecture decision.
> - `[AppName]_Mid_Build_Review_[N].md` — Section 5 (Open Questions Answered During Coding) may need a new AD-XX entry to formalize an informal architectural decision Ryan made in chat.
> - **Future readers** (onboarding, V2 design) — read this log to understand the rationale behind architecture choices that aren't obvious from the code alone.
>
> **Agent role:** When a design decision is being made with real architectural consequence, propose an AD-XX entry. The agent does NOT invent decisions — every entry traces to a real discussion or design-doc edit. The agent does NOT close an entry without a clear resolution (decision chosen, alternatives rejected, rationale stated).
>
> **The three rules while maintaining this log:**
> 1. **Log it if any of these are true:** multiple real options were considered; there's a compliance/legal/cost reason; the decision contradicts a common default; the decision will be invisible from the code alone. If none of these are true, it's probably not an AD — it's just a design choice that lives in the relevant design doc.
> 2. **Capture rejected alternatives.** A decision without its rejected alternatives is just a record of what we did, not WHY. The rejected alternatives are what protect future readers from re-litigating the same choice.
> 3. **Reference AD-XX from code or other docs.** When the decision is implemented, code comments and Tech Spec sections reference the AD-XX. Without back-references the log is an orphan.
>
> **Not to be confused with:** `[AppName]_Build_Decisions_Log.md`. This log captures **design-phase architecture decisions** (made before coding starts). Decisions, workarounds, deviations, gaps, and concerns that occur *during* build go in the Build Decisions Log. If you're not sure which one applies: are you designing or building? Designing → here. Building → Build Decisions Log.
>
> **When this doc is fully filled and approved:** Remove every `🤖 AGENT INSTRUCTIONS` block. Keep every AD entry — the log is permanent reference for the project's lifetime.

**Purpose:** Record key design and architecture decisions (ADRs) with rationale and rejected alternatives, made during the design phase.

**Update:** Whenever a non-obvious design decision is made — especially anything with compliance, cost, or architectural consequences.

**Use:** Audit trail, onboarding reference, and "why did we do it this way" lookup.

---

## Status & Next Steps

| Section | Status | Owner | Notes |
|---------|--------|-------|-------|
| Decision Log | ⏳ Not Started | — | Add first entry when a real design decision is made |

> **Status values:** ⏳ Not Started / 🔄 In Progress / ❓ Needs Discussion / ✅ Done (Design Doc scheme — this log is filled during design phase)

---

## How to Use

- Add a row per decision — don't delete old entries, ever
- Link to the relevant doc section where the decision lives (e.g., `Tech_Spec.md § Authentication`)
- Reference an entry from code or other docs as `AD-01`, `AD-02`, etc.

**Log it if any of these are true:**
- Multiple real options were considered
- There's a compliance or legal reason
- It contradicts a common default (e.g., chose X over the obvious Y)
- You'll forget the reason in 6 months

**Don't log:**
- Obvious defaults with no real alternatives ("we used Postgres")
- Personal preference with no tradeoffs
- Anything where the rationale is self-evident from the doc

---

## Architecture Decision Log

| ID | Date | Decision | Rationale | Alternatives Rejected | Doc Reference | Owner |
|----|------|----------|-----------|-----------------------|---------------|-------|
| AD-01 | YYYY-MM-DD | [What was decided] | [Why this choice was made] | [What else was considered and why it was rejected] | [Link or section] | [Ryan / Claude / Team] |

---

## Entry Template

Copy this block for each new decision:

```
### AD-XX — [Short Title]

- **Date:** YYYY-MM-DD
- **Owner:** Ryan / Claude / Team
- **Status:** Active / Superseded by AD-XX

**Decision:**
[One or two sentences describing exactly what was decided.]

**Rationale:**
[Why this was the right call. What constraints, requirements, or tradeoffs drove it.]

**Alternatives Rejected:**
- **[Option A]:** [Why it was ruled out]
- **[Option B]:** [Why it was ruled out]

**Compliance Note:** *(if applicable)*
[Any IRS, legal, or regulatory reason this decision was made or constrained.]

**Doc Reference:**
[e.g., `Technical_Spec.md § Authentication` or `DB_Schema.md § Users table`]
```

---

## Decisions

*(Add entries below — oldest at top, newest at bottom)*
