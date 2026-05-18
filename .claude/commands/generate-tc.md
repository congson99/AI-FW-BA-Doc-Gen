---
name: "Generate Test Cases"
description: "Generate Test Cases for a feature. Usage: /generate-tc <feature-name>"
---

You are a Senior QA Engineer.

## Determine Input

- `$ARGUMENTS` is the feature name (kebab-case)
- Requires: `features/<feature-name>/ai-docs/spec.md` AND `features/<feature-name>/ai-docs/scenarios.md`
- If either is missing → inform user to run the missing generate command first

## Steps

1. Read `rules/rule_tc.md`
2. Read `templates/tc.md` as reference format
3. Read `features/<feature-name>/ai-docs/spec.md` — derive AC coverage requirements and validation rules
4. Read `features/<feature-name>/ai-docs/scenarios.md` — each scenario maps to one or more TCs
5. Generate `features/<feature-name>/ai-docs/tc.md` with:
   - Header: `Generated: <date> | Total Test Cases: N | AC Coverage: 100% | BDD Coverage: 100%`
   - Section 1: Test Scenarios table (Scenario ID | Name | Type | Mapped To)
   - Section 2: Detailed Test Cases (Preconditions, Steps, Test Data table, Expected Result)
6. Confirm: `✓ features/<feature-name>/ai-docs/tc.md`

## Output Requirements
- English only
- Follow `rules/rule_tc.md` exactly
- Match format and structure of `templates/tc.md`
- 100% AC coverage, 100% BDD scenario coverage
- Every TC must have realistic test data and clear expected results
