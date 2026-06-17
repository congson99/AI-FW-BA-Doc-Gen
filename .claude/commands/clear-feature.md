---
name: "Clear Feature"
description: "Delete a specific feature folder or all features in workspace/features/. Usage: /clear-feature [Feature Name]"
---

You are a Senior Business Analyst managing the workspace.

## Input

`$ARGUMENTS` is the **Feature name** (optional).

## Steps

### Case 1 — Feature name provided

1. Derive folder name: kebab-case of the feature name (e.g. "Update PO" → `update-po`)
2. Check if `workspace/features/<folder-name>/` exists:
   - If not found → stop and inform user: "Feature folder `workspace/features/<folder-name>/` not found. Nothing to delete."
3. Ask the user to confirm: "Delete `workspace/features/<folder-name>/` and all its contents? (yes/no)"
   - If user confirms → delete the folder and all contents, then confirm: "✓ Deleted workspace/features/<folder-name>/"
   - If user cancels → stop: "Cancelled. Nothing was deleted."

### Case 2 — No feature name provided

1. List all folders currently in `workspace/features/`.
   - If empty → stop and inform: "workspace/features/ is already empty. Nothing to delete."
2. Show the user the list of folders that will be deleted.
3. Ask the user to confirm: "Delete ALL feature folders listed above? This cannot be undone. (yes/no)"
   - If user confirms → delete all contents inside `workspace/features/` (all subfolders and files), then confirm: "✓ Cleared workspace/features/"
   - If user cancels → stop: "Cancelled. Nothing was deleted."
