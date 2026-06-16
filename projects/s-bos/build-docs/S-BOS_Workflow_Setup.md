# S-BOS Workflow Setup — Brainstorm-Anywhere via a Claude Project + GitHub

> **Purpose:** How to set up and run design-doc work from anywhere (incl. phone), with docs staying as version-controlled markdown in GitHub.
> **Source of truth:** the GitHub repo. The chat is a scratchpad; `memory.md` + `restart.md` carry state between sessions and surfaces.

---

## The model

- **App data** → Supabase (Postgres). Not Notion. (Ryan vetted Supabase.)
- **Design/build docs** → markdown in GitHub (`clint-stitser/SBos-Knowledge-Base`, `projects/s-bos/build-docs/`). Not Notion — keeps git history + lives next to code.
- **Brainstorming** → a Claude Project connected to GitHub (read **and** write), so any device picks up the same state.

---

## Setup (one-time)

### 1. GitHub connector (read + write)
Requires a Claude plan with custom connectors (Pro / Max / Team / Enterprise).

- Settings → Connectors → **Add custom connector**
- Name `GitHub`, URL `https://api.githubcopilot.com/mcp/`
- **Connect** → authorize via GitHub OAuth → grant **only** `SBos-Knowledge-Base` (and `sb-crm-poc` if code access wanted). Least privilege.
- Fallback if unavailable: fine-grained PAT with **Contents: Read and write** on those repos + a PAT-based GitHub MCP connector.

> Auth/tokens are done by Clint — Claude never handles credentials.

### 2. The "S-BOS Build" Project
- New Project → `S-BOS Build`
- Paste the **Session Bootstrap** (below) into the project Instructions
- Enable the GitHub connector for the project

### 3. Session Bootstrap (paste into project Instructions)
```
# S-BOS Build — Session Bootstrap
At the start of EVERY session, before anything else, read these files from
GitHub repo `clint-stitser/SBos-Knowledge-Base`, folder
`projects/s-bos/build-docs/`, in this order:
  1. S-BOS_Operating_Agreement.md   (the working agreement — follow it)
  2. memory.md
  3. restart.md
  4. S-BOS_Design_Context.md
Then confirm current state and wait for me to say go. Pick up from
restart.md → Next Steps.
When we finish or pause: write updated memory.md and restart.md back to the
repo and commit (two-line message), then summarize what changed in chat.
Decision-maker: Clint. Follow the collaboration model in the Operating
Agreement — human leads, agent executes precisely, nothing invented,
ambiguity killed at the source, calibrated honesty over confident answers.
```

### 4. Verify
- **Read:** "Read the build-docs continuity files and tell me where we left off."
- **Write:** "Add a line to restart.md noting the connector is live, and commit." → confirm the commit on GitHub.

---

## Regular workflow

1. Open **S-BOS Build** on any device.
2. "Let's continue." → it reads the continuity files, states where we are.
3. Brainstorm → it drafts/edits the design doc section by section (Operating Agreement model).
4. It commits markdown to GitHub + updates `memory.md` / `restart.md`.
5. Resume anywhere next time from `restart.md`.

---

## Surfaces that all share this one source of truth
- **Claude Project (web/Desktop/phone):** this setup — brainstorm anywhere.
- **Claude Code (desktop or `claude.ai/code` web):** full native git read/write/push; best at the machine.
- Both read/write the **same GitHub repo**, so state never diverges.
