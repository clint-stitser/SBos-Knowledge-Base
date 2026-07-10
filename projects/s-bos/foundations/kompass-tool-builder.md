# Kompass builds tools with you

*S-BOS Foundations · Article 5 of 5*

---

Every operating system faces the same bottleneck: one person who knows how to
build the tools, and a line of people waiting for them. The Kompass
Tool-Builder exists to remove that bottleneck.

**The point: the system helps users build their own tools, so one designer
isn't the bottleneck.** Kompass walks a user from a plain-language workflow
description down to the foundational building blocks and back up — and at
every step, if a template, automation, or logic block already exists, the
machine does that step instead of a human.

## The loop

```
OBSERVE     the user's workflow
            ("I want to solicit vendor bids once a budget exists")

DECOMPOSE   into building blocks
            (budget template → vendor list per cost code →
             solicitation send → bid intake → comparison table)

MATCH       each block against the library:
            existing tool? blueprint template? automation?
            yes → the machine does it
            no  → build the smallest new block

ASSEMBLE    activate tools on the record's toolbelt;
            seed from templates — never a blank table

INTEGRATE   at the right place, right time — a button or tab
            where the work happens (the "Audit this loan"
            pattern), not a new silo
```

Two of those steps carry the philosophy:

- **ASSEMBLE — never a blank table.** A user should always start from a
  populated foundation. Blank tables are where adoption goes to die.
- **INTEGRATE — no new silos.** A new capability appears as a button or tab
  at the spot in the existing workflow where the work happens, not as
  another destination people have to remember to visit.

## What works today: "set up a budget on <project>"

The first working slice is the `assemble_tool` Kompass capability. Tell
Kompass:

> "Set up a budget on Wilson Landing."

Kompass then:

1. Activates the `budget` tool on that record's toolbelt (see
   [The Tool Library](tool-library.md)), and
2. Seeds its line items from the department blueprint's **51 template rows**
   (the CrossMod Land Development template), and
3. Replies with where the budget now lives.

You never start from a blank table — you start from a full, blueprint-shaped
budget and edit from there.

[screenshot to be added: Kompass chat activating a budget and the seeded
budget tab on the project]

## The roadmap: the vendor-solicitation arc

The budget seeding is step A of a complete worked example. The next slices,
in order:

1. **Vendor enrichment** — per cost code, Kompass suggests companies (from
   `companies` × sector) as bid candidates on each budget line.
2. **RFQ automation** — Kompass drafts and sends solicitation emails through
   the Gmail/Nango connection, logging each to the project's CRM tab.
3. **Bid comparison** — a `budget_simple`-style table comparing vendor
   responses per line, feeding award decisions back into the budget.
4. **Full describe-your-workflow mode** — `assemble_tool` grows a
   `describe_workflow` mode: Kompass interviews you, produces the DECOMPOSE
   list, and proposes the MATCH table for your approval before assembling
   anything.

That last step is the destination: any team member describes a workflow in
plain language, and Kompass — with the user approving each match — assembles
the tool.

## For builders

If you're adding a tool that Kompass should be able to assemble, the
requirement is one extra step in the
[builder checklist](tool-library.md#adding-a-tool-to-the-library-builder-checklist):
teach `assemble_tool` the tool's seeding step, so it can always hand users a
populated foundation.

---

*Previous: [Surfaces](surfaces.md) · Start of series: [The Grid](the-grid.md)*
