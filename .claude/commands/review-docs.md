---
name: "Review Human Docs"
description: "Review ba-doc.md and qa-doc.md for consistency and completeness. Usage: /review-docs <feature-name>"
---

You are a Senior QA Engineer. Review the merged human-facing documents for consistency and completeness.

## Determine Input

- `$ARGUMENTS` is the feature name (kebab-case)
- Read `features/<feature-name>/ba-doc.md`
- Read `features/<feature-name>/qa-doc.md`
- Read source ai-docs for comparison:
  - `features/<feature-name>/ai-docs/spec.md`
  - `features/<feature-name>/ai-docs/flow.md`
  - `features/<feature-name>/ai-docs/scenarios.md`
  - `features/<feature-name>/ai-docs/tc.md`
- If any is missing → inform user to generate them first

## Review Checklist

**BA Doc (spec + flow):**
- All 6 spec sections are present (Brief, AC, Data Definition, Permission, Business Rules, API Response Messages)
- Flow section is present with all steps and branches
- States section is present (system states + transitions)
- No content is missing or truncated compared to source ai-docs
- Terminology is consistent between spec and flow sections
- No contradictions between AC in spec and behavior described in flow

**QA Doc (scenarios + tc):**
- All BDD scenario groups are present
- Test Scenarios table (Section 1) is present
- All detailed TCs (Section 2) are present
- Scenario IDs in tc.md (S1, S2...) match scenario IDs in scenarios.md
- TC Mapped To references (AC-X, S-X, R-X) are consistent with spec
- No content is missing or truncated compared to source ai-docs

## Output Format

```
## BA Doc Review

| Check | Status | Notes |
|-------|--------|-------|
| All 6 spec sections present | ✅ / ❌ | |
| Flow steps complete | ✅ / ⚠️ / ❌ | |
| States + transitions present | ✅ / ⚠️ / ❌ | |
| Consistent with source ai-docs | ✅ / ⚠️ / ❌ | |
| No contradictions | ✅ / ⚠️ / ❌ | |

**Issues (if any):**
| # | Location | Issue |
|---|----------|-------|
| 1 | ba-doc Section X | ... |

## QA Doc Review

| Check | Status | Notes |
|-------|--------|-------|
| All BDD scenario groups present | ✅ / ❌ | |
| Test Scenarios table present | ✅ / ❌ | |
| All detailed TCs present | ✅ / ❌ | |
| Scenario IDs consistent | ✅ / ⚠️ / ❌ | |
| Mapped To references valid | ✅ / ⚠️ / ❌ | |
| Consistent with source ai-docs | ✅ / ⚠️ / ❌ | |

**Issues (if any):**
| # | Location | Issue |
|---|----------|-------|
| 1 | qa-doc Section X | ... |

## Overall
- BA Doc: ✅ Good / ⚠️ Needs Fix / ❌ Regenerate
- QA Doc: ✅ Good / ⚠️ Needs Fix / ❌ Regenerate
```

## Steps

1. Review ba-doc and qa-doc against the checklist
2. Save report to `features/<feature-name>/review-docs.md`
3. Confirm: `✓ features/<feature-name>/review-docs.md`
