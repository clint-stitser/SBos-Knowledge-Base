# The Brain — Domain-Structured Memory

## What It Is

The Brain is the moat. It is the accumulated, organized intelligence about the operator's world — the thing that makes every Kompass interaction more useful than a generic chat session.

Unlike a flat key/value memory store, the Kompass Brain is **hierarchically structured** around the operator's actual domain model.

## The Universal Data Hierarchy

Every Kompass product organizes its Brain around the same four-level hierarchy:

```
Entity
  └── Category
        └── Project
              └── [People · Tasks · Budget · Schedule · Outcome]
```

- **Entity** — the legal or organizational owner (a business, a trust, a person)
- **Category** — the domain lens (Construction, Brokerage, Finance, Personal)
- **Project** — the unit of work (a deal, a property, a listing, a program)
- **Project atoms** — the five things every project contains

## How the Brain Grows

1. **Conversation** — facts extracted from every chat, filed automatically
2. **Quick Capture** — voice notes, share sheet inputs, on-the-go entries
3. **Imports** — documents, transcripts, PDFs, CSV exports
4. **Skill execution** — every skill run writes what it found back to the Brain

## Brain Setup Score

A score that shows the operator how complete their context is and what to fill in next. Higher score = sharper, more autonomous assistant responses.

Dimensions scored:
- Entity and project data completeness
- People profiles (team + key counterparties)
- Vendor ratings (% of active vendors rated)
- Decision matrices (% of recurring decision types configured)
- Knowledge library (% of domain areas seeded)
- Integration connections (Gmail, Drive, Smartsheet, QuickBooks)

## Tech Stack

- **Primary data layer:** SmartSuite (Entity → Category → Project → atoms)
- **Reference docs and context:** GitHub (`clint-stitser/Clint-s-Kompass`)
- **File storage:** Google Drive (linked to project records)
- **Integrations:** Gmail, Smartsheet, QuickBooks, Oura, Strava
