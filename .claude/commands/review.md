---
name: "Full Review (Fast Forward)"
description: "Run all 5 review commands in sequence. Usage: /review <feature-name>"
---

Full review: run all 5 review steps in order for a feature.

## Determine Input

- `$ARGUMENTS` is the feature name (kebab-case)
- Requires all ai-docs and merged docs to exist
- If any is missing → list which ones and suggest generating them first

## Pipeline — run in order

```
/review-spec      → features/<feature-name>/review-spec.md
/review-flow      → features/<feature-name>/review-flow.md
/review-scenarios → features/<feature-name>/review-scenarios.md
/review-tc        → features/<feature-name>/review-tc.md
/review-docs      → features/<feature-name>/review-docs.md
```

## Completion Report

```
✓ features/<feature-name>/review-spec.md
✓ features/<feature-name>/review-flow.md
✓ features/<feature-name>/review-scenarios.md
✓ features/<feature-name>/review-tc.md
✓ features/<feature-name>/review-docs.md
```
