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
5. Create `workspace/features/<folder-name>/env_<slug>.md` with this exact format:

```
# Environment

**Feature name:** <Feature name>
**US Jira ticket:** <jira-ticket-url>
**BA Task Jira ticket:** <jira-ticket-url>

**Context files:**
- <additional-context-file-or-confluence-url>

**Confluence output pages:**
- BA Doc: <confluence-page-url>
```

6. Confirm:
```
✓ workspace/features/<folder-name>/env_<slug>.md

Fill in the placeholders (Jira ticket, context files, Confluence pages), then run /gen-brief <Feature Name> to continue.
```
