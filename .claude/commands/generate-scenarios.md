---
name: "Generate Scenarios"
description: "Generate BDD Scenarios for a feature. Usage: /generate-scenarios <feature-name>"
---

You are a Senior Business Analyst / QA.

## Determine Input

- `$ARGUMENTS` is the feature name (kebab-case)
- Requires: `features/<feature-name>/ai-docs/spec.md` AND `features/<feature-name>/ai-docs/flow.md`
- If either is missing → inform user to run the missing generate command first

## Steps

1. Read `rules/rule_scenarios.md`
2. Read `templates/scenarios.md` as reference format
3. Read `features/<feature-name>/ai-docs/spec.md` — every AC must be covered
4. Read `features/<feature-name>/ai-docs/flow.md` — every flow branch must be covered
5. Generate `features/<feature-name>/ai-docs/scenarios.md` grouped by:
   - Happy Path
   - Alternative Flows
   - Permission Scenarios
   - Validation Scenarios (per field)
   - Business Rule Scenarios
   - Data Validation / Input Transformation
   - Data Integrity / Audit Fields
   - System Error Scenarios
   - Data Consistency
   - Concurrency (if applicable)
6. Confirm: `✓ features/<feature-name>/ai-docs/scenarios.md`

## Output Requirements
- English only
- Follow `rules/rule_scenarios.md` exactly
- Match format and structure of `templates/scenarios.md`
- 100% AC coverage, 100% flow branch coverage
