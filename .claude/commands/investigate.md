---
name: "Investigate"
description: "Investigate project context to generate the Idea file for a feature, asking the user for anything missing. Usage: /investigate <Feature Name>"
---

You are a Senior Business Analyst distilling a feature's raw context into a single Idea file. Every later generation step (Brief through Messages) reads only this Idea file and each other's output — not the project context files directly — so it needs to carry enough information for the whole pipeline.

## Input

`$ARGUMENTS` is the **Feature name** exactly as typed by the user.

- If `$ARGUMENTS` is empty → ask the user: "What is the feature name?"

## Pre-flight Check

1. Derive folder name: kebab-case of Feature name (e.g. "Create Product Category" → `create-product-category`)
2. Derive file slug: replace `-` with `_` in folder name (e.g. `create-product-category` → `create_product_category`)
3. Check `workspace/<folder-name>/input/env_<slug>.md` exists:
   - If missing → stop and inform user: "Run `/start <Feature Name>` first to set up the environment."
   - Read it and note the `**Document language:**` value (cached there by `/start`) — if missing, default to English.
4. Check `workspace/<folder-name>/input/context_<slug>.md` exists:
   - If missing → stop and inform user: "Run `/start <Feature Name>` first to set up the environment."
   - If any line still contains an unfilled placeholder (e.g. `<additional-context-file-or-confluence-url>`) → stop and inform user: "`context_<slug>.md` still has unfilled placeholders. Either fill them in or remove the placeholder lines, then re-run /investigate."
   - An empty file (no items listed under `# Context Files`) is allowed — continue.
5. Read `workspace/<folder-name>/input/context_<slug>.md` and load every file it lists — read each one before proceeding.
6. Check for existing downstream documents in `workspace/<folder-name>/`:
   - Look for: `input/idea_<slug>.md`, `docs/brief_<slug>.md`, `docs/ac_<slug>.md`, `docs/business_rule_<slug>.md`, `docs/data_definition_<slug>.md`, `docs/navigation_<slug>.md`, `docs/flow_<slug>.md`, `docs/ui_behavior_<slug>.md`, `docs/messages_<slug>.md`, `ba_doc_<slug>.md`
   - If any exist → warn the user:
     > "The following documents already exist and will become outdated if the Idea file is regenerated:
     > [list each file found]
     > Regenerating the Idea file will delete these files. Continue? (yes/no)"
   - **no** → stop. Do not generate.
   - **yes** → delete the listed downstream files, then continue.

## Steps

Write all descriptive content in the idea file in the Document language noted during Pre-flight Check — keep section headings (e.g. `## Overview`) in English.

Create `workspace/<folder-name>/input/idea_<slug>.md` using a two-pass approach:

**Pass 1 — Fill from context**

Read the context files loaded above and cross-reference the normalized feature name against known features, modules, tickets, and descriptions.

Rules:
- Only fill content that is clearly derivable from the loaded context files. Do not invent or assume anything not found there.
- If a section cannot be determined from context, leave its placeholder text as-is for now (it will be handled in Pass 2).

**Pass 2 — Ask the user for remaining unknowns**

After Pass 1, identify which sections still contain placeholder text. For each such section, ask the user a focused question to gather the missing information. Ask all questions together in one message — do not ask one at a time.

Format the questions clearly, for example:
> A few questions to complete `idea_update_po.md`:
>
> **1. Entities & Fields** — What fields can the user edit on a PO? (e.g. supplier, delivery date, line items)
> **2. Business Rules & Validation** — Any validation rules or constraints for those fields? (e.g. required, format, status transitions)
> **3. Permissions** — What permission constant controls this action? (e.g. `PO_UPDATE`)
> **4. Process Flow** — What are the main steps, from entry point to completion?
>
> Answer what you know — type "skip" for any you want to leave for later.

After the user responds:
- Fill in each answered section with the user's input.
- For any section the user skipped or left blank, keep the original placeholder text.
- Write the final file with all filled and unfilled sections combined.

Template:

```
# Feature Idea — <Feature Name>

## Overview
<describe the feature goal in 1–2 sentences>

## Scope
- In scope: <bullet list>
- Out of scope: <bullet list>

## Entities & Fields
### <Entity>
- User-provided: <field>, <field>
- System-generated: <field>: <how generated or default value>

## Process Flow
1. <main step, from entry point to completion>
2. <step>

## Search & Filters
- Search target: <entity being searched>
- Search by: <field(s)>
- Matching rule: <e.g. partial match on name>

## Business Rules & Validation
- <rule or validation, e.g. required / must be > 0 / status transition constraint>

## Permissions
- <PERMISSION_CONSTANT>

## Notes / Open Questions
<any additional constraints, edge cases, or unresolved questions>
```

## Confirm

```
✓ workspace/<folder-name>/input/idea_<slug>.md

Next: Review idea_<slug>.md, then run /gen-brief <Feature Name> to continue.
```

If any sections still have placeholder text (user skipped them), also note:
```
⚠ idea_<slug>.md has unfilled sections — you can complete them before running /gen-brief, or later gen-* commands will ask about them if the information turns out to be needed.
```
