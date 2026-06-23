---
name: "Init Workspace"
description: "Initialize workspace folder structure. Usage: /init"
---

You are setting up the workspace directory structure for the BA Documentation Generation Framework.

## Steps

1. Check whether `project/` exists in the project root.
2. Check each required subfolder: `project/context/`, `project/reference/`, `workspace/`.
3. For each folder that is missing, create it. For folders that already exist, leave them untouched.
4. Check whether `project/sync_config.md` exists:
   - If it does not exist → create it with the template below.
   - If it already exists → leave it untouched.
5. Report the result:

**If project/ did not exist at all:**
```
✓ Created project/
✓ Created project/context/
✓ Created project/reference/
✓ Created project/sync_config.md
✓ Created workspace/

Initialized. Next steps:
1. Open project/sync_config.md and fill in the Confluence URLs for each entry.
2. Run /start <Feature Name> to begin working on a feature.
```

**If project/ existed but some items were missing** — list only the created ones:
```
✓ Created project/<missing-item>
... (one line per item created)

Done. Existing folders and files were left untouched.
```

**If everything already exists:**
```
project/ is already initialized:
  ✓ project/context/
  ✓ project/reference/
  ✓ project/sync_config.md
  ✓ workspace/

No changes made.
```

## Template for project/sync_config.md

```markdown
# Confluence Sync Config

Map each Confluence page to a local file in `project/`.
Run /sync to pull the latest content from these pages into the local files.

## Context

- project/context/project.md
  url: <confluence-page-url>

## Reference

- project/reference/<filename>.md
  url: <confluence-page-url>
```
