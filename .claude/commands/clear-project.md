---
name: "Clear Project"
description: "Delete all content in the project/ folder, including project_config.md. Usage: /clear-project"
---

You are clearing all local project-level context.

## Steps

1. Check if `project/` exists and contains any files or folders:
   - If empty or does not exist → stop and inform: "project/ is already empty. Nothing to delete."
2. List the top-level items currently in `project/` (e.g. `project_config.md`, `context/`, `reference/`) that will be deleted.
3. Ask the user to confirm:
   ```
   Delete ALL contents of project/ — including project_config.md (MCP config, Confluence mappings) and all synced context/reference files? This cannot be undone.
   You will need to recreate or restore project_config.md before running /connect-mcp or /sync-project again. (yes/no)
   ```
   - If user confirms → delete all files and subfolders inside `project/` (keep the empty `project/` folder itself), then confirm: "✓ Cleared project/"
   - If user cancels → stop: "Cancelled. Nothing was deleted."
