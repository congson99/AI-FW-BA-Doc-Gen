# BA Documentation Generation Framework

AI-assisted framework for Business Analysts to generate a complete documentation set from a single input.

## Generate Commands

| Command | Purpose | Requires |
|---|---|---|
| `/generate-spec <feature\|JIRA>` | Generate Spec (sections 1–6) | `input.md` |
| `/generate-flow <feature>` | Generate Flow + States | `spec.md` |
| `/generate-scenarios <feature>` | Generate BDD Scenarios | `spec.md` + `flow.md` |
| `/generate-tc <feature>` | Generate Test Cases | `spec.md` + `scenarios.md` |
| `/generate-docs <feature>` | Merge ba-doc + qa-doc | 4 ai-docs |
| `/generate <feature\|JIRA>` | Fast-forward all 5 steps | `input.md` |
| `/generate-next <feature>` | Generate next missing artifact | — |

## Review Commands

| Command | Purpose | Requires |
|---|---|---|
| `/review-spec <feature>` | Review AC quality + completeness | `spec.md` |
| `/review-flow <feature>` | Review Flow + States completeness | `spec.md` + `flow.md` |
| `/review-scenarios <feature>` | Review BDD quality + AC coverage | `spec.md` + `scenarios.md` |
| `/review-tc <feature>` | Review TC coverage + quality | `spec.md` + `scenarios.md` + `tc.md` |
| `/review-docs <feature>` | Review ba-doc + qa-doc consistency | all 6 files |
| `/review <feature>` | Fast-forward all 5 reviews | all 6 files |

## Other Commands

| Command | Purpose |
|---|---|
| `/archive <feature>` | Move completed feature to `archive/` |

## Output Structure

```
features/<feature-name>/
  input.md
  ai-docs/
    spec.md             ← /generate-spec
    flow.md             ← /generate-flow
    scenarios.md        ← /generate-scenarios
    tc.md               ← /generate-tc
  ba-doc.md             ← /generate-docs (spec + flow)
  qa-doc.md             ← /generate-docs (scenarios + tc)
  review-spec.md        ← /review-spec
  review-flow.md        ← /review-flow
  review-scenarios.md   ← /review-scenarios
  review-tc.md          ← /review-tc
  review-docs.md        ← /review-docs
```

## Rules
- `rules/rule_spec.md` / `rules/rule_flow.md` / `rules/rule_scenarios.md` / `rules/rule_tc.md` / `rules/reviewACBDD.md`

## Templates (reference: Create Product Category)
- `templates/spec.md` / `templates/flow.md` / `templates/scenarios.md` / `templates/tc.md`
