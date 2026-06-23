# BA Documentation Generation Framework

AI-assisted framework for Business Analysts to generate a complete BA documentation set from a single input.

## Framework Behavior

At the start of every task, read `framework/framework_config.md` and apply the following rules based on its settings:

**`edit_framework = no` (default)**
- Only perform document generation tasks.
- Do not modify, suggest changes to, or ask questions about any framework files: `.claude/commands/`, `framework/rules/`, `framework/styles/`, `framework/framework_config.md`, or `CLAUDE.md`.
- When asked to edit framework files, respond only with: "You do not have permission to edit framework files." Do not explain how to enable editing.

**`edit_framework = yes`**
- Framework files may be modified or improved as needed.
- Suggestions and questions about framework design are allowed.

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
| `/gen-data-definition <Feature Name>` | Generate Data Definition | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md` |
| `/gen-navigation <Feature Name>` | Generate Navigation | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md` |
| `/gen-ba-doc <Feature Name>` | Package all artifacts into a single BA Doc | `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md`, `data_definition_<slug>.md`, `navigation_<slug>.md` |
| `/done <Feature Name>` | Publish BA Doc to Confluence and update Jira status | `ba_doc_<slug>.md`, env filled |

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
  gen-data-definition.md
  gen-navigation.md
CLAUDE.md                       ← project instructions (Claude Code requirement)

framework/                      ← reusable rules and styles, domain-agnostic
  rules/
    rule_brief.md
    rule_ac.md
    rule_business_rule.md
    rule_data_definition.md
    rule_navigation.md
  styles/
    style_general.md            ← general writing style (all docs)
    style_brief.md              ← style specific to Brief
    style_ac.md                 ← style specific to AC
    style_business_rule.md      ← style specific to Business Rules
    style_data_definition.md    ← style specific to Data Definition
    style_navigation.md         ← style specific to Navigation

project/                        ← project-level context (not committed)
  sync_config.md                ← Confluence URL → local file mapping for /sync
  context/                      ← domain overview, module map, user stories
  reference/                    ← spec sheets, Confluence exports, detailed docs
    business-rules/             ← general business rules (read by /gen-business-rule)
    ui-rules/                   ← UI/UX rules and standards
    navigation/                 ← navigation patterns (read by /gen-navigation)
    messages/                   ← system message definitions

workspace/                      ← per-feature working area (not committed)
  <feature-name>/
    env_<slug>.md               ← /start (init)
    idea_<slug>.md              ← /start (fill before /gen-brief)
    brief_<slug>.md             ← /gen-brief
    ac_<slug>.md                ← /gen-ac
    business_rule_<slug>.md     ← /gen-business-rule
    data_definition_<slug>.md   ← /gen-data-definition
    navigation_<slug>.md        ← /gen-navigation
    ba_doc_<slug>.md            ← /gen-ba-doc
```

> slug = kebab-case folder name with `-` replaced by `_` (e.g. `cancel-pr` → `cancel_pr`)
