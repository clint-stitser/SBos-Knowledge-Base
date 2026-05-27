# Kompass — S-BOS System Admin

This folder is the source of truth for all files in the **Kompass Claude.ai Project** knowledge base.

Edit files here first. Then upload the updated file to the Kompass project in Claude.ai (replacing the old version). Log the change to the S-BOS Activity Log.

---

## Folder Structure

```
projects/kompass/
├── README.md               ← This file
├── docs/                   ← .docx and .pdf reference documents
├── specs/                  ← .md spec and UI blueprint files
├── skills/                 ← .skill files installed in Kompass
├── mcp/                    ← MCP server source code
└── briefings/              ← Agent briefing .md files for team members
```

---

## File Index

### /specs
| File | Purpose | Change Frequency |
|------|---------|-----------------|
| `S-Bos_Agent___Guiding_Spec.md` | Master guiding spec — system architecture, Kompass MCP config, field slugs, app IDs, API behaviors | High |
| `Project_Execution_UI.md` | Blueprint for Softr-based project execution UI | Medium |
| `sb-skill-management-protocol.md` | Protocol for skill creation, logging, installation, and maintenance | Low |

### /briefings
| File | Person | Role |
|------|--------|------|
| `andi-westrich-agent-briefing.md` | Andi Westrich | S-BOS Admin, skill installer, HITL coordinator |
| `gino-perano-agent-briefing.md` | Gino Perano | Owner Broker, Stitser Properties |
| `alyssa-mcdermott-agent-briefing.md` | Alyssa McDermott | Role TBD — see briefing |

### /skills
| File | Workflow |
|------|---------|
| `sb-sales-team-skill.skill` | Sales team workflow |

### /mcp
| File | Purpose |
|------|---------|
| `smartsuite_mcp_server.py` | Kompass MCP server source — connects Claude to SmartSuite. Deployed on Railway. |

### /docs
| File | Purpose |
|------|---------|
| `Stitser_BUILT_SmartSuite_Data_Dictionary.docx` | Full field-level data dictionary for every SmartSuite app |
| `Platform_Economics___Incentive_Architecture_v4_-_March_2026.docx` | Compensation structure and incentive architecture |
| `Project_Playbook___Organizational_Alignment_v4_-_April_2026.docx` | Project execution playbook and org alignment framework |
| `Stitser_BUILT_Realignment_Roadmap_-_March_2026.docx` | Company realignment roadmap and strategic priorities |
| `stitser-built-team-activation.docx` | Team activation guide and onboarding reference |

---

## Key System References

| Item | Value |
|------|-------|
| Kompass MCP URL | `https://earnest-vitality-production.up.railway.app/mcp/mcp` |
| S-BOS Frontend | `https://app.stitserbuilt.com` |
| SmartSuite Projects App ID | `68216a706900e8eaf75a05a7` |
| SmartSuite People App ID | `68216a706900e8eaf75a05af` |
| Activity Log App ID | `69dc55333fe841263503f235` |
| Clint's People Record ID | `683f72d0591c71a2159825b8` |

> Full field slugs, app IDs, and API behaviors live in `specs/S-Bos_Agent___Guiding_Spec.md`.

---

## How Updates Work

1. Edit the file in this folder
2. Upload the updated file to the Kompass Claude.ai project (replace old version)
3. Log the change to the S-BOS Activity Log
4. For skill files, follow the full Skill Management Protocol in `specs/sb-skill-management-protocol.md`
