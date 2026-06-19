---
name: "Generate Brief"
description: "Generate Brief for a feature from chat input. Usage: /gen-brief <Feature Name>"
---

You are a Senior Business Analyst.

## Input

`$ARGUMENTS` is the **Feature name** exactly as typed by the user.

- If `$ARGUMENTS` is empty → ask the user: "What is the feature name?"
- If the feature description in the conversation is too vague to determine In Scope / Out of Scope → ask one focused clarifying question before generating.

## Pre-flight Check

1. Derive folder name: kebab-case of Feature name (e.g. "Create Product Category" → `create-product-category`)
2. Derive file slug: replace `-` with `_` in folder name (e.g. `create-product-category` → `create_product_category`)
3. Check `workspace/features/<folder-name>/env_<slug>.md` exists:
   - If missing → stop and inform user: "Run `/start <Feature Name>` first to set up the environment, then fill in the placeholders before running /gen-brief."
   - If exists but still contains unfilled placeholders (e.g. `<jira-ticket-url>`) → warn the user but continue generating.
4. Check the **Context files:** section in the env file:
   - If any line still contains an unfilled placeholder (e.g. `<additional-context-file-or-confluence-url>`) → stop and inform user: "**Context files** in `env_<slug>.md` still has unfilled placeholders. Either fill them in or remove the placeholder lines, then re-run /gen-brief."
   - An empty **Context files:** section (no items listed) is allowed — continue.
5. Read `workspace/features/<folder-name>/env_<slug>.md` and load every file listed under **Context files** — read each one before proceeding.
6. If `idea_<slug>.md` was loaded and its content still contains `<add feature description here>` → stop and inform user: "Fill in `workspace/features/<folder-name>/idea_<slug>.md` with the feature description before running /gen-brief."
7. Check for conflicts across all loaded context files:
   - Look for contradictions in business rules, field definitions, status flows, permissions, or scope between any two context files.
   - If conflicts are found → stop and list each conflict clearly: "Conflicts found in context files: [describe each conflict and which files it involves]. Resolve before running /gen-brief."
   - If no conflicts → continue.
8. Check for existing downstream documents in `workspace/features/<folder-name>/`:
   - Look for: `ac_<slug>.md`, `business_rule_<slug>.md`, `ba_doc_<slug>.md`
   - If any exist → warn the user:
     > "The following downstream documents already exist and will become outdated if the Brief is regenerated:
     > [list each file found]
     > Regenerating the Brief will delete these files. Continue? (yes/no)"
   - **no** → stop. Do not generate.
   - **yes** → delete the listed downstream files, then continue.
9. Read `framework/styles/style_general.md` — general writing style rules.
10. Read `framework/styles/style_brief.md` — style rules specific to Brief.
11. Read `framework/rules/rule_brief.md` — writing quality rules for brief content.

## Steps

1. Create `workspace/features/<folder-name>/brief_<slug>.md` using the format defined in `framework/styles/style_brief.md`, filling in:
   - **Feature name** — from `$ARGUMENTS` directly (no modification)
   - **Goal** — one sentence derived from the user's description in chat
   - **In Scope** — derived from the user's description
   - **Out of Scope** — derived from the user's description

2. Confirm:
```
✓ workspace/features/<folder-name>/brief_<slug>.md

Review the brief and edit if needed, then run /gen-ac <Feature Name> to continue.
```

