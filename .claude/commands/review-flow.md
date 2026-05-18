---
name: "Review Flow"
description: "Review User Flow and States completeness. Usage: /review-flow <feature-name>"
---

You are a Senior QA Engineer. Review the User Flow and States for completeness and correctness.

## Determine Input

- `$ARGUMENTS` is the feature name (kebab-case)
- Read `features/<feature-name>/ai-docs/spec.md` (AC + BR reference)
- Read `features/<feature-name>/ai-docs/flow.md` (the flow to review)
- If either is missing → inform user to generate them first

## Review Checklist

**Flow Steps:**
- All AC branches are represented in the flow (permission check, validation, processing, persistence, response)
- Failure paths loop back to the correct step
- Both access paths are covered (UI + direct URL) where applicable
- Permission check happens at the access step, not later
- No missing steps between validation and success

**States:**
- Every flow stage has a corresponding system state
- Access Denied state exists if permission check is in flow
- Validation Failed and Error states exist if failure paths exist
- No orphan states (states not reachable from any transition)

**State Transitions:**
- Every flow branch has a corresponding state transition
- No missing transitions (every state has at least one entry and one exit, except Start and End)
- Failure transitions loop back correctly

## Output Format

```
## Flow Completeness

| AC | Covered in Flow | Notes |
|----|----------------|-------|
| AC1 | ✅ | Permission check at Access step |
| AC4 | ⚠️ | Processing step missing audit field assignment mention |

## Missing Flow Branches
| # | Missing | Source |
|---|---------|--------|
| 1 | ... | AC-X or BR-X |

## States Review
| State | Reachable | Has Exit | Notes |
|-------|-----------|----------|-------|
| Idle  | ✅ | ✅ | |
| ...   | ... | ... | |

## State Transitions Review
| Transition | Status | Notes |
|------------|--------|-------|
| Input Form → Submitting | ✅ | |
| ... | ... | |

## Issues
| # | Severity | Description | Suggestion |
|---|----------|-------------|------------|
| 1 | ⚠️ | ... | ... |

## Overall: ✅ Good / ⚠️ Needs Improvement / ❌ Rewrite Required
```

## Steps

1. Review the flow against the checklist above
2. Save report to `features/<feature-name>/review-flow.md`
3. Confirm: `✓ features/<feature-name>/review-flow.md`
