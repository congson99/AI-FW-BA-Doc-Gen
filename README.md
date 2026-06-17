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

### Step 6 — Fill in project context (one-time setup)

Open `workspace/context/project.md` (already in the repo) and fill in your project background. Claude reads all files in this folder automatically when generating documents.

---

## Generating BA Documents

### Step 1 — Initialize the feature environment

In the Claude Code chat, run:

```
/gen-ba <Feature Name>
```

**Example:**
```
/gen-ba Cancel Purchase Request
```

This creates:
```
workspace/features/cancel-purchase-request/
  env_cancel_purchase_request.md   ← fill this in before the next step
```

---

### Step 2 — Fill in the environment file

Open the generated `env_<slug>.md` and replace all placeholders:

```
# Environment

**Feature name:** Cancel Purchase Request
**US Jira ticket:** https://jira.example.com/browse/INV-123
**BA Task Jira ticket:** https://jira.example.com/browse/INV-456

**Context files:**
- workspace/context/project.md

**Confluence output pages:**
- BA Doc: https://confluence.example.com/pages/viewpage.action?pageId=67890
```

> **Context files** — files in `workspace/context/` are auto-populated by `/gen-ba`. You can also add Confluence URLs or other local `.md` files here.

---

### Step 3 — Generate the Brief

Describe the feature in the Claude Code chat, then run:

```
/gen-brief <Feature Name>
```

**Example:**

First, describe the feature in chat:
> "Allow users to cancel a Purchase Request that is in Draft or Pending Approval status. Once cancelled, the PR cannot be edited or resubmitted."

Then run:
```
/gen-brief Cancel Purchase Request
```

Claude reads the env file and context files, then generates:
```
workspace/features/cancel-purchase-request/
  brief_cancel_purchase_request.md   ← generated
```

---

## Available Commands

| Command | Purpose | Requires |
|---|---|---|
| `/gen-ba <Feature Name>` | Initialize feature folder and env file | — |
| `/gen-brief <Feature Name>` | Generate Brief from chat description | `env_<slug>.md` filled in |
| `/clear-feature <Feature Name>` | Delete a specific feature folder | — |
| `/clear-feature` | Delete all feature folders | — |

---

## Folder Structure

```
AI-FW-Doc-Generation/
├── CLAUDE.md                          ← project instructions for Claude
├── .claude/
│   └── commands/
│       ├── gen-ba.md                  ← /gen-ba command definition
│       ├── gen-brief.md               ← /gen-brief command definition
│       └── clear-feature.md           ← /clear-feature command definition
├── framework/
│   └── rules/
│       └── rule_brief.md              ← writing rules for Brief
└── workspace/                         ← your working area (not committed to git)
    ├── context/
    │   └── project.md                 ← project background context
    └── features/
        └── <feature-name>/
            ├── env_<slug>.md          ← created by /gen-ba
            └── brief_<slug>.md        ← created by /gen-brief
```

> `workspace/` is in `.gitignore` — your documents stay local and are never pushed to the repository.
