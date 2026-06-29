# BA Documentation Generation Framework

AI-assisted framework for Business Analysts to generate a complete BA documentation set from a single input.

## Framework Behavior

At the start of every task, read `framework/framework_config.md` and apply the following rules based on its settings:

**`edit_framework = no` (default)**
- Only perform document generation tasks.
- Do not modify, suggest changes to, or ask questions about any framework files: `.claude/`, `framework/`, or `CLAUDE.md`.
- When asked to edit framework files, respond only with: "You do not have permission to edit framework files." Do not explain how to enable editing.

**`edit_framework = yes`**
- Framework files may be modified or improved as needed.
- Suggestions and questions about framework design are allowed.

## Commands

| Command | Purpose | Requires |
|---|---|---|
| `/init` | Initialize project/ and workspace/ folder structure | — |
| `/connect-mcp` | Connect to MCP servers listed in project_config.md | `project/project_config.md` filled |
| `/sync` | Fetch Confluence pages into local project files | `project/project_config.md` filled |
| `/start <Feature Name>` | Init feature folder + env + idea file | — |
| `/check <Feature Name>` | Show doc status + suggest next step | — |
| `/gen-brief <Feature Name>` | Generate Brief from idea file | `env_<slug>.md`, `idea_<slug>.md` filled |
| `/gen-ac <Feature Name>` | Generate Acceptance Criteria | `env_<slug>.md`, `brief_<slug>.md` |
| `/gen-business-rule <Feature Name>` | Generate Business Rules | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md` |
| `/gen-data-definition <Feature Name>` | Generate Data Definition | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md` |
| `/gen-navigation <Feature Name>` | Generate Navigation | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md`, `data_definition_<slug>.md` |
| `/gen-flow <Feature Name>` | Generate Flow | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md`, `data_definition_<slug>.md`, `navigation_<slug>.md` |
| `/gen-ui-behavior <Feature Name>` | Generate UI Behavior | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md`, `data_definition_<slug>.md`, `navigation_<slug>.md`, `flow_<slug>.md` |
| `/gen-messages <Feature Name>` | Generate Messages | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md` |
| `/package <Feature Name>` | Package all artifacts into a single BA Doc | `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md`, `data_definition_<slug>.md`, `navigation_<slug>.md`, `flow_<slug>.md`, `ui_behavior_<slug>.md`, `messages_<slug>.md` |
| `/publish <Feature Name>` | Publish BA Doc to Confluence and update Jira status | `ba_doc_<slug>.md`, env filled |

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
  gen-flow.md
CLAUDE.md                       ← project instructions (Claude Code requirement)

framework/                      ← reusable rules and styles, domain-agnostic
  rules/
    rule_brief.md
    rule_ac.md
    rule_business_rule.md
    rule_data_definition.md
    rule_navigation.md
    rule_flow.md
    rule_ui_behavior.md
    rule_messages.md
  styles/
    style_general.md            ← general writing style (all docs)
    style_brief.md              ← style specific to Brief
    style_ac.md                 ← style specific to AC
    style_business_rule.md      ← style specific to Business Rules
    style_data_definition.md    ← style specific to Data Definition
    style_navigation.md         ← style specific to Navigation
    style_flow.md               ← style specific to Flow
    style_ui_behavior.md        ← style specific to UI Behavior
    style_messages.md           ← style specific to Messages

project/                        ← project-level context (not committed)
  project_config.md                ← project config (MCP, sync, env template, automation)
  context/                      ← domain overview, module map, user stories
  reference/                    ← spec sheets, Confluence exports, detailed docs
    business-rules/
      principles/               ← general principles (applied when generating rules)
      shared-references/        ← shared rule groups (appended as reference lines in output)
    navigation/                 ← navigation patterns (read by /gen-navigation)
    ui-behavior/
      principles/               ← general UI principles (applied when generating entries)
      shared-references/        ← shared UI groups (appended as reference lines in output)
    messages/                   ← message templates and wording conventions (read by /gen-messages)

workspace/                      ← per-feature working area (not committed)
  <feature-name>/
    env_<slug>.md               ← /start (init)
    idea_<slug>.md              ← /start (fill before /gen-brief)
    brief_<slug>.md             ← /gen-brief
    ac_<slug>.md                ← /gen-ac
    business_rule_<slug>.md     ← /gen-business-rule
    data_definition_<slug>.md   ← /gen-data-definition
    navigation_<slug>.md        ← /gen-navigation
    flow_<slug>.md              ← /gen-flow
    ui_behavior_<slug>.md       ← /gen-ui-behavior
    messages_<slug>.md          ← /gen-messages
    ba_doc_<slug>.md            ← /gen-ba-doc
```

> slug = kebab-case folder name with `-` replaced by `_` (e.g. `cancel-pr` → `cancel_pr`)
