---
name: "Generate All Artifacts (Fast Forward)"
description: "Run all generate commands in sequence. Usage: /generate <feature-name> | /generate <JIRA-KEY>"
---

Fast-forward: run all 5 generate steps in order for a feature.

## Determine Input

- Jira key (`[A-Z]+-[0-9]+`): fetch via Atlassian MCP, derive `<feature-name>` from ticket summary
- Feature name: read `features/<feature-name>/input.md`
- No argument: list features with `input.md` but incomplete artifacts, ask which one

## Pipeline — run in order

```
/generate-spec      → features/<feature-name>/ai-docs/spec.md
/generate-flow      → features/<feature-name>/ai-docs/flow.md
/generate-scenarios → features/<feature-name>/ai-docs/scenarios.md
/generate-tc        → features/<feature-name>/ai-docs/tc.md
/generate-docs      → features/<feature-name>/ba-doc.md + qa-doc.md
```

Follow each command's rules exactly (read the corresponding rule file and template).

## Completion Report

```
✓ features/<feature-name>/ai-docs/spec.md
✓ features/<feature-name>/ai-docs/flow.md
✓ features/<feature-name>/ai-docs/scenarios.md
✓ features/<feature-name>/ai-docs/tc.md
✓ features/<feature-name>/ba-doc.md
✓ features/<feature-name>/qa-doc.md

All 6 artifacts generated. Run /archive <feature-name> when ready.
```
