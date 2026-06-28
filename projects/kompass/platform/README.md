# Kompass Platform

This folder contains the Platform Design Document (PDD) and all supporting design work for the Kompass product family — a suite of niche-specific AI operating systems for real estate professionals.

## Products

| Product | Audience | Folder |
|---|---|---|
| Developer Kompass | Land developers, LIHTC, value-add | `developer/` |
| Contractor Kompass | General contractors, construction managers | `contractor/` |
| Agent Kompass | Real estate brokers and agents | `agent/` |
| Personal Kompass | Individual operators (Stitser Way integration) | `personal/` |

## Shared Architecture

All four products share the same underlying architecture. See `shared/` for:
- The Brain model and data hierarchy
- Intelligence stores (people profiles, vendor ratings, decision matrices, decision log, knowledge library)
- The Operator Assistant (Kompass) design
- The Feed and human-in-the-loop model
- Universal skills and routines
- Onboarding and Brain Setup Score

## Platform PDD

The master Platform Design Document lives at: `platform-pdd.md`

## Status

Phase 1 — Internal (Stitser BUILT). All four product lines in design.
