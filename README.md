# BA Documentation Generation Framework

AI-assisted framework for Business Analysts to generate a complete BA documentation set from a single input using Claude Code.

---

## Prerequisites

- [VS Code](https://code.visualstudio.com/) installed

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

### Step 4 — Connect Atlassian via MCP (one-time setup)

Paste any Jira ticket URL or Confluence page URL into the Claude Code chat — Claude will automatically prompt you to connect Atlassian and guide you through the authentication flow.

### Step 5 — Initialize the workspace

```
/init
```

Creates the `project/` and `workspace/` folders if they don't already exist, and generates `project/sync_config.md`.

### Step 6 — Configure project context (optional)

Open `project/sync_config.md` and fill in the Confluence URLs for your project pages, then run:

```
/sync
```

This fetches the Confluence content into the local `project/context/` and `project/reference/` files, giving Claude background knowledge about your domain before generating any documents. After syncing, `/sync` will also detect any local files that are no longer mapped in `sync_config.md` and ask if you want to delete them.

---

## Generating BA Documents

### Step 1 — Initialize the feature

```
/start <Feature Name>
```

**Example:**
```
/start Cancel Purchase Request
```

Creates:
```
workspace/cancel-purchase-request/
  env_cancel_purchase_request.md    ← fill in Jira ticket and Confluence pages
  idea_cancel_purchase_request.md   ← fill in your feature description
```

---

### Step 2 — Fill in the environment and idea files

**`env_<slug>.md`** — replace Jira ticket and Confluence page placeholders:

```
# Environment

**Feature name:** Cancel Purchase Request
**BA Task Jira ticket:** https://jira.example.com/browse/INV-456

**Context files:**
- project/context/project.md
- workspace/cancel-purchase-request/idea_cancel_purchase_request.md

**Confluence output pages:**
- BA Doc: https://confluence.example.com/pages/viewpage.action?pageId=67890
```

**`idea_<slug>.md`** — replace the placeholder with your feature description, requirements, constraints, or any notes:

```
# Feature Idea

Allow users to cancel a Purchase Request that is in Draft or Pending Approval status.
Once cancelled, the PR cannot be edited or resubmitted.
```

> `/gen-brief` will stop if the placeholder in `idea_<slug>.md` has not been replaced.

---

### Step 3 — Generate the Brief

```
/gen-brief <Feature Name>
```

Generates:
```
workspace/cancel-purchase-request/
  brief_cancel_purchase_request.md   ← generated
```

---

### Step 4 — Generate Acceptance Criteria

Review and edit the brief if needed, then run:

```
/gen-ac <Feature Name>
```

Generates:
```
workspace/cancel-purchase-request/
  ac_cancel_purchase_request.md   ← generated
```

---

### Step 5 — Generate Business Rules

Review and edit the AC if needed, then run:

```
/gen-business-rule <Feature Name>
```

Generates:
```
workspace/cancel-purchase-request/
  business_rule_cancel_purchase_request.md   ← generated
```

---

### Step 6 — Generate Data Definition

Review and edit the Business Rules if needed, then run:

```
/gen-data-definition <Feature Name>
```

Generates:
```
workspace/cancel-purchase-request/
  data_definition_cancel_purchase_request.md   ← generated
```

---

### Step 7 — Generate Navigation

Review and edit the Data Definition if needed, then run:

```
/gen-navigation <Feature Name>
```

Generates:
```
workspace/cancel-purchase-request/
  navigation_cancel_purchase_request.md   ← generated
```

---

### Step 8 — Generate Flow

Review and edit the Navigation if needed, then run:

```
/gen-flow <Feature Name>
```

Generates:
```
workspace/cancel-purchase-request/
  flow_cancel_purchase_request.md   ← generated
```

---

### Step 9 — Package into BA Doc

```
/gen-ba-doc <Feature Name>
```

Packages Brief, AC, Business Rules, Data Definition, and Navigation into a single document:
```
workspace/cancel-purchase-request/
  ba_doc_cancel_purchase_request.md   ← generated
```

---

### Step 10 — Publish and close

```
/done <Feature Name>
```

Publishes the BA Doc to Confluence, updates the Jira ticket status, and optionally clears the local feature folder.

---

## Available Commands

| Command | Purpose | Requires |
|---|---|---|
| `/init` | Initialize project/ and workspace/ folder structure | — |
| `/sync` | Fetch Confluence pages into local project files | `project/sync_config.md` filled |
| `/start <Feature Name>` | Initialize feature folder, env file, and idea file | — |
| `/check <Feature Name>` | Show doc status and suggest next step | — |
| `/gen-brief <Feature Name>` | Generate Brief from idea file | `env_<slug>.md`, `idea_<slug>.md` filled |
| `/gen-ac <Feature Name>` | Generate Acceptance Criteria | `env_<slug>.md`, `brief_<slug>.md` |
| `/gen-business-rule <Feature Name>` | Generate Business Rules | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md` |
| `/gen-data-definition <Feature Name>` | Generate Data Definition | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md` |
| `/gen-navigation <Feature Name>` | Generate Navigation | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md` |
| `/gen-flow <Feature Name>` | Generate Flow | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md` |
| `/gen-ba-doc <Feature Name>` | Package all artifacts into a single BA Doc | `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md`, `data_definition_<slug>.md`, `navigation_<slug>.md`, `flow_<slug>.md` |
| `/done <Feature Name>` | Publish BA Doc to Confluence and update Jira status | `ba_doc_<slug>.md`, env filled |
| `/clear-feature <Feature Name>` | Delete a specific feature folder | — |
| `/clear-feature` | Delete all feature folders | — |

---

## Folder Structure

```
AI-FW-Doc-Generation/
├── CLAUDE.md                              ← project instructions for Claude
├── .claude/
│   └── commands/
│       ├── init.md                        ← /init command definition
│       ├── sync.md                        ← /sync command definition
│       ├── start.md                       ← /start command definition
│       ├── check.md                       ← /check command definition
│       ├── gen-brief.md                   ← /gen-brief command definition
│       ├── gen-ac.md                      ← /gen-ac command definition
│       ├── gen-business-rule.md           ← /gen-business-rule command definition
│       ├── gen-data-definition.md         ← /gen-data-definition command definition
│       ├── gen-navigation.md              ← /gen-navigation command definition
│       ├── gen-flow.md                    ← /gen-flow command definition
│       ├── gen-ba-doc.md                  ← /gen-ba-doc command definition
│       ├── done.md                        ← /done command definition
│       └── clear-feature.md              ← /clear-feature command definition
├── framework/
│   ├── rules/
│   │   ├── rule_brief.md                  ← writing rules for Brief
│   │   ├── rule_ac.md                     ← writing rules for Acceptance Criteria
│   │   ├── rule_business_rule.md          ← writing rules for Business Rules
│   │   ├── rule_data_definition.md        ← writing rules for Data Definition
│   │   ├── rule_navigation.md             ← writing rules for Navigation
│   │   └── rule_flow.md                   ← writing rules for Flow
│   └── styles/
│       ├── style_general.md               ← general writing style (all docs)
│       ├── style_brief.md                 ← format rules for Brief
│       ├── style_ac.md                    ← format rules for Acceptance Criteria
│       ├── style_business_rule.md         ← format rules for Business Rules
│       ├── style_data_definition.md       ← format rules for Data Definition
│       ├── style_navigation.md            ← format rules for Navigation
│       └── style_flow.md                  ← format rules for Flow
├── project/                               ← project-level context (not committed)
│   ├── sync_config.md                     ← Confluence URL → local file mapping
│   ├── context/                           ← domain overview, modules, user stories
│   └── reference/                         ← spec sheets, Confluence exports
│       ├── business-rules/                ← general business rules (used by /gen-business-rule)
│       ├── ui-rules/                      ← UI/UX rules and standards
│       ├── navigation/                    ← navigation patterns (used by /gen-navigation)
│       └── messages/                      ← system message definitions
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
        └── ba_doc_<slug>.md               ← created by /gen-ba-doc
```
