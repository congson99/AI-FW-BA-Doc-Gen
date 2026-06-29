---
name: "Init Workspace"
description: "Initialize workspace folder structure. Usage: /init"
---

You are setting up the workspace directory structure for the BA Documentation Generation Framework.

## Steps

1. Check whether `project/` exists in the project root.
2. Check each required subfolder: `project/context/`, `project/reference/`, `project/reference/business-rules/`, `project/reference/business-rules/principles/`, `project/reference/business-rules/shared-references/`, `project/reference/ui-behavior/`, `project/reference/ui-behavior/principles/`, `project/reference/ui-behavior/shared-references/`, `project/reference/navigation/`, `project/reference/messages/`, `workspace/`.
3. For each folder that is missing, create it. For folders that already exist, leave them untouched.
4. Check whether `project/project_config.md` exists:
   - If it does not exist → create it with the template below.
   - If it already exists → leave it untouched.
5. Report the result:

**If project/ did not exist at all:**
```
✓ Created project/
✓ Created project/context/
✓ Created project/reference/
✓ Created project/reference/business-rules/
✓ Created project/reference/business-rules/principles/
✓ Created project/reference/business-rules/shared-references/
✓ Created project/reference/ui-behavior/
✓ Created project/reference/ui-behavior/principles/
✓ Created project/reference/ui-behavior/shared-references/
✓ Created project/reference/navigation/
✓ Created project/reference/messages/
✓ Created project/project_config.md
✓ Created workspace/

Initialized. Next steps:
1. Open project/project_config.md and fill in the values for your project.
2. Run /connect-mcp to connect to the configured MCP servers.
3. Run /sync to fetch Confluence content into local project files.
4. Run /start <Feature Name> to begin working on a feature.
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
  ✓ project/reference/business-rules/
  ✓ project/reference/business-rules/principles/
  ✓ project/reference/business-rules/shared-references/
  ✓ project/reference/ui-behavior/
  ✓ project/reference/ui-behavior/principles/
  ✓ project/reference/ui-behavior/shared-references/
  ✓ project/reference/navigation/
  ✓ project/reference/messages/
  ✓ project/project_config.md
  ✓ workspace/

No changes made.
```

## Template for project/project_config.md

```markdown
# Project Config

---

## 1. MCP Config

MCP servers used by this project.

- Atlassian: <confluence-mcp-url>

---

## 2. Context Sync

Map each Confluence page to a local file in `project/`.
Run `/sync` to pull the latest content from these pages into the local files.

### Context

### Reference

- project/reference/<filename>.md
  url: <confluence-page-url>

### Business Rules — Principles

- project/reference/business-rules/principles/<filename>.md
  url: <confluence-page-url>

### Business Rules — Shared References

- project/reference/business-rules/shared-references/<filename>.md
  url: <confluence-page-url>

### UI Behavior — Principles

- project/reference/ui-behavior/principles/<filename>.md
  url: <confluence-page-url>

### UI Behavior — Shared References

- project/reference/ui-behavior/shared-references/<filename>.md
  url: <confluence-page-url>

### Navigation

- project/reference/navigation/<filename>.md
  url: <confluence-page-url>

### Messages

- project/reference/messages/<filename>.md
  url: <confluence-page-url>

---

## 3. Task Environment

Default structure for `env_<slug>.md` files created by `/start`.
Edit the values below to match your project before running `/start`.

\`\`\`
**BA Task Jira ticket:** <jira-ticket-url>

**Context files:**
- project/context/project.md

**Confluence output pages:**
- BA Doc: <confluence-page-url>
\`\`\`

---

## 4. Task Automation

Actions Claude automatically performs when running `/publish` for each feature task.
Fill in the Jira and Confluence targets for this project.

### Jira

- Update ticket status to: <jira-status>
  jira-project: <jira-project-key>

- Add Confluence page link as comment on ticket
  jira-project: <jira-project-key>

### Confluence

- Publish BA Doc to parent page
  confluence-parent: <confluence-parent-page-url>
```
