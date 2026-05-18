---
name: "Generate Merged Docs"
description: "Merge ai-docs into ba-doc.md and qa-doc.md. Usage: /generate-docs <feature-name>"
---

Merge the 4 AI-generated artifacts into 2 consolidated documents.

## Determine Input

- `$ARGUMENTS` is the feature name (kebab-case)
- Requires all 4 ai-docs to exist:
  - `features/<feature-name>/ai-docs/spec.md`
  - `features/<feature-name>/ai-docs/flow.md`
  - `features/<feature-name>/ai-docs/scenarios.md`
  - `features/<feature-name>/ai-docs/tc.md`
- If any is missing → list which ones are missing and suggest running the corresponding generate command

## Steps

1. Read `features/<feature-name>/ai-docs/spec.md`
2. Read `features/<feature-name>/ai-docs/flow.md`
3. Merge spec + flow → save as `features/<feature-name>/ba-doc.md`

4. Read `features/<feature-name>/ai-docs/scenarios.md`
5. Read `features/<feature-name>/ai-docs/tc.md`
6. Merge scenarios + tc → save as `features/<feature-name>/qa-doc.md`

7. Confirm:
```
✓ features/<feature-name>/ba-doc.md   (spec + flow)
✓ features/<feature-name>/qa-doc.md   (scenarios + tc)
```

## Notes
- ba-doc.md = exactly spec.md content followed by flow.md content, no modifications
- qa-doc.md = exactly scenarios.md content followed by tc.md content, no modifications
