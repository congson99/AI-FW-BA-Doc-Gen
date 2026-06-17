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
   - If missing → stop and inform user: "Run `/gen-ba <Feature Name>` first to set up the environment, then fill in the placeholders before running /gen-brief."
   - If exists but still contains unfilled placeholders (e.g. `<jira-ticket-url>`) → warn the user but continue generating.
4. Check the **Context files:** section in the env file:
   - If any line still contains an unfilled placeholder (e.g. `<additional-context-file-or-confluence-url>`) → stop and inform user: "**Context files** in `env_<slug>.md` still has unfilled placeholders. Either fill them in or remove the placeholder lines, then re-run /gen-brief."
   - An empty **Context files:** section (no items listed) is allowed — continue.
5. Read `workspace/features/<folder-name>/env_<slug>.md` and load every file listed under **Context files** — read each one before proceeding.
5. Read `framework/rules/rule_brief.md` — writing quality rules for brief content.

## Steps

1. Take:
   - **Feature name** — from `$ARGUMENTS` directly (no modification)
   - **Goal** — generate one sentence from the user's description in chat
   - **In Scope** — derive from the user's description
   - **Out of Scope** — derive from the user's description

2. Create `workspace/features/<folder-name>/brief_<slug>.md` with this exact format:

```
# 1. Brief

**Feature name:** <Feature name>

**Goal:** <one sentence>

**In scope:**
- <item>

**Out of scope:**
- <item>
```

3. Confirm:
```
✓ workspace/features/<folder-name>/brief_<slug>.md

Review the brief and edit if needed, then run /generate-spec <folder-name> to continue.
```

