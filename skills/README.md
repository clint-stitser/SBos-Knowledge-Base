# Skills

This folder is the source of truth for all Claude skills installed in the Kompass project.

Each subfolder contains one skill's `SKILL.md` source file.

---

## How It Works

| Step | Who | Action |
|---|---|---|
| 1 | Claude Code (S-BOS System Admin) | Creates or updates a `SKILL.md` file in this folder |
| 2 | Claude Code | Packages it into a `.skill` file using the `skill-creator` packaging script |
| 3 | Andi | Downloads the `.skill` file from the S-BOS Activity Log and uploads it to the Kompass Claude.ai project |
| 4 | Andi | Checks "HITL Admin Update Complete" on the Activity Log record |

**Rule:** Never edit a skill directly in Claude.ai. Edit the `SKILL.md` here, package it, and re-upload. GitHub is the source of truth.

---

## Installed Skills

| Skill | Description | Status |
|---|---|---|
| `value-chain-guide` | Guides team members through documenting, editing, and reviewing value chain workflow docs. Produces structured markdown and submits to SmartSuite for integration. | ✅ Live |
| `sb-skill-management` | Manages the full lifecycle of Claude skills — intake, deduplication check, creation, Activity Log, and admin handoff. | ✅ Live |

---

## Adding a New Skill

1. Create a new subfolder: `skills/[skill-name]/`
2. Add a `SKILL.md` with the correct frontmatter:
   ```
   ---
   name: skill-name
   description: [trigger text — when should Claude invoke this skill]
   ---
   ```
3. Commit and push
4. Package and upload per steps 2–4 above
