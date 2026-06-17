# BA Documentation Generation Framework

AI-assisted framework for Business Analysts to generate a complete BA documentation set from a single input.

## Commands

| Command | Purpose | Requires |
|---|---|---|
| `/gen-ba <Feature Name>` | Init folder + env file | — |
| `/gen-brief <Feature Name>` | Generate Brief from chat input | `env_<slug>.md` |

## Structure

```
.claude/commands/           ← slash commands (Claude Code requirement)
  gen-ba.md
  gen-brief.md
CLAUDE.md                   ← project instructions (Claude Code requirement)

framework/                  ← reusable, không phụ thuộc domain
  rules/
    rule_brief.md

workspace/                  ← context nghiệp vụ + doc được gen
  context/
    project.md
  features/
    <feature-name>/
      env_<slug>.md         ← /gen-ba (init)
      brief_<slug>.md       ← /gen-brief (from chat)
```

> slug = folder name với `-` thay bằng `_` (e.g. `cancel-pr` → `cancel_pr`)
