---
name: "Generate Flow"
description: "Generate Flow for a feature. Usage: /gen-flow <Feature Name>"
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
   - If any line still contains an unfilled placeholder (e.g. `<additional-context-file-or-confluence-url>`) → stop and inform user: "**Context files** in `env_<slug>.md` still has unfilled placeholders. Either fill them in or remove the placeholder lines, then re-run /gen-flow."
   - An empty **Context files:** section (no items listed) is allowed — continue.
5. Read `workspace/<folder-name>/env_<slug>.md` and load every file listed under **Context files** — read each one before proceeding.
6. Check `workspace/<folder-name>/brief_<slug>.md` exists:
   - If missing → stop and inform user: "Brief not found. Run `/gen-brief <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
7. Check `workspace/<folder-name>/ac_<slug>.md` exists:
   - If missing → stop and inform user: "Acceptance Criteria not found. Run `/gen-ac <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
8. Check `workspace/<folder-name>/business_rule_<slug>.md` exists:
   - If missing → stop and inform user: "Business Rules not found. Run `/gen-business-rule <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
9. Check for existing downstream documents in `workspace/<folder-name>/`:
   - Look for: `ba_doc_<slug>.md`
   - If any exist → warn the user:
     > "The following downstream documents already exist and will become outdated if Flow is regenerated:
     > [list each file found]
     > Regenerating Flow will delete these files. Continue? (yes/no)"
   - **no** → stop. Do not generate.
   - **yes** → delete the listed downstream files, then continue.
10. Check if `project/reference/flow/` exists and contains any `.md` files:
    - If files exist → read all of them as reference guidelines before proceeding. These files define shared flow patterns and conventions that apply across features. Use them to inform generation — do not extract flow steps directly from them.
    - If the folder is empty or does not exist → skip, proceed normally.
11. Read `framework/styles/style_general.md` — general writing style rules.
12. Read `framework/styles/style_flow.md` — style rules specific to Flow.
13. Read `framework/rules/rule_flow.md` — writing quality rules for Flow content.

## Steps

1. Analyze the feature source (env file, brief, AC, business rules, and context files) to identify:

   - How the feature is triggered (user action, external event, or scheduled/system event explicitly described)
   - Preconditions that must be true before the feature can proceed
   - The successful end-to-end behavior at a business level
   - Alternate flows explicitly described in the source
   - Supporting interactions that should be modeled as Secondary Flows according to the Flow rules

   Include:
   - Feature entry interactions explicitly described in the source (e.g. opening a page, opening a dialog, loading existing data)

   Ignore:
   - Navigation paths between pages
   - UI layout or component structure
   - Field-level validation details (already in AC)
   - Business rule details (already in Business Rules)
   - Data definition details (already in Data Definition)
   - Message wording
   - Database operations or schema details
   - API endpoints or HTTP methods
   - Internal services or microservice architecture
   - Technical algorithms or implementation details

2. Write the document in this order:
   - **Entry**: describe the trigger event and preconditions. Do not use `[Start]`/`[End]` markers.
   - **Main Flow**: describe the successful end-to-end behavior using `[Start]`/`[End]` markers. When the source describes user interactions before submission, preserve them (e.g. User opens page → System loads data → User modifies information → User submits request). Summarize validation as a single step unless the source explicitly expands it. Do not repeat AC, Business Rules, or Data Definition details.
   - **Alternate Flows** (if any): one `#### [Name]` block per alternate flow using `[Start]`/`[End]` markers.
   - **Secondary Flows** (if any): one `#### [Name]` block per secondary flow using `[Start]`/`[End]` markers.
   - Omit Alternate Flows and Secondary Flows sections entirely if none are identified.

3. Create `workspace/<folder-name>/flow_<slug>.md` using the format defined in `framework/styles/style_flow.md`.
   - If no flow steps were identified, still create the file with the section heading but write: `No flow identified for this feature.`

4. Confirm:
```
✓ workspace/<folder-name>/flow_<slug>.md

Review the Flow and edit if needed.
```
