---
name: "Clear Project"
description: "Delete all synced context/reference files, reset project_config.md back to its blank template, and clear all feature folders in workspace/. Usage: /clear-project"
---

You are resetting `project/` and `workspace/` so the repo is ready for a new project, while keeping the config file's structure intact.

## Steps

1. Check if `project/context/`, `project/reference/` contain any files, if `project/project_config.md` differs from `framework/templates/project_config_blank.md`, or if `workspace/` contains any feature folders:
   - If nothing to reset → stop and inform: "project/ and workspace/ are already at their default state. Nothing to clear."
2. List what will happen:
   - All files under `project/context/` and `project/reference/` (recursively) will be deleted.
   - `project/project_config.md` will be reset to the blank template — any MCP URLs, Confluence mappings, and Task Automation settings filled in for this project will be lost, but the section structure and guidance comments are kept.
   - All feature folders under `workspace/` will be deleted (same as `/clear-workspace`).
3. Ask the user to confirm:
   ```
   Reset project/ for a new project? This deletes all synced context/reference files, resets project_config.md to its blank template, and deletes all feature folders in workspace/. This cannot be undone. (yes/no)
   ```
   - If user cancels → stop: "Cancelled. Nothing was deleted."
   - If user confirms → continue.
4. Delete all files and subfolders inside `project/context/` and `project/reference/` (keep the empty folders themselves).
5. Delete all contents inside `workspace/` (all feature subfolders and files) — same effect as `/clear-workspace`, run without its own separate confirmation since this confirmation already covers it.
6. Overwrite `project/project_config.md` with the exact contents of `framework/templates/project_config_blank.md` (read that file and copy it verbatim — do not paraphrase or reformat).
7. Confirm: "✓ Cleared project/context/ and project/reference/, reset project_config.md to its blank template, and cleared workspace/."
