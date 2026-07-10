# The Tool Library: the craftsman's truck

*S-BOS Foundations · Article 3 of 5*

---

A craftsman's truck carries every tool he owns. But when he walks up to a
job, he doesn't drag the whole truck to the door — he loads a toolbelt with
only what *this* job calls for.

S-BOS works exactly the same way. A **tool** is a code surface — a budget
tab, a CRM tab, an underwriting module, a pay-app workflow. Every tool that
exists is always "in the truck." What shows up on any given record is only
what that record's work actually calls for.

```
   THE TRUCK                          THE TOOLBELT
   (code registry — every tool,       (a record's tabs — only what
    always available)                  this record's work calls for)

   ┌───────────────────────┐          Wilson Landing (project)
   │ details  budget       │          ┌─────────────────────────┐
   │ budget_simple  crm    │   ──→    │ details │ budget │ crm  │
   │ underwriting  pay_apps│          └─────────────────────────┘
   │ documents  schedule…  │
   └───────────────────────┘
```

## Three layers decide the toolbelt

### 1. The registry (code) — every tool, once

`lib/tools/registry.ts` is the single list of every tool: its key, label,
description, and which record kinds it applies to (projects, companies,
people, loans). Adding a tool to the platform means one registry entry plus
one render site. That's it.

### 2. Blueprint defaults (data) — what a department hands out

Each department's blueprint declares what a record in that department gets
out of the box: `departments.default_tools[]`.

- A **full vertical** (Entry-Level Housing, Retail…) gets the works —
  budget, schedule, pay apps, documents, CRM.
- **General Brokerage** gets `budget_simple` — a lean budget shaped around
  offers and a closing statement. No pay apps; a brokerage deal never has a
  schedule of values.
- **Support departments** get a lean set.

### 3. Record overrides (data) — the exceptions

Sparse rows in `record_tools` flip individual tools on or off for one record:

- The **subdivision master project** turns ON `underwriting` — most projects
  never need it.
- A one-off venture turns OFF pay apps.

## Resolution: override → blueprint → default

`lib/tools/resolve.ts` answers "what's on this record's toolbelt?" in strict
order:

```
record override (record_tools)     — if a row exists, it wins
   ↓ else
department blueprint (default_tools)
   ↓ else
registry default
```

One tool is special: `details` is **locked** — it's the launch pad itself,
the record's home tab, and can never be removed.

## The UI: tabs and the Tools button

A record's detail page renders its tabs directly from the resolution. To
change the toolbelt, use the **Tools** button on the record's detail page
(the ManageToolsButton) — it's the toolbelt editor, showing everything in the
truck and letting you toggle what this record carries.

[screenshot to be added: a project detail page with the Tools button open,
showing active and available tools]

You can also just ask Kompass — "set up a budget on Wilson Landing" activates
the tool *and* seeds it from the blueprint template. See
[Kompass builds tools with you](kompass-tool-builder.md).

## Adding a tool to the library (builder checklist)

1. **Register** the surface in `lib/tools/registry.ts` — key, label,
   description, applicable record kinds.
2. **Render** it where its key resolves — a record tab, or a launch-out page.
3. **Add it to the right department blueprints** — `departments.default_tools`
   for every department whose records should get it by default.
4. **Teach Kompass** — if Kompass should be able to assemble or seed it, add
   its seeding step to `assemble_tool`.
5. **Document the technique** in the KB (`kb_entries` /
   SBos-Knowledge-Base) — remember the ontology: a tool without a technique
   isn't a skill yet.

---

*Previous: [Task ontology](task-ontology.md) · Next: [Surfaces](surfaces.md)*
