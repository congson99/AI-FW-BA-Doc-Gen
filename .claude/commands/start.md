---
name: "Setup BA Environment"
description: "Initialize feature folder and env file before generating BA artifacts. Usage: /start <Feature Name>"
---

You are a Senior Business Analyst setting up the working environment for a new feature.

## Input

`$ARGUMENTS` is the **Feature name** exactly as typed by the user.

- If `$ARGUMENTS` is empty → ask the user: "What is the feature name?"

## Pre-flight

1. Check whether `## 0. Status` in `project/project_config.md` contains a `Latest sync:` line with a real timestamp (not a placeholder):
   - If not found → stop and inform the user:
     > "Project has not been synced yet. Please run /sync before starting a feature."

2. Scan `## 3. Task Environment` in `project/project_config.md` for unfilled placeholders (pattern `<...>`), including content inside code blocks (fenced with ` ``` `) — the template lives inside the code block.
   - If any placeholders are found → stop and inform the user:
     ```
     project/project_config.md — Task Environment has unfilled placeholders:
       - <placeholder 1>
       ...
     Please complete section 3 before running /start.
     ```

---

## Feature Name Normalization

Before any steps, normalize the feature name:

1. Apply title case: capitalize the first letter of every word.
2. Preserve known domain acronyms in UPPERCASE. Recognized acronyms for this project: `PO`, `PR`, `IR`, `SI`, `BA`, `SKU`, `ID`. Any word that matches one of these (case-insensitive) must be uppercased in full.
   - Examples: `create po` → `Create PO`, `update pr item` → `Update PR Item`, `view si` → `View SI`
3. After normalizing, check if the feature name looks valid:
   - Use `project/context/project.md` (if it exists) to cross-reference against known feature names, modules, and User Stories.
   - If the name seems like a typo, abbreviation, or doesn't match any known domain concept → show the normalized name and ask: "Did you mean **`<Normalized Feature Name>`**? Confirm to continue, or type the correct name."
   - If the name is clear and recognizable → proceed silently with the normalized name.
4. Use the confirmed, normalized feature name for all subsequent steps.

## Steps

1. Derive folder name: kebab-case of Feature name (e.g. "Create Product Category" → `create-product-category`)
2. Derive file slug: replace `-` with `_` in folder name (e.g. `create-product-category` → `create_product_category`)
3. Check if `workspace/<folder-name>/` already exists:
   - If it exists → list all files currently in the folder, then warn the user:
     > "Feature folder `workspace/<folder-name>/` already exists with the following files:
     > [list each file]
     > Running /start again will delete all of these and reinitialize the folder. Continue? (yes/no)"
   - **no** → stop. Do not change anything.
   - **yes** → delete all files in `workspace/<folder-name>/`, then continue to step 4.
4. Create folder `workspace/<folder-name>/` if it does not exist.
5. Scan `project/context/` for all files (e.g. `project.md`, `domain.md`, etc.) and collect their relative paths as a list.
6. Read `project/project_config.md` and locate the `## 3. Task Environment` section. Extract the template content inside the code block (stop at the closing fence). Create `workspace/<folder-name>/env_<slug>.md` with:
   - Line 1: `**Feature name:** <normalized Feature name>`
   - Line 2: blank
   - Line 3 onwards: the extracted template content verbatim, as-is, without any modification.
   - After writing the template content, inject the idea file path:
     - If the env file contains a `**Context files:**` line → append `- workspace/<folder-name>/idea_<slug>.md` as the last item under that section.
     - If no `**Context files:**` section exists → append the following block at the end of the file:
       ```
       **Context files:**
       - workspace/<folder-name>/idea_<slug>.md
       ```
   - If `project/project_config.md` does not exist or `## 3. Task Environment` is not found → create the file with only: `**Feature name:** <normalized Feature name>`

7. Create `workspace/<folder-name>/idea_<slug>.md` with this exact content:

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

8. Create `workspace/<folder-name>/manual_tasks_<slug>.md` with this exact content:

```
# Manual Tasks — <Feature Name>
```

9. Confirm:
```
✓ workspace/<folder-name>/env_<slug>.md
✓ workspace/<folder-name>/idea_<slug>.md
✓ workspace/<folder-name>/manual_tasks_<slug>.md

1. Fill in the placeholders in env_<slug>.md (Jira ticket, Confluence pages).
2. Replace the placeholder in idea_<slug>.md with your feature description.
Then run /gen-brief <Feature Name> to continue.
```
