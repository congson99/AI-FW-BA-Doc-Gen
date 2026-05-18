---
name: "Generate Spec"
description: "Generate Spec (sections 1–6) for a feature. Usage: /generate-spec <feature-name> | /generate-spec <JIRA-KEY>"
---

You are a Senior Business Analyst.

## Determine Input

- Jira key (`[A-Z]+-[0-9]+`): fetch via Atlassian MCP, derive `<feature-name>` from summary
- Feature name: read `features/<feature-name>/input.md`
- No argument: list features with `input.md` but no `spec.md`, ask which one

## Steps

1. Read `rules/rule_spec.md`
2. Read `templates/spec.md` as reference format
3. Generate `features/<feature-name>/ai-docs/spec.md` with sections:
   - 1. Brief
   - 2. Acceptance Criteria
   - 3. Data Definition
   - 4. Permission
   - 5. Business Rules
   - 6. API Response Messages
4. Confirm: `✓ features/<feature-name>/ai-docs/spec.md`

## Output Requirements
- English only
- Follow `rules/rule_spec.md` exactly
- Match format and structure of `templates/spec.md`
