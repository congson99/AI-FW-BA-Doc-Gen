# BA Documentation Generation Tool

AI-assisted framework for Business Analysts to generate a complete BA documentation set from a single input using Claude Code.

---

## Prerequisites

- [VS Code](https://code.visualstudio.com/) installed, or any other IDE that supports the Claude Code extension (e.g. JetBrains IDEs)

---

## Setup

### Step 1 — Extract the zip and open the project in VS Code

Extract the downloaded zip file, then open the folder in VS Code:

1. Open VS Code
2. Go to **File → Open Folder**
3. Select the extracted folder

### Step 2 — Install the Claude Code extension in VS Code

1. Go to **Extensions** (Ctrl+Shift+X / Cmd+Shift+X)
2. Search for **Claude Code**
3. Click **Install**

### Step 3 — Open the Claude Code panel

- Click the **Claude** icon in the VS Code sidebar, or
- Use the keyboard shortcut shown after the extension installs

### Step 4 — Fill in project config

`project/project_config.md` already comes with the repo (as a placeholder template). Open it and fill in the values for your project:

- **MCP Config** — add the MCP server URLs (e.g. Atlassian Confluence URL)
- **Context Sync** — map each Confluence page to a local file path
- **Task Environment** — set the default structure for feature env files

### Step 5 — Connect MCP servers

Run:

```
/connect-mcp
```

This connects Claude to the configured MCP servers (e.g. Atlassian) and guides you through the authentication flow.

### Step 6 — Sync project context (optional)

```
/sync
```

Fetches Confluence pages into local `project/` files so Claude has background knowledge before generating documents.

---

## Generating BA Documents

### Step 1 — Initialize the feature

```
/start <Feature Name>
```

Example:
```
/start Create User
```

Creates:
```
workspace/create-user/
  env_create_user.md    ← fill in Jira ticket and Confluence pages
  idea_create_user.md   ← fill in your feature description
```

---

### Step 2 — Fill in the environment and idea files

**`env_<slug>.md`** — replace Jira ticket and Confluence page placeholders:

```
# Environment

**Feature name:** Create User
**BA Task Jira ticket:** https://jira.example.com/browse/PROJ-123

**Context files:**
- project/context/project.md
- workspace/create-user/idea_create_user.md

**Confluence output pages:**
- BA Doc: https://confluence.example.com/pages/viewpage.action?pageId=67890
```

**`idea_<slug>.md`** — replace the placeholder with your feature description, requirements, constraints, or any notes:

```
# Feature Idea

Allow administrators to create a new user account with basic profile information.
The user will receive an email with login credentials upon successful creation.
```

> `/gen-brief` will stop if the placeholder in `idea_<slug>.md` has not been replaced.

---

### Step 3 — Generate the Brief

```
/gen-brief <Feature Name>
```

Generates:
```
workspace/create-user/
  brief_create_user.md   ← generated
```

---

### Step 4 — Generate Acceptance Criteria

Review and edit the brief if needed, then run:

```
/gen-ac <Feature Name>
```

Generates:
```
workspace/create-user/
  ac_create_user.md   ← generated
```

---

### Step 5 — Generate Business Rules

Review and edit the AC if needed, then run:

```
/gen-business-rule <Feature Name>
```

Generates:
```
workspace/create-user/
  business_rule_create_user.md   ← generated
```

---

### Step 6 — Generate Data Definition

Review and edit the Business Rules if needed, then run:

```
/gen-data-definition <Feature Name>
```

Generates:
```
workspace/create-user/
  data_definition_create_user.md   ← generated
```

---

### Step 7 — Generate Navigation

Review and edit the Data Definition if needed, then run:

```
/gen-navigation <Feature Name>
```

Generates:
```
workspace/create-user/
  navigation_create_user.md   ← generated
```

---

### Step 8 — Generate Flow

Review and edit the Navigation if needed, then run:

```
/gen-flow <Feature Name>
```

Generates:
```
workspace/create-user/
  flow_create_user.md   ← generated
```

---

### Step 9 — Generate UI Behavior

Review and edit the Flow if needed, then run:

```
/gen-ui-behavior <Feature Name>
```

Generates:
```
workspace/create-user/
  ui_behavior_create_user.md   ← generated
```

---

### Step 10 — Generate Messages

Review and edit the UI Behavior if needed, then run:

```
/gen-messages <Feature Name>
```

Generates:
```
workspace/create-user/
  messages_create_user.md   ← generated
```

---

### Step 11 — Package into BA Doc

```
/package <Feature Name>
```

Packages all artifacts into a single document:
```
workspace/create-user/
  ba_doc_create_user.md   ← generated
```

---

### Step 12 — Publish and close

```
/publish <Feature Name>
```

Publishes the BA Doc to Confluence, updates the Jira ticket status, and optionally clears the local feature folder.

---

## Available Commands

| Command | Purpose | Requires |
|---|---|---|
| `/check <Feature Name>` | Show doc status and suggest next step | — |
| `/clear-feature` | Delete all feature folders | — |
| `/clear-feature <Feature Name>` | Delete a specific feature folder | — |
| `/connect-mcp` | Connect to MCP servers listed in project_config.md | `project/project_config.md` filled |
| `/gen-ac <Feature Name>` | Generate Acceptance Criteria | `env_<slug>.md`, `brief_<slug>.md` |
| `/gen-brief <Feature Name>` | Generate Brief from idea file | `env_<slug>.md`, `idea_<slug>.md` filled |
| `/gen-business-rule <Feature Name>` | Generate Business Rules | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md` |
| `/gen-data-definition <Feature Name>` | Generate Data Definition | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md` |
| `/gen-flow <Feature Name>` | Generate Flow | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md`, `data_definition_<slug>.md`, `navigation_<slug>.md` |
| `/gen-messages <Feature Name>` | Generate Messages | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md` |
| `/gen-navigation <Feature Name>` | Generate Navigation | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md`, `data_definition_<slug>.md` |
| `/gen-ui-behavior <Feature Name>` | Generate UI Behavior | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md` |
| `/package <Feature Name>` | Package all artifacts into a single BA Doc | `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md`, `data_definition_<slug>.md`, `navigation_<slug>.md`, `flow_<slug>.md`, `ui_behavior_<slug>.md`, `messages_<slug>.md` |
| `/publish <Feature Name>` | Publish BA Doc to Confluence and update Jira status | `ba_doc_<slug>.md`, env filled |
| `/start <Feature Name>` | Initialize feature folder, env file, and idea file | — |
| `/sync` | Fetch Confluence pages into local project files | `project/project_config.md` filled |

---

## Folder Structure

```
AI-FW-Doc-Generation/
├── CLAUDE.md                              ← project instructions for Claude
├── .claude/
│   └── commands/
│       ├── sync.md                        ← /sync command definition
│       ├── start.md                       ← /start command definition
│       ├── check.md                       ← /check command definition
│       ├── gen-brief.md                   ← /gen-brief command definition
│       ├── gen-ac.md                      ← /gen-ac command definition
│       ├── gen-business-rule.md           ← /gen-business-rule command definition
│       ├── gen-data-definition.md         ← /gen-data-definition command definition
│       ├── gen-navigation.md              ← /gen-navigation command definition
│       ├── gen-flow.md                    ← /gen-flow command definition
│       ├── gen-ui-behavior.md             ← /gen-ui-behavior command definition
│       ├── gen-messages.md                ← /gen-messages command definition
│       ├── package.md                  ← /gen-ba-doc command definition
│       ├── publish.md                     ← /publish command definition
│       └── clear-feature.md              ← /clear-feature command definition
├── framework/
│   ├── rules/
│   │   ├── rule_brief.md                  ← writing rules for Brief
│   │   ├── rule_ac.md                     ← writing rules for Acceptance Criteria
│   │   ├── rule_business_rule.md          ← writing rules for Business Rules
│   │   ├── rule_data_definition.md        ← writing rules for Data Definition
│   │   ├── rule_navigation.md             ← writing rules for Navigation
│   │   ├── rule_flow.md                   ← writing rules for Flow
│   │   ├── rule_ui_behavior.md            ← writing rules for UI Behavior
│   │   └── rule_messages.md               ← writing rules for Messages
│   └── styles/
│       ├── style_general.md               ← general writing style (all docs)
│       ├── style_brief.md                 ← format rules for Brief
│       ├── style_ac.md                    ← format rules for Acceptance Criteria
│       ├── style_business_rule.md         ← format rules for Business Rules
│       ├── style_data_definition.md       ← format rules for Data Definition
│       ├── style_navigation.md            ← format rules for Navigation
│       ├── style_flow.md                  ← format rules for Flow
│       ├── style_ui_behavior.md           ← format rules for UI Behavior
│       └── style_messages.md              ← format rules for Messages
├── project/                               ← project-level context
│   ├── project_config.md                     ← project config (tracked — committed as a placeholder template; fill in locally per project)
│   ├── context/                           ← domain overview, modules, user stories (not committed)
│   └── reference/                         ← spec sheets, Confluence exports (not committed)
│       ├── business-rules/
│       │   ├── principles/                ← general principles (applied when generating rules)
│       │   └── shared-references/         ← shared rule groups (appended as reference lines in output)
│       ├── navigation/                    ← navigation patterns (used by /gen-navigation)
│       ├── ui-behavior/
│       │   ├── principles/                ← general UI principles (applied when generating entries)
│       │   └── shared-references/         ← shared UI groups (appended as reference lines in output)
│       └── messages/                      ← message templates and wording conventions (used by /gen-messages)
└── workspace/                             ← per-feature working area (not committed)
    └── <feature-name>/
        ├── env_<slug>.md                  ← created by /start
        ├── idea_<slug>.md                 ← created by /start (fill in before /gen-brief)
        ├── brief_<slug>.md                ← created by /gen-brief
        ├── ac_<slug>.md                   ← created by /gen-ac
        ├── business_rule_<slug>.md        ← created by /gen-business-rule
        ├── data_definition_<slug>.md      ← created by /gen-data-definition
        ├── navigation_<slug>.md           ← created by /gen-navigation
        ├── flow_<slug>.md                 ← created by /gen-flow
        ├── ui_behavior_<slug>.md          ← created by /gen-ui-behavior
        ├── messages_<slug>.md             ← created by /gen-messages
        └── ba_doc_<slug>.md               ← created by /gen-ba-doc
```
