---
name: "Generate UI Behavior"
description: "Generate UI Behavior for a feature. Usage: /gen-ui-behavior <Feature Name>"
---

You are a Senior Business Analyst.

## Input

`$ARGUMENTS` is the **Feature name** exactly as typed by the user.

- If `$ARGUMENTS` is empty → ask the user: "What is the feature name?"

## Pre-flight Check

1. Derive folder name: kebab-case of Feature name (e.g. "Create Product Category" → `create-product-category`)
2. Derive file slug: replace `-` with `_` in folder name (e.g. `create-product-category` → `create_product_category`)
3. Check `workspace/<folder-name>/env_<slug>.md` exists:
   - If missing → stop and inform user: "Run `/start <Feature Name>` first to set up the environment."
4. Check the **Context files:** section in the env file:
   - If any line still contains an unfilled placeholder (e.g. `<additional-context-file-or-confluence-url>`) → stop and inform user: "**Context files** in `env_<slug>.md` still has unfilled placeholders. Either fill them in or remove the placeholder lines, then re-run /gen-ui-behavior."
   - An empty **Context files:** section (no items listed) is allowed — continue.
5. Read `workspace/<folder-name>/env_<slug>.md` and load every file listed under **Context files** — read each one before proceeding.
6. Check `workspace/<folder-name>/brief_<slug>.md` exists:
   - If missing → stop and inform user: "Brief not found. Run `/gen-brief <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
7. Check `workspace/<folder-name>/ac_<slug>.md` exists:
   - If missing → stop and inform user: "Acceptance Criteria not found. Run `/gen-ac <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
8. If `workspace/<folder-name>/navigation_<slug>.md` exists → read it as additional context.
9. Check for existing downstream documents in `workspace/<folder-name>/`:
   - Look for: `ba_doc_<slug>.md`
   - If any exist → warn the user:
     > "The following downstream documents already exist and will become outdated if UI Behavior is regenerated:
     > [list each file found]
     > Regenerating UI Behavior will delete these files. Continue? (yes/no)"
   - **no** → stop. Do not generate.
   - **yes** → delete the listed downstream files, then continue.
10. Check if `project/reference/ui-behavior/` exists and contains any `.md` files:
    - If files exist → read all of them as shared UI behavior standards. Use them only to determine which groups are relevant to this feature — do not generate UI behavior entries directly from them.
    - If the folder is empty or does not exist → skip, proceed normally.
11. Read `framework/styles/style_general.md` — general writing style rules.
12. Read `framework/styles/style_ui_behavior.md` — style rules specific to UI Behavior.
13. Read `framework/rules/rule_ui_behavior.md` — writing quality rules for UI Behavior content.

## Steps

1. Analyze the feature source (env file, brief, AC, navigation, and context files) to identify feature-specific UI behavior:

   - Visibility rules based on permission or user role
   - Read-only or editable states of fields
   - Conditional display based on record status
   - Disabled states and when they apply

   Do not include:
   - Generic UI standards covered by shared reference files
   - Validation logic (→ AC)
   - Business policies (→ Business Rules)
   - Navigation paths (→ Navigation)
   - Flow steps (→ Flow)

2. If shared reference files were loaded in step 10, identify which groups are relevant to this feature:
   - A group is relevant when the feature clearly contains UI elements governed by that group (e.g. table, edit form, page header, sidebar).
   - Only include groups that are evidenced by the feature source.
   - Do not infer or add groups that are not clearly used by the feature.

3. Write the document:
   - List all feature-specific UI behavior entries first, numbered UI1, UI2, …
   - Then append one reference line per relevant shared group, continuing the numbering sequence.
   - Omit the shared references block entirely if no shared reference files were loaded or no groups are relevant.

4. Create `workspace/<folder-name>/ui_behavior_<slug>.md` using the format defined in `framework/styles/style_ui_behavior.md`.
   - If no UI behavior was identified, still create the file with the section heading but write: `No UI behavior identified for this feature.`

5. Confirm:
```
✓ workspace/<folder-name>/ui_behavior_<slug>.md

Review the UI Behavior and edit if needed.
```
