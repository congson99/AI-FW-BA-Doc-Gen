---
name: "Setup BA Environment"
description: "Initialize feature folder and env file before generating BA artifacts. Usage: /gen-ba <Feature Name>"
---

You are a Senior Business Analyst setting up the working environment for a new feature.

## Input

`$ARGUMENTS` is the **Feature name** exactly as typed by the user.

- If `$ARGUMENTS` is empty → ask the user: "What is the feature name?"

## Steps

1. Derive folder name: kebab-case of Feature name (e.g. "Create Product Category" → `create-product-category`)
2. Derive file slug: replace `-` with `_` in folder name (e.g. `create-product-category` → `create_product_category`)
3. Check if `workspace/features/<folder-name>/` already exists:
   - If it exists and `env_<slug>.md` already exists → inform user and stop: "Environment already set up. Edit `workspace/features/<folder-name>/env_<slug>.md` directly if needed."
4. Create folder `workspace/features/<folder-name>/` if it does not exist.
5. Scan `workspace/context/` for all files (e.g. `project.md`, `domain.md`, etc.) and collect their relative paths as a list.
6. Create `workspace/features/<folder-name>/env_<slug>.md` with this exact format, inserting each file found in step 5 as a `- workspace/context/<filename>` line under **Context files:**:

```
# Environment

**Feature name:** <Feature name>
**US Jira ticket:** <jira-ticket-url>
**BA Task Jira ticket:** <jira-ticket-url>

**Context files:**
- workspace/context/project.md
- <other files found in workspace/context/, one per line>

**Confluence output pages:**
- BA Doc: <confluence-page-url>
```

> If `workspace/context/` is empty or does not exist, leave **Context files:** section empty (no items).

7. Confirm:
```
✓ workspace/features/<folder-name>/env_<slug>.md

Fill in the placeholders (Jira ticket, Confluence pages), then run /gen-brief <Feature Name> to continue.
```
