---
name: "Init Workspace"
description: "Initialize workspace folder structure. Usage: /init"
---

You are setting up the workspace directory structure for the BA Documentation Generation Framework.

## Steps

1. Check whether `workspace/` exists in the project root.
2. Check each required subfolder: `workspace/context/`, `workspace/reference/`, `workspace/features/`.
3. For each folder that is missing, create it. For folders that already exist, leave them untouched.
4. Report the result:

**If workspace/ did not exist at all:**
```
✓ Created workspace/
✓ Created workspace/context/
✓ Created workspace/reference/
✓ Created workspace/features/

Workspace initialized. Next steps:
- Add project context files to workspace/context/ (e.g. project.md, domain.md).
- Add reference documents to workspace/reference/ (e.g. Confluence exports, spec sheets).
- Run /start <Feature Name> to begin working on a feature.
```

**If workspace/ existed but some subfolders were missing** — list only the created ones:
```
✓ Created workspace/<missing-folder>/
... (one line per folder created)

Done. Existing folders were left untouched.
```

**If all folders already exist:**
```
workspace/ is already initialized:
  ✓ workspace/context/
  ✓ workspace/reference/
  ✓ workspace/features/

No changes made.
```
