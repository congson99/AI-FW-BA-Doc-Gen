# BA Documentation Generation Framework

AI-assisted framework for Business Analysts to generate a complete BA documentation set from a single input.

## Commands

| Command | Purpose | Requires |
|---|---|---|
| `/init` | Initialize project/ and workspace/ folder structure | — |
| `/sync` | Fetch Confluence pages into local project files | `project/sync_config.md` filled |
| `/start <Feature Name>` | Init feature folder + env + idea file | — |
| `/check <Feature Name>` | Show doc status + suggest next step | — |
| `/gen-brief <Feature Name>` | Generate Brief from idea file | `env_<slug>.md`, `idea_<slug>.md` filled |
| `/gen-ac <Feature Name>` | Generate Acceptance Criteria | `env_<slug>.md`, `brief_<slug>.md` |
| `/gen-business-rule <Feature Name>` | Generate Business Rules | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md` |

## Structure

```
.claude/commands/               ← slash commands (Claude Code requirement)
  init.md
  sync.md
  start.md
  check.md
  gen-brief.md
  gen-ac.md
  gen-business-rule.md
CLAUDE.md                       ← project instructions (Claude Code requirement)

framework/                      ← reusable rules and styles, domain-agnostic
  rules/
    rule_brief.md
    rule_ac.md
    rule_business_rule.md
  styles/
    style_general.md            ← general writing style (all docs)
    style_brief.md              ← style specific to Brief
    style_ac.md                 ← style specific to AC
    style_business_rule.md      ← style specific to Business Rules

project/                        ← project-level context (not committed)
  sync_config.md                ← Confluence URL → local file mapping for /sync
  context/                      ← domain overview, module map, user stories
  reference/                    ← spec sheets, Confluence exports, detailed docs

workspace/                      ← per-feature working area (not committed)
  <feature-name>/
    env_<slug>.md               ← /start (init)
    idea_<slug>.md              ← /start (fill before /gen-brief)
    brief_<slug>.md             ← /gen-brief
    ac_<slug>.md                ← /gen-ac
    business_rule_<slug>.md     ← /gen-business-rule
    ba_doc_<slug>.md            ← /gen-ba-doc
```

> slug = kebab-case folder name with `-` replaced by `_` (e.g. `cancel-pr` → `cancel_pr`)
