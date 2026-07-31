# Project Config

## 0. Status
Latest MCP connect: 2026/07/31 14:53:38
Latest sync: 2026/07/31 15:13:40
---

> See README.md § "Configure the Project" for detailed guidance on filling in each section below. Placeholders look like `<this>` — replace them with real values.

---

## 1. MCP Config

- Atlassian: https://dc32claude.atlassian.net/wiki/spaces/INV/overview?homepageId=98414

---

## 2. Language

- Document language: English

---

## 3. Context Sync

### Context

- project/context/project-overview.md
  url: https://dc32claude.atlassian.net/wiki/spaces/INV/overview?homepageId=98414

### Business Rules — Principles

### Business Rules — Shared References

- project/reference/business-rules/shared-references/general-business-rules.md
  url: https://dc32claude.atlassian.net/wiki/spaces/INV/pages/43548674/General+Business+Rules

### UI Behavior — Principles

### UI Behavior — Shared References

- project/reference/ui-behavior/shared-references/ui-rules.md
  url: https://dc32claude.atlassian.net/wiki/spaces/INV/pages/35717128/UI+Rules

### Navigation

### Messages

- project/reference/messages/message-format.md
  url: https://dc32claude.atlassian.net/wiki/spaces/INV/pages/10190877/Message+Format+Definition

---

## 4. Task Environment

### env_<slug>.md template

```
**BA Task Jira ticket:** <jira-ticket-url>

**Confluence output pages:**
- BA Doc: <confluence-page-url>
- Spec: <confluence-page-url>
- Flow: <confluence-page-url>
```

### context_<slug>.md template

```
# Context Files

- project/context/project-overview.md
```

---

## 5. Task Automation

### Jira

- Update ticket status to: In Review
  jira-project: IN

### Confluence

- Publish BA Doc to "BA Doc" confluence output page
- Publish sections Brief, AC, Business Rules, Data Definition from BA Doc to "Spec" confluence output page
- Publish sections Navigation, Flow, UI Behavior, Messages from BA Doc to "Flow" confluence output page
- Check the BA Doc for any permissions defined for the feature (permission key + description); if found, add each one under its corresponding module section (matching the "Module" column) on the Permission Definition page — creating a new module section if the module doesn't exist yet
  url: https://dc32claude.atlassian.net/wiki/spaces/INV/pages/17465348/Permission+Definition
