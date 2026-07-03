# S-BOS 2.0 — Multi-Entity Tenancy & Relationship Access Model

**Status:** Adopted · 2026-07-03
**Applies to:** People (contacts), Companies, Projects, Goals, and all relationship activity (comments, notes, call history, tasks, deals)
**Audience:** Future S-BOS users, licensees, and anyone extending the data model

---

## Why this exists

S-BOS 2.0 is built to be **licensed to multiple entities** — Stitser companies today, third-party firms tomorrow (e.g. a brokerage and its agents). A single contact can matter to more than one entity at the same time, and each entity must be able to nurture that relationship **privately**, without leaking its notes, deals, or activity to the others. This document defines how access, ownership, and visibility work so that stays true at any scale.

The whole model rides **one structure — an entity tree** — reused for three things at once:
- **Family privacy** (kids as sub-entities under a parent trust)
- **Brokerage tenancy** (agents as sub-entities under a brokerage)
- **Product-line separation** (BUILT vs. Realm vs. Stitser Properties)

One tree, not three systems.

---

## The three-part model

Everything reduces to a clean separation:

| Layer | Scope | Examples |
|---|---|---|
| **The person** | **Shared** — one record | Name, email, phone, tags, audience. There is only one "Bradley Watson." |
| **The relationship** | **Per-entity** | Who's assigned, who collaborates, stage, source. BUILT's rep and Realm's rep are different people on the same contact. |
| **The activity** | **Entity-scoped** | Comments, notes, call history, tasks, deals/project links — owned by the entity that created them. |

**Plain-English consequence:** a Realm deal involving a person does **not** appear when a BUILT rep opens that person's detail. They see the shared identity and BUILT's own history — nothing of Realm's.

---

## The entity tree

- A **license covers an entity.** That entity is the **root**; the account's primary entity sits at the top.
- **Sub-entities** (kids, affiliated companies, agents under a brokerage) hang beneath the root as children.
- Modeled as a self-referential hierarchy: `entities.parent_entity_id`.
- **Membership = the roster.** A person linked to an entity (`person_entities`) is a **member** of it. This is the roster; no separate table.

## The two rules

**Rule 1 — Visibility flows *down*.**
A viewer sees data for their entity **and every descendant** — never siblings, never up.
- Brokerage (parent) sees all its agents' contacts and activity.
- Agent A does **not** see Agent B's (siblings are blind to each other).
- The top of the license sees everything beneath it.

**Rule 2 — Assignment is the owning entity's right, scoped to its roster.**
On any relationship, `assigned_to` and `collaborators` must be **members of the entity that owns that relationship**. The parent can *see* a sub-entity's relationship, but the sub-entity decides who owns it, from its own roster.

---

## Worked examples

**Two agents in the same brokerage list the same person.**
→ One `people` record. Two `relationship_assignments` rows — same `contact_id`, each `entity_id` = that agent's sub-entity, each with its own `assigned_to`. The person exists once; the relationships are separate and private to each agent. The brokerage (parent) sees both.

**A commercial client hires BUILT, then hires Realm to build their house.**
→ One `people` record. Two relationship rows: `(client, BUILT, …)` and `(client, Realm, …)`. Each product line nurtures independently with its own notes/stage/deals. A BUILT rep never sees the Realm engagement; the Stitser umbrella (parent) sees both.

---

## Schema

```
entities
  parent_entity_id  → entities(id)      -- the tree; license holder = root

person_entities                          -- the roster (membership); already existed
  person_id, entity_id, role

relationship_assignments                 -- one row per (contact × entity)
  contact_id      → people(id)
  entity_id       → entities(id)
  assigned_to_id  → people(id)           -- must be in entity_id's roster
  collaborators   → people[]             -- also from entity_id's roster
  stage, source, status                  -- per-relationship
  unique (contact_id, entity_id)

comments / tasks / notes                 -- activity
  entity_id       → entities(id)         -- whose relationship this belongs to
  shared          boolean                -- rare exception, visible to all relationships
```

**Access resolution (read filter):** a record is visible when its `entity_id ∈ { viewer's entity } ∪ { descendants of viewer's entity }`, OR `shared = true`.

**Projects** are gated the same way through `entity_links` (project ↔ entity): a Realm project is visible to Realm and its ancestors only.

---

## Enforcement

- **Today (pre-launch):** the `entity_id` stamps and the bridge exist; visibility is applied in queries. Anon-read RLS placeholders are in place.
- **At launch (Stage-3 auth):** the read filter above becomes a **Row-Level Security policy** keyed to the logged-in user's entity scope (their entity + descendants), so isolation is enforced by the database, not the application. The `entity_id` columns are exactly what that policy reads.

## The `shared` exception

Default is entity-private. A small set of facts should be visible across *all* relationships on a person — "do not contact," "deceased," a shared bio. Those records set `shared = true`. Expect ~2% of records.

---

## Consistency note

This is the same structure as the **family goal firewall**: the kids are sub-entities under the Family Trust; the Trust's members (Clint + Christie) see down into the kids' entities, but the kids are off the Trust's roster and never see its money goals. "Kids under a parent" and "agents under a brokerage" are the identical pattern — which is why one entity tree serves family privacy, brokerage tenancy, and licensing.

---

*Design decision captured from the 2026-07-03 working session. Source of truth for how access, ownership, and visibility behave in S-BOS 2.0.*
