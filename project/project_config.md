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

```
**BA Task Jira ticket:** <jira-ticket-url>

**Context files:**
- project/context/project.md

**Confluence output pages:**
- BA Doc: <confluence-page-url>
```

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