---
name: "Generate Messages"
description: "Generate Messages for a feature. Usage: /gen-messages <Feature Name>"
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
   - If any line still contains an unfilled placeholder (e.g. `<additional-context-file-or-confluence-url>`) → stop and inform user: "**Context files** in `env_<slug>.md` still has unfilled placeholders. Either fill them in or remove the placeholder lines, then re-run /gen-messages."
   - An empty **Context files:** section (no items listed) is allowed — continue.
5. Read `workspace/<folder-name>/env_<slug>.md` and load every file listed under **Context files** — read each one before proceeding.
6. Check `workspace/<folder-name>/brief_<slug>.md` exists:
   - If missing → stop and inform user: "Brief not found. Run `/gen-brief <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
7. Check `workspace/<folder-name>/ac_<slug>.md` exists:
   - If missing → stop and inform user: "Acceptance Criteria not found. Run `/gen-ac <Feature Name>` first to generate it."
   - If exists → read it before proceeding.
8. If `workspace/<folder-name>/business_rule_<slug>.md` exists → read it as additional context.
9. If `workspace/<folder-name>/data_definition_<slug>.md` exists → read it as additional context.
10. If `workspace/<folder-name>/flow_<slug>.md` exists → read it as additional context.
11. Check for existing downstream documents in `workspace/<folder-name>/`:
    - Look for: `ba_doc_<slug>.md`
    - If any exist → warn the user:
      > "The following downstream documents already exist and will become outdated if Messages is regenerated:
      > [list each file found]
      > Regenerating Messages will delete these files. Continue? (yes/no)"
    - **no** → stop. Do not generate.
    - **yes** → delete the listed downstream files, then continue.
12. Check if `project/reference/messages/` exists and contains any `.md` files:
    - If files exist → read all of them as shared message standards or wording templates. Apply them when writing message text.
    - If the folder is empty or does not exist → skip, proceed normally.
13. Read `framework/styles/style_general.md` — general writing style rules.
14. Read `framework/styles/style_messages.md` — style rules specific to Messages.
15. Read `framework/rules/rule_messages.md` — writing quality rules for Messages content.

## Steps

1. Analyze all loaded source (env file, brief, AC, business rules, data definition, flow, and context files) to identify message cases:

   - Permission errors (from Access Control in AC)
   - Validation errors (required fields, invalid references, format, range, attachment, item list)
   - Confirmation dialogs
   - Business errors (auto-generation failures, business constraint violations)
   - System errors (processing failures, persistence failures, system failures)
   - Success messages

   For each case, determine:
   - The triggering condition (Case)
   - Message Type: `Validation Error`, `Error`, `Success`, or `Confirmation`
   - Source: `BE`, `FE`, or `BE / FE`
   - Where the message appears in the UI (UI Display)
   - The exact message wording (Message) — use source wording when provided

2. Order rows:
   - Permission errors first
   - Validation errors in field order (top to bottom, left to right as they appear in the UI)
   - Confirmation dialogs
   - Business errors
   - System errors
   - Success messages last

3. Create `workspace/<folder-name>/messages_<slug>.md` using the format defined in `framework/styles/style_messages.md`.
   - If no messages were identified, still create the file with the section heading but write: `No messages identified for this feature.`

4. Confirm:
```
✓ workspace/<folder-name>/messages_<slug>.md

Review the Messages and edit if needed.
```
