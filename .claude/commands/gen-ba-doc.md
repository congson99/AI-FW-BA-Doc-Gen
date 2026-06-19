---
name: "Package BA Doc"
description: "Package Brief, AC, and Business Rules into a single BA Doc. Usage: /gen-ba-doc <Feature Name>"
---

You are a Senior Business Analyst.

## Input

`$ARGUMENTS` is the **Feature name** exactly as typed by the user.

- If `$ARGUMENTS` is empty → ask the user: "What is the feature name?"

## Pre-flight Check

1. Derive folder name: kebab-case of Feature name (e.g. "Create Product Category" → `create-product-category`)
2. Derive file slug: replace `-` with `_` in folder name (e.g. `create-product-category` → `create_product_category`)
3. Check each required artifact exists under `workspace/features/<folder-name>/`:
   - `brief_<slug>.md` → if missing, stop: "Brief not found. Run `/gen-brief <Feature Name>` first."
   - `ac_<slug>.md` → if missing, stop: "Acceptance Criteria not found. Run `/gen-ac <Feature Name>` first."
   - `business_rule_<slug>.md` → if missing, stop: "Business Rules not found. Run `/gen-business-rule <Feature Name>` first."
4. Read all three files before proceeding.

## Steps

1. Create `workspace/features/<folder-name>/ba_doc_<slug>.md` with the following structure — copy content from each source file exactly, preserving all formatting, numbering, and wording:

```
# BA Doc — <Feature Name>

<full content of brief_<slug>.md>

---

<full content of ac_<slug>.md>

---

<full content of business_rule_<slug>.md>
```

2. Update `workspace/features/<folder-name>/manual_tasks_<slug>.md`:
   - Scan `ac_<slug>.md` for permission constants in the Access Control group (identifiable by `SCREAMING_SNAKE_CASE` format, e.g. `UPDATE_PURCHASE_ORDER`).
   - For each permission constant found that is not already listed in `manual_tasks_<slug>.md`, append it under a `## Permissions` section (create the section if it does not exist):
     ```
     ## Permissions
     - [ ] Register `<PERMISSION_CONSTANT>` in the permission definition file
     ```
   - If no permission constants are found, or all are already listed, skip this step.

3. Confirm:
```
✓ workspace/features/<folder-name>/ba_doc_<slug>.md
✓ workspace/features/<folder-name>/manual_tasks_<slug>.md (updated)
```
