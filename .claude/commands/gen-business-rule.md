---
name: "Generate Business Rules"
description: "Generate Business Rules for a feature. Usage: /gen-business-rule <Feature Name>"
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
   - If any line still contains an unfilled placeholder (e.g. `<additional-context-file-or-confluence-url>`) → stop and inform user: "**Context files** in `env_<slug>.md` still has unfilled placeholders. Either fill them in or remove the placeholder lines, then re-run /gen-business-rule."
   - An empty **Context files:** section (no items listed) is allowed — continue.
5. Read `workspace/<folder-name>/env_<slug>.md` and load every file listed under **Context files** — read each one before proceeding.
6. Check `workspace/<folder-name>/brief_<slug>.md` exists:
   - If missing → stop and inform user: "Brief not found. Run `/gen-brief <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
7. Check `workspace/<folder-name>/ac_<slug>.md` exists:
   - If missing → stop and inform user: "Acceptance Criteria not found. Run `/gen-ac <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
8. Check for conflicts across all loaded context files, brief, and AC:
   - Look for contradictions in business rules, field definitions, status flows, permissions, or scope between any two sources.
   - If conflicts are found → stop and list each conflict clearly: "Conflicts found: [describe each conflict and which files it involves]. Resolve before running /gen-business-rule."
   - If no conflicts → continue.
9. Check for existing downstream documents in `workspace/<folder-name>/`:
   - Look for: `ba_doc_<slug>.md`
   - If any exist → warn the user:
     > "The following downstream documents already exist and will become outdated if Business Rules are regenerated:
     > [list each file found]
     > Regenerating Business Rules will delete these files. Continue? (yes/no)"
   - **no** → stop. Do not generate.
   - **yes** → delete the listed downstream files, then continue.
10. Check `project/reference/business-rules/principles/` for `.md` files:
    - If files exist → read all of them. Apply these principles when analyzing the feature to identify business rules (step 1). Do not copy principle content into the output.
    - If the folder is empty or does not exist → skip.
11. Check `project/reference/business-rules/shared-references/` for `.md` files:
    - If files exist → read all of them and note which rule groups each file contains (e.g. Email, Phone Number, Attachment, Pagination). These groups will be appended as reference lines in the output.
    - If the folder is empty or does not exist → skip.
12. Read `framework/styles/style_general.md` — general writing style rules.
13. Read `framework/styles/style_business_rule.md` — style rules specific to Business Rules.
14. Read `framework/rules/rule_business_rule.md` — writing quality rules for Business Rules content.

## Steps

1. Analyze all loaded context (env file, brief, AC, and context files) to identify business rules, including:

   - Permission enforcement rules
   - Numbering or auto-generation rules
   - Uniqueness rules
   - Status assignment rules
   - State transition rules
   - Calculation rules
   - Cross-field rules
   - Cross-entity rules
   - External dependency rules
   - Concurrency rules
   - Record consistency rules across related business objects
   - Any other business policies or constraints explicitly present in the source

   Apply any principles loaded from `principles/` as guiding rules during this analysis.

   Determine whether each candidate rule represents a business policy or business constraint.

   Exclude behaviors that belong to Acceptance Criteria, including validation, search, processing, persistence, response, audit, notification, and UI behavior.

2. If any files from `project/reference/business-rules/shared-references/` were loaded, identify which rule groups from those files are relevant to this feature:
   - A rule group is relevant if the feature involves a field or behavior that the group governs (e.g. a phone number field → Phone Number rules apply).
   - Do not re-list the rules. Instead, for each relevant group, append one rule entry at the end of the numbered list (continuing the R-number sequence), with a blank line between each rule:
     > `**R<N>:** <Rule Group>: follow General Business Rules`
   - Feature-specific rules must always come first. General rule references are appended last, each as its own numbered rule in the same list, following the same blank-line spacing as all other rules.

3. Create `workspace/<folder-name>/business_rule_<slug>.md` using the format defined in `framework/styles/style_business_rule.md`.
   - If no business rules were identified, still create the file with the section heading but write: `No specific business rules identified for this feature.`

4. Confirm:
```
✓ workspace/<folder-name>/business_rule_<slug>.md

Review the Business Rules and edit if needed, then run /gen-data-definition <Feature Name> to continue.
```
