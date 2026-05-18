---
name: "Archive Feature"
description: "Archive a completed feature. Usage: /archive <feature-name>"
---

Move a completed feature from `features/` to `archive/` so it no longer clutters the active workspace.

## Steps

1. Verify `features/<feature-name>/` exists
2. Check that all 6 artifacts exist:
   - `ai-docs/spec.md`
   - `ai-docs/flow.md`
   - `ai-docs/scenarios.md`
   - `ai-docs/tc.md`
   - `ba-doc.md`
   - `qa-doc.md`
3. If any artifact is missing, warn the user and ask whether to archive anyway or generate the missing ones first (suggest `/generate-next <feature-name>`)
4. Move the entire `features/<feature-name>/` folder to `archive/<feature-name>/`
5. Confirm:

```
✓ Archived: features/<feature-name>/ → archive/<feature-name>/
```

## Notes

- Archived features are preserved in git history — nothing is deleted
- To revisit an archived feature, copy it back from `archive/` to `features/`
- Only archive features that are fully reviewed and signed off
