# BA Documentation Generation Framework

AI-assisted framework for Business Analysts to generate a complete BA documentation set from a single input.

## Commands

| Command | Purpose | Requires |
|---|---|---|
| `/start <Feature Name>` | Init folder + env + idea file | — |
| `/check <Feature Name>` | Show doc status + suggest next step | — |
| `/gen-brief <Feature Name>` | Generate Brief from idea file | `env_<slug>.md`, `idea_<slug>.md` filled |
| `/gen-ac <Feature Name>` | Generate Acceptance Criteria | `env_<slug>.md`, `brief_<slug>.md` |
| `/gen-business-rule <Feature Name>` | Generate Business Rules | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md` |

## Structure

```
.claude/commands/           ← slash commands (Claude Code requirement)
  start.md
  check.md
  gen-brief.md
  gen-ac.md
  gen-business-rule.md
CLAUDE.md                   ← project instructions (Claude Code requirement)

framework/                  ← reusable, không phụ thuộc domain
  rules/
    rule_brief.md
    rule_ac.md
    rule_business_rule.md
  styles/
    style_general.md        ← general writing style (all docs)
    style_brief.md          ← style specific to Brief
    style_ac.md             ← style specific to AC
    style_business_rule.md  ← style specific to Business Rules

workspace/                  ← context nghiệp vụ + doc được gen
  context/
    project.md
  features/
    <feature-name>/
      env_<slug>.md              ← /start (init)
      idea_<slug>.md             ← /start (fill before /gen-brief)
      brief_<slug>.md            ← /gen-brief
      ac_<slug>.md               ← /gen-ac
      business_rule_<slug>.md    ← /gen-business-rule
```

> slug = folder name với `-` thay bằng `_` (e.g. `cancel-pr` → `cancel_pr`)
