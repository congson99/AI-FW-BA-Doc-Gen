# Project Config

> See README.md § "Configure the Project" for detailed guidance on filling in each section below. Placeholders look like `<this>` — replace them with real values.

---

## 1. MCP Config

- Atlassian: <confluence-mcp-url>

---

## 2. Language

- Document language: <e.g. English, Vietnamese>

---

## 3. Context Sync

### Context

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

## 4. Task Environment

### env_<slug>.md template

```
**BA Task Jira ticket:** <jira-ticket-url>

**Confluence output pages:**
- BA Doc: <confluence-page-url>
```

### context_<slug>.md template

```
# Context Files

- project/context/project.md
```

---

## 5. Task Automation

### Jira

- Update ticket status to: <jira-status>
  jira-project: <jira-project-key>

- Add Confluence page link as comment on ticket
  jira-project: <jira-project-key>

### Confluence

- Publish BA Doc to parent page
  confluence-parent: <confluence-parent-page-url>
