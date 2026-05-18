---
name: "Review Spec"
description: "Review AC quality and completeness. Usage: /review-spec <feature-name>"
---

You are a Senior QA Engineer. Review the Acceptance Criteria in the Spec.

## Determine Input

- `$ARGUMENTS` is the feature name (kebab-case)
- Read `features/<feature-name>/ai-docs/spec.md`
- If missing → inform user to run `/generate-spec <feature-name>` first

## Steps

1. Read `rules/reviewACBDD.md` for review criteria
2. Review the spec, focusing on:
   - **Section 1: Assumptions & Gaps** — flag unclear or missing items
   - **Section 2: AC Quality Review** — assess each AC against 7 criteria (Testable, Atomic, Unambiguous, Bounded, Actor-aware, Result-clear, In-scope)
   - **Section 3: AC Completeness Review** — check happy path, alternatives, negatives, BRs, flow coverage
3. Save report to `features/<feature-name>/review-spec.md`
4. Confirm: `✓ features/<feature-name>/review-spec.md`

## Output Requirements
- English only
- Follow sections 1–3 of `rules/reviewACBDD.md` output format exactly
- Flag missing AC candidates with source (which BR or flow step)
