---
name: "Generate Navigation"
description: "Generate Navigation for a feature. Usage: /gen-navigation <Feature Name>"
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
   - If any line still contains an unfilled placeholder (e.g. `<additional-context-file-or-confluence-url>`) → stop and inform user: "**Context files** in `env_<slug>.md` still has unfilled placeholders. Either fill them in or remove the placeholder lines, then re-run /gen-navigation."
   - An empty **Context files:** section (no items listed) is allowed — continue.
5. Read `workspace/<folder-name>/env_<slug>.md` and load every file listed under **Context files** — read each one before proceeding.
6. Check `workspace/<folder-name>/brief_<slug>.md` exists:
   - If missing → stop and inform user: "Brief not found. Run `/gen-brief <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
7. Check `workspace/<folder-name>/ac_<slug>.md` exists:
   - If missing → stop and inform user: "Acceptance Criteria not found. Run `/gen-ac <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
8. Check for existing downstream documents in `workspace/<folder-name>/`:
   - Look for: `ba_doc_<slug>.md`
   - If any exist → warn the user:
     > "The following downstream documents already exist and will become outdated if Navigation is regenerated:
     > [list each file found]
     > Regenerating Navigation will delete these files. Continue? (yes/no)"
   - **no** → stop. Do not generate.
   - **yes** → delete the listed downstream files, then continue.
9. Check if `project/reference/navigation/` exists and contains any `.md` files:
   - If files exist → read all of them as reference guidelines before proceeding. These files define shared navigation patterns, button naming conventions, confirmation dialog rules, and similar standards that apply across features. Use them to inform generation — do not extract navigation actions directly from them.
   - If the folder is empty or does not exist → skip, proceed normally.
10. Read `framework/styles/style_general.md` — general writing style rules.
11. Read `framework/styles/style_navigation.md` — style rules specific to Navigation.
12. Read `framework/rules/rule_navigation.md` — writing quality rules for Navigation content.

## Steps

1. Analyze the feature source (env file, brief, AC, and context files) to identify the navigation for this feature. Apply the reference guidelines from `project/reference/navigation/` (shared patterns, button conventions, confirmation rules) when making decisions — do not extract navigation actions from those files. Identify:

   - Entry pages that lead to the feature
   - Main page (the primary page for the feature)
   - Dialogs or sub-pages opened from the main page
   - User-triggered navigation actions on each page (button clicks, links)
   - Confirmation dialogs triggered by unsaved changes (if mentioned in the source)

   Ignore:
   - Workflow steps
   - System processing
   - Business logic
   - Data persistence behavior
   - Response messages
   - Validation behavior
   - Permission enforcement
   - UI layout or component structure
   - System-initiated redirects not explicitly described in the source
   - User actions that do not change page, dialog, or view (e.g. sort, filter, select checkbox, toast messages, validation popups)

2. For each page or dialog identified, list all navigation actions in the format defined in `framework/styles/style_navigation.md`:
   - Output sections in order: Entry Page(s) → Main Page → Dialog/Sub-page(s).
   - Omit a section entirely if it has no navigation actions — do not create an empty heading.
   - Do not infer page names or actions not described in the source.

3. Create `workspace/<folder-name>/navigation_<slug>.md` using the format defined in `framework/styles/style_navigation.md`.
   - If no navigation actions were identified, still create the file with the section heading but write: `No navigation identified for this feature.`

4. Confirm:
```
✓ workspace/<folder-name>/navigation_<slug>.md

Review the Navigation and edit if needed.
```
