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

7. Create `workspace/<folder-name>/idea_<slug>.md` using a two-pass approach:

   **Pass 1 — Fill from context**

   Read available context files (e.g. `project/context/project.md`, `project/reference/`). Cross-reference the normalized feature name against known features, modules, tickets, and descriptions.

   Rules:
   - Only fill content that is clearly derivable from existing context files. Do not invent or assume anything not found there.
   - If a section cannot be determined from context, leave its placeholder text as-is for now (it will be handled in Pass 2).
   - The Overview should come from the feature description in context (e.g. from the Scope of Work table in project.md).
   - Permissions should be left as `<PERMISSION_CONSTANT>` unless a matching permission constant is found in context.

   **Pass 2 — Ask user for remaining unknowns**

   After Pass 1, identify which sections still contain placeholder text. For each such section, ask the user a focused question to gather the missing information. Ask all questions together in one message — do not ask one at a time.

   Format the questions clearly, for example:
   > A few questions to complete `idea_update_po.md`:
   >
   > **1. User-provided fields** — What fields can the user edit on a PO? (e.g. supplier, delivery date, line items)
   > **2. Validation** — Any validation rules for those fields? (e.g. required, format, constraints)
   > **3. Permissions** — What permission constant controls this action? (e.g. `PO_UPDATE`)
   > **4. Notes** — Any special business rules or constraints?
   >
   > Answer what you know — type "skip" for any you want to leave for later.

   After the user responds:
   - Fill in each answered section with the user's input.
   - For any section the user skipped or left blank, keep the original placeholder text.
   - Write the final file with all filled and unfilled sections combined.

   Template:

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

Next: Fill in the placeholders in env_<slug>.md (Jira ticket, Confluence pages).
Then run /gen-brief <Feature Name> to continue.
```
If any sections in `idea_<slug>.md` still have placeholder text (user skipped them), also note:
```
⚠ idea_<slug>.md has unfilled sections — you can complete them before running /gen-brief, or let the generator handle them with available context.
```
