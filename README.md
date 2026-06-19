# BA Documentation Generation Framework

AI-assisted framework for Business Analysts to generate a complete BA documentation set from a single input using Claude Code.

---

## Prerequisites

- [VS Code](https://code.visualstudio.com/) installed
- Git installed

---

## Setup

### Step 1 — Clone the repository

```bash
git clone https://github.com/congson99/AI-FW-Doc-Generation.git
cd AI-FW-Doc-Generation
```

### Step 2 — Install the Claude Code extension in VS Code

1. Open VS Code
2. Go to **Extensions** (Ctrl+Shift+X / Cmd+Shift+X)
3. Search for **Claude Code**
4. Click **Install**

> Alternatively, install the [Claude Code CLI](https://claude.ai/code) and run `claude` in the terminal from the project root.

### Step 3 — Open the project in VS Code

```bash
code .
```

### Step 4 — Open the Claude Code panel

- Click the **Claude** icon in the VS Code sidebar, or
- Use the keyboard shortcut shown after the extension installs

### Step 5 — Connect Atlassian via MCP (one-time setup)

Paste any Jira ticket URL or Confluence page URL into the Claude Code chat — Claude will automatically prompt you to connect Atlassian and guide you through the authentication flow.

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
workspace/features/cancel-purchase-request/
  env_cancel_purchase_request.md    ← fill in Jira tickets and Confluence pages
  idea_cancel_purchase_request.md   ← fill in your feature description
```

---

### Step 2 — Fill in the environment and idea files

**`env_<slug>.md`** — replace Jira ticket and Confluence page placeholders:

```
# Environment

**Feature name:** Cancel Purchase Request
**US Jira ticket:** https://jira.example.com/browse/INV-123
**BA Task Jira ticket:** https://jira.example.com/browse/INV-456

**Context files:**
- workspace/context/project.md
- workspace/features/cancel-purchase-request/idea_cancel_purchase_request.md

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
workspace/features/cancel-purchase-request/
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
workspace/features/cancel-purchase-request/
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
workspace/features/cancel-purchase-request/
  business_rule_cancel_purchase_request.md   ← generated
```

---

### Step 6 — Package into BA Doc

```
/gen-ba-doc <Feature Name>
```

Packages Brief, AC, and Business Rules into a single document:
```
workspace/features/cancel-purchase-request/
  ba_doc_cancel_purchase_request.md   ← generated
```

---

### Step 7 — Publish and close

```
/done <Feature Name>
```

Publishes the BA Doc to Confluence, updates the Jira ticket status, and optionally clears the local feature folder.

---

## Available Commands

| Command | Purpose | Requires |
|---|---|---|
| `/start <Feature Name>` | Initialize feature folder, env file, and idea file | — |
| `/check <Feature Name>` | Show doc status and suggest next step | — |
| `/gen-brief <Feature Name>` | Generate Brief from idea file | `env_<slug>.md`, `idea_<slug>.md` filled |
| `/gen-ac <Feature Name>` | Generate Acceptance Criteria | `env_<slug>.md`, `brief_<slug>.md` |
| `/gen-business-rule <Feature Name>` | Generate Business Rules | `env_<slug>.md`, `brief_<slug>.md`, `ac_<slug>.md` |
| `/gen-ba-doc <Feature Name>` | Package Brief, AC, and Business Rules into a single BA Doc | `brief_<slug>.md`, `ac_<slug>.md`, `business_rule_<slug>.md` |
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
│       ├── start.md                       ← /start command definition
│       ├── check.md                       ← /check command definition
│       ├── gen-brief.md                   ← /gen-brief command definition
│       ├── gen-ac.md                      ← /gen-ac command definition
│       ├── gen-business-rule.md           ← /gen-business-rule command definition
│       ├── gen-ba-doc.md                  ← /gen-ba-doc command definition
│       ├── done.md                        ← /done command definition
│       └── clear-feature.md              ← /clear-feature command definition
├── framework/
│   ├── rules/
│   │   ├── rule_brief.md                  ← writing rules for Brief
│   │   ├── rule_ac.md                     ← writing rules for Acceptance Criteria
│   │   └── rule_business_rule.md          ← writing rules for Business Rules
│   └── styles/
│       ├── style_general.md               ← general writing style (all docs)
│       ├── style_brief.md                 ← format rules for Brief
│       ├── style_ac.md                    ← format rules for Acceptance Criteria
│       └── style_business_rule.md         ← format rules for Business Rules
└── workspace/                             ← your working area (not committed to git)
    ├── context/
    │   └── project.md                     ← project background context
    └── features/
        └── <feature-name>/
            ├── env_<slug>.md              ← created by /start
            ├── idea_<slug>.md             ← created by /start (fill in before /gen-brief)
            ├── brief_<slug>.md            ← created by /gen-brief
            ├── ac_<slug>.md               ← created by /gen-ac
            ├── business_rule_<slug>.md    ← created by /gen-business-rule
            └── ba_doc_<slug>.md           ← created by /gen-ba-doc
```

> `workspace/` is in `.gitignore` — your documents stay local and are never pushed to the repository.
