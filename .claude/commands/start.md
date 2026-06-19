---
name: "Setup BA Environment"
description: "Initialize feature folder and env file before generating BA artifacts. Usage: /start <Feature Name>"
---

You are a Senior Business Analyst setting up the working environment for a new feature.

## Input

`$ARGUMENTS` is the **Feature name** exactly as typed by the user.

- If `$ARGUMENTS` is empty → ask the user: "What is the feature name?"

## Feature Name Normalization

Before any steps, normalize the feature name:

1. Apply title case: capitalize the first letter of every word.
2. Preserve known domain acronyms in UPPERCASE. Recognized acronyms for this project: `PO`, `PR`, `IR`, `SI`, `BA`, `SKU`, `ID`. Any word that matches one of these (case-insensitive) must be uppercased in full.
   - Examples: `create po` → `Create PO`, `update pr item` → `Update PR Item`, `view si` → `View SI`
3. After normalizing, check if the feature name looks valid:
   - Use `workspace/context/project.md` (if it exists) to cross-reference against known feature names, modules, and User Stories.
   - If the name seems like a typo, abbreviation, or doesn't match any known domain concept → show the normalized name and ask: "Did you mean **`<Normalized Feature Name>`**? Confirm to continue, or type the correct name."
   - If the name is clear and recognizable → proceed silently with the normalized name.
4. Use the confirmed, normalized feature name for all subsequent steps.

## Steps

1. Derive folder name: kebab-case of Feature name (e.g. "Create Product Category" → `create-product-category`)
2. Derive file slug: replace `-` with `_` in folder name (e.g. `create-product-category` → `create_product_category`)
3. Check if `workspace/features/<folder-name>/` already exists:
   - If it exists → list all files currently in the folder, then warn the user:
     > "Feature folder `workspace/features/<folder-name>/` already exists with the following files:
     > [list each file]
     > Running /start again will delete all of these and reinitialize the folder. Continue? (yes/no)"
   - **no** → stop. Do not change anything.
   - **yes** → delete all files in `workspace/features/<folder-name>/`, then continue to step 4.
4. Create folder `workspace/features/<folder-name>/` if it does not exist.
5. Scan `workspace/context/` for all files (e.g. `project.md`, `domain.md`, etc.) and collect their relative paths as a list.
6. Create `workspace/features/<folder-name>/env_<slug>.md` with this exact format, replacing `<context files>` with one `- workspace/context/<filename>` line per file found in step 5:

```
# Environment

**Feature name:** <Feature name>
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
### <Entity>
- <field>
- <field>

## System-generated fields
### <Entity>
- <field>: <how generated or default value>

## Search
- Search target: <entity being searched>
- Search by: <field(s)>
- Matching rule: <e.g. partial match on name>

## Validation
- <field>: <rule, e.g. required / must be > 0>

## Permissions
- <PERMISSION_CONSTANT>

## Notes
<any additional rules or constraints>
```

8. Create `workspace/features/<folder-name>/manual_tasks_<slug>.md` with this exact content:

```
# Manual Tasks — <Feature Name>
```

9. Confirm:
```
✓ workspace/features/<folder-name>/env_<slug>.md
✓ workspace/features/<folder-name>/idea_<slug>.md
✓ workspace/features/<folder-name>/manual_tasks_<slug>.md

1. Fill in the placeholders in env_<slug>.md (Jira ticket, Confluence pages).
2. Replace the placeholder in idea_<slug>.md with your feature description.
Then run /gen-brief <Feature Name> to continue.
```
