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
6. Create `workspace/features/<folder-name>/env_<slug>.md` with this exact format, replacing `<context files>` with one `- workspace/context/<filename>` line per file found in step 5:

```
# Environment

**Feature name:** <Feature name>
**US Jira ticket:** <jira-ticket-url>
**BA Task Jira ticket:** <jira-ticket-url>

**Context files:**
<context files>
- workspace/features/<folder-name>/idea_<slug>.md
- <confluence-page-url>

**Confluence output pages:**
- BA Doc: <confluence-page-url>
```

> If `workspace/context/` is empty or does not exist, only list the idea file under **Context files:**.

7. Create `workspace/features/<folder-name>/idea_<slug>.md` with this exact content:

```
# Feature Idea

## Overview
<describe the feature goal in 1–2 sentences>

## User-provided fields
### <Main Entity>
- <field>
- <field>

### <Sub-entity> (if applicable)
- <field>
- <field>

## System-generated fields
### <Main Entity>
- <field>: <how generated or default value>

### <Sub-entity> (if applicable)
- <field>: <default value>

## Search
- Search target: <entity being searched>
- Search by: <field(s)>
- Matching rule: <e.g. partial match on name>

## Validation
- <field>: <rule, e.g. required / must be > 0>

## Default Values
- <field>: <value>

## Permissions
- <PERMISSION_CONSTANT>

## Notes
<any additional rules or constraints>
```

8. Confirm:
```
✓ workspace/features/<folder-name>/env_<slug>.md
✓ workspace/features/<folder-name>/idea_<slug>.md

1. Fill in the placeholders in env_<slug>.md (Jira ticket, Confluence pages).
2. Replace the placeholder in idea_<slug>.md with your feature description.
Then run /gen-brief <Feature Name> to continue.
```
