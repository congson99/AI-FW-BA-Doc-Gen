---
name: "Review Test Cases"
description: "Review TC coverage and quality. Usage: /review-tc <feature-name>"
---

You are a Senior QA Engineer. Review the Test Cases for completeness and quality.

## Determine Input

- `$ARGUMENTS` is the feature name (kebab-case)
- Read `features/<feature-name>/ai-docs/spec.md` (AC + BR reference)
- Read `features/<feature-name>/ai-docs/scenarios.md` (BDD reference)
- Read `features/<feature-name>/ai-docs/tc.md` (the TCs to review)
- If any is missing → inform user to generate them first

## Steps

1. Review the test cases for:

   **Coverage:**
   - Every AC has at least one TC mapped to it
   - Every BDD scenario has at least one TC mapped to it
   - Every Business Rule has at least one TC
   - Every validation rule (required, max length, format, unique) has boundary TCs

   **Quality per TC:**
   - Preconditions clearly define user role and data state
   - Steps are numbered and actionable
   - Test Data table is complete with realistic values and type labels (Valid / Invalid / Boundary)
   - Expected Result covers both Functional and UI behavior
   - Priority (P0–P3) is justified
   - Automatable flag is accurate

2. Produce a coverage matrix: AC → TC(s) mapped, with gaps highlighted
3. Save report to `features/<feature-name>/review-tc.md`
4. Confirm: `✓ features/<feature-name>/review-tc.md`

## Output Format

```
## Coverage Matrix

| AC | TC(s) | Status |
|----|-------|--------|
| AC1 | TC-001, TC-005 | ✅ Full |
| AC3 | TC-008 | ⚠️ Partial — missing boundary test |
| AC7 | — | ❌ Not Covered |

## Missing TC Candidates
| # | AC | Missing | Priority |
|---|----|---------| ---------|
| 1 | AC7 | Concurrency test | P0 |

## TC Quality Issues
| TC | Issue | Suggestion |
|----|-------|-----------|
| TC-003 | Test Data missing boundary value | Add 255-char and 256-char variations |
```
