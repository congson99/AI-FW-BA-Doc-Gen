---
name: "Generate Next Artifact"
description: "Generate the next missing artifact for a feature. Usage: /generate-next <feature-name>"
---

You are a Senior Business Analyst and QA Engineer generating the next missing document for a feature.

## Identify Feature

`$ARGUMENTS` is the feature name (kebab-case). If not provided, list all features in `features/` that are partially complete and ask which one to continue.

## Detect Next Missing Artifact

Check `features/<feature-name>/ai-docs/` in order:

| Priority | File | Requires |
|---|---|---|
| 1 | `spec.md` | `input.md` |
| 2 | `flow.md` | `spec.md` |
| 3 | `scenarios.md` | `spec.md` + `flow.md` |
| 4 | `tc.md` | `spec.md` + `scenarios.md` |
| 5 | `ba-doc.md` | `spec.md` + `flow.md` |
| 6 | `qa-doc.md` | `scenarios.md` + `tc.md` |

Generate the first item in the list that is missing but whose dependencies exist.

If all 6 artifacts exist → report: "All artifacts complete. Run /archive <feature-name> to archive."

## Generation Rules

Follow the same rules as `/generate` for whichever step is next:
- Spec → `rules/rule_spec.md` + `templates/spec.md`
- Flow → `rules/rule_flow.md` + `templates/flow.md`
- Scenarios → `rules/rule_scenarios.md` + `templates/scenarios.md`
- TC → `rules/rule_tc.md` + `templates/tc.md`
- BA Doc → merge spec + flow
- QA Doc → merge scenarios + tc

## Output Requirements

- Always generate output in English
- Confirm which artifact was generated and its path
