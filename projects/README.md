# Projects

This folder holds the knowledge base documents for each Claude.ai Project used at Stitser BUILT.

Each subfolder = one Claude.ai Project. The files inside are the source-of-truth versions of everything uploaded to that project's knowledge base.

---

## How It Works

| Step | Who | Action |
|---|---|---|
| 1 | Claude Code (S-BOS System Admin) | Creates or updates a doc in the relevant project subfolder |
| 2 | Andi (or assigned team member) | Downloads the updated file and re-uploads to the Claude.ai Project knowledge base |
| 3 | Andi | Marks the Update S-BOS task complete in SmartSuite |

**Rule:** Never edit a project doc directly in Claude.ai. Edit it here first, then upload. GitHub is the source of truth.

---

## Projects

| Folder | Claude.ai Project | Description |
|---|---|---|
| `kompass/` | Kompass — S-BOS System Admin | All reference docs, protocols, and system knowledge for the Kompass agent |

---

## Adding a New Project

1. Create a subfolder: `projects/[project-name]/`
2. Add a `README.md` describing what the project does and what files belong in its knowledge base
3. Add the source files
4. Commit and push
5. An Update S-BOS task will be created to upload the new files to Claude.ai
