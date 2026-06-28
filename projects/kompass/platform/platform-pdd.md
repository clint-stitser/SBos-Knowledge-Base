# Kompass Platform Design Document
## Version 1.0 — June 2026
### Stitser BUILT / S-BOS Infrastructure

---

## Purpose of This Document

This PDD defines the architecture, product structure, user experience, and intelligence model for the **Kompass** platform family — a suite of niche-specific AI operating systems for real estate professionals. It serves as the north star reference for building, teaching, and eventually productizing S-BOS for audiences beyond Stitser BUILT.

This document should be read alongside:
- `06-platform-design/kompass-operating-platform.md` — the S-BOS technical PDD
- `03-stitser-way/protocols-overview.md` — the Stitser Way protocol library
- The MarketingSecrets.ai documentation — the closest public analog to this product category

---

## The Core Thesis

MarketingSecrets.ai built a horizontal AI Chief of Staff for bootstrapped marketers. Their moat is the **Brain** — persistent business context that accumulates across every conversation, import, and integration, and makes every future interaction more intelligent.

Kompass is the vertical version of that thesis — built for real estate operators instead of marketers. The difference is not just niche targeting. It is **domain-structured intelligence**.

A marketer's brain needs to know: offer, audience, funnel, competitors, voice.  
A developer's brain needs to know: entities, capital stack, entitlement status, vendor track record, go/no-go criteria, deal history.  
A contractor's brain needs to know: job cost by trade, subcontractor performance, change order patterns, schedule risk, pay app status.  
An agent's brain needs to know: client profiles, listing pipeline, disclosure requirements, commission structure, market data.

The structure of the memory is the product. Generic memory compounds slowly. Domain-structured memory compounds fast — because the LLM knows not just *what* to remember, but *where to put it* and *when to surface it*.

**Tagline:** *Your operating system for real estate — built to know your world and run your business.*

---

## The Four Products

See each product folder for full design detail.

- `developer/` — Developer Kompass
- `contractor/` — Contractor Kompass
- `agent/` — Agent Kompass
- `personal/` — Personal Kompass

---

## Shared Architecture

See `shared/` for the complete shared architecture documentation:

- `shared/brain-model.md` — The Brain: domain-structured memory
- `shared/intelligence-stores.md` — The six intelligence stores
- `shared/operator-assistant.md` — Kompass: the AI operator assistant
- `shared/feed-model.md` — The Feed: the assistant's outbox
- `shared/skills-universal.md` — Universal skills (shared across all products)
- `shared/routines-universal.md` — Universal routines
- `shared/onboarding.md` — Onboarding and Brain Setup Score
- `shared/ux-principles.md` — User experience principles

---

## Productization Path

| Phase | Description | Status |
|---|---|---|
| Phase 1 — Internal | S-BOS for Stitser BUILT. Principal + core team. | Active |
| Phase 2 — Operator Beta | Select external operators, one product (Developer first) | Planned |
| Phase 3 — Packaged Product | Standalone offering with documented onboarding, starter skills/routines | Future |
| Phase 4 — Platform | Operators build and publish their own skills; shared knowledge library | Future |

---

## Competitive Reference

See `research/marketingsecrets-analysis.md` for the full comparison to MarketingSecrets.ai and the key design decisions it informed.

---

*Document owner: Clint Stitser / Stitser BUILT*  
*Last updated: June 2026*  
*Status: Active development — Phase 1*
