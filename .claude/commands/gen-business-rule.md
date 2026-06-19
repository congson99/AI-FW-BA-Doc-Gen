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
3. Check `workspace/features/<folder-name>/env_<slug>.md` exists:
   - If missing → stop and inform user: "Run `/start <Feature Name>` first to set up the environment."
4. Check the **Context files:** section in the env file:
   - If any line still contains an unfilled placeholder (e.g. `<additional-context-file-or-confluence-url>`) → stop and inform user: "**Context files** in `env_<slug>.md` still has unfilled placeholders. Either fill them in or remove the placeholder lines, then re-run /gen-business-rule."
   - An empty **Context files:** section (no items listed) is allowed — continue.
5. Read `workspace/features/<folder-name>/env_<slug>.md` and load every file listed under **Context files** — read each one before proceeding.
6. Check `workspace/features/<folder-name>/brief_<slug>.md` exists:
   - If missing → stop and inform user: "Brief not found. Run `/gen-brief <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
7. Check `workspace/features/<folder-name>/ac_<slug>.md` exists:
   - If missing → stop and inform user: "Acceptance Criteria not found. Run `/gen-ac <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
8. Check for conflicts across all loaded context files, brief, and AC:
   - Look for contradictions in business rules, field definitions, status flows, permissions, or scope between any two sources.
   - If conflicts are found → stop and list each conflict clearly: "Conflicts found: [describe each conflict and which files it involves]. Resolve before running /gen-business-rule."
   - If no conflicts → continue.
9. Check for existing downstream documents in `workspace/features/<folder-name>/`:
   - Look for: `ba_doc_<slug>.md`
   - If any exist → warn the user:
     > "The following downstream documents already exist and will become outdated if Business Rules are regenerated:
     > [list each file found]
     > Regenerating Business Rules will delete these files. Continue? (yes/no)"
   - **no** → stop. Do not generate.
   - **yes** → delete the listed downstream files, then continue.
10. Read `framework/styles/style_general.md` — general writing style rules.
11. Read `framework/styles/style_business_rule.md` — style rules specific to Business Rules.
12. Read `framework/rules/rule_business_rule.md` — writing quality rules for Business Rules content.

## Steps

1. Analyze all loaded context (env file, brief, AC, context files) to identify business rules:
   - Permission enforcement rules
   - Numbering / auto-generation rules
   - Status assignment rules
   - Cross-field or cross-entity rules
   - Concurrency rules
   - Rollback rules
   - Any other business-level constraints present in the source

2. Create `workspace/features/<folder-name>/business_rule_<slug>.md` using the format defined in `framework/styles/style_business_rule.md`.

3. Confirm:
```
✓ workspace/features/<folder-name>/business_rule_<slug>.md

Review the Business Rules and edit if needed.
```
