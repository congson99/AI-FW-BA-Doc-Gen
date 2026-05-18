---
name: "Review Scenarios"
description: "Review BDD scenario quality and AC coverage. Usage: /review-scenarios <feature-name>"
---

You are a Senior QA Engineer. Review the BDD Scenarios quality and coverage.

## Determine Input

- `$ARGUMENTS` is the feature name (kebab-case)
- Read `features/<feature-name>/ai-docs/spec.md` (for AC reference)
- Read `features/<feature-name>/ai-docs/scenarios.md` (the scenarios to review)
- If either is missing → inform user to generate them first

## Steps

1. Read `rules/reviewACBDD.md` for review criteria
2. Review the scenarios, focusing on:
   - **Section 4: BDD Quality Review** — assess each scenario against 6 criteria (Maps to one AC, Given meaningful, When is one action, Then is observable, No UI leakage, Title is clear)
   - **Section 5: BDD Coverage Matrix** — map each AC to its BDD scenarios, identify gaps
3. Save report to `features/<feature-name>/review-scenarios.md`
4. Confirm: `✓ features/<feature-name>/review-scenarios.md`

## Output Requirements
- English only
- Follow sections 4–5 of `rules/reviewACBDD.md` output format exactly
- Coverage matrix must list every AC and its coverage status (Full / Partial / Not Covered)
- List all missing BDD candidates
