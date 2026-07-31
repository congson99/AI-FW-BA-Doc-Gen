# DC32 BA Documentation Claude Tool

v2.1

---

## Table of Contents

1. [Overview](#1-overview)
2. [Setup Environment](#2-setup-environment-one-time-only)
3. [Generating BA Documents](#3-generating-ba-documents)
4. [Configure the Project](#4-configure-the-project-for-ba-leader)
5. [Available Commands](#5-available-commands)
6. [Folder Structure](#6-folder-structure)

---

## 1. Overview

**DC32 BA Documentation Claude Tool** is an AI-assisted framework built on Claude Code that lets Business Analysts generate a complete BA documentation set — Brief, Acceptance Criteria, Business Rules, Data Definition, Navigation, Flow, UI Behavior, and Messages — from a single feature idea, then publish it straight to Confluence and update Jira.

**Where to start:**
- **Joining an existing, already-configured project to generate docs?** Do [2. Setup Environment](#2-setup-environment-one-time-only), then go straight to [3. Generating BA Documents](#3-generating-ba-documents).
- **Setting up a brand-new project for the first time?** That's the BA leader's job — after [2. Setup Environment](#2-setup-environment-one-time-only), do [4. Configure the Project](#4-configure-the-project-for-ba-leader) once, then push the result to the repo so the rest of the team can just clone and go.

---

## 2. Setup Environment (one-time only)

### Step 1 — Install VS Code

Install [VS Code](https://code.visualstudio.com/), or any other IDE that supports the Claude Code extension (e.g. JetBrains IDEs).

### Step 2 — Clone the project's branch and open it in VS Code

Clone the branch corresponding to your project, then open the folder in VS Code:

1. Clone the branch: `git clone -b <branch-name> <repository-url>`
2. Open VS Code
3. Go to **File → Open Folder**
4. Select the cloned folder

### Step 3 — Install the Claude Code extension in VS Code

1. Go to **Extensions** (Ctrl+Shift+X / Cmd+Shift+X)
2. Search for **Claude Code**
3. Click **Install**
4. Click the **Claude** icon in the VS Code sidebar (or use the keyboard shortcut shown after install) to open the panel

### Step 4 — Sync project data

```
/sync-project
```

Fetches the Confluence pages mapped in `project/project_config.md` into local `project/` files, so Claude has background knowledge before generating documents. If MCP servers (e.g. Atlassian) aren't connected yet, `/sync-project` connects them automatically first, then proceeds with the sync.

> Re-run `/sync-project` any time the source data changes (e.g. someone updates a mapped Confluence page) to pull the latest content locally.

---

## 3. Generating BA Documents

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
  input/
    env_create_user.md      ← fill in Jira ticket and Confluence pages
    context_create_user.md  ← list every relevant context file
```

---

### Step 2 — Fill in the environment and context files

**`env_<slug>.md`** — replace Jira ticket and Confluence page placeholders:

```
**Feature name:** Create User

**Document language:** English

**BA Task Jira ticket:** https://jira.example.com/browse/PROJ-123

**Confluence output pages:**
- BA Doc: https://confluence.example.com/pages/viewpage.action?pageId=67890
```

**`context_<slug>.md`** — list every project context/reference file relevant to this feature:

```
# Context Files

- project/context/project.md
```

`/investigate` reads all of them to build the Idea file.

---

### Step 3 — Generate the BA Doc

```
/gen-ba-doc <Feature Name>
```

Runs Idea → Brief → AC → Business Rules → Data Definition → Navigation → Flow → UI Behavior → Messages → Package back-to-back, without pausing for review between steps. It only pauses to ask if something genuinely needs clarifying along the way — including for the Idea file itself, if the Context files don't cover everything.

Generates:
```
workspace/create-user/
  input/
    idea_create_user.md
  docs/
    brief_create_user.md
    ac_create_user.md
    business_rule_create_user.md
    data_definition_create_user.md
    navigation_create_user.md
    flow_create_user.md
    ui_behavior_create_user.md
    messages_create_user.md
  ba_doc_create_user.md          ← final packaged document
```

> Prefer to review and edit each artifact before generating the next? Run the individual `/gen-*` commands one at a time instead — see [Generate Step-by-Step](#3-generate-step-by-step-alternative) below.

---

### Step 4 — Publish and close

```
/publish <Feature Name>
```

Publishes the BA Doc to Confluence, updates the Jira ticket status, and optionally clears the local feature folder.

---

## 4. Configure the Project (for BA leader)

This is for the BA leader to set up once when starting a new project. Configure it, then push `project/project_config.md` to the repo so the whole team can clone and reuse it — only needs to be done once per project.

```
/config-project
```

Run this to configure interactively — asks one question at a time and fills in `project/project_config.md` as you go. Skip anything you don't have yet; run it again any time to fill in the rest or change values.

Or follow the steps below to edit `project/project_config.md` directly:

### Step 1 — Set up MCP Config

Edit `## 1. MCP Config`. This tells Claude which external MCP servers to connect to — `/sync-project` and `/publish` both need this connection to fetch Confluence content and publish back to it. Add one line per server: `- Atlassian: <confluence-mcp-url>`.

### Step 2 — Set the document language

Edit `## 2. Language`. This controls what language the prose content of every generated document is written in — the Idea file and all 8 BA docs (Brief, AC, Business Rules, Data Definition, Navigation, Flow, UI Behavior, Messages). Useful if your team writes BA docs in Vietnamese, English, or another language. Section headings and fixed markers (`AC1`, `R1`, `[Start]`/`[End]`, etc.) always stay in English regardless, so cross-document structure stays consistent. Set `Document language` to what you want (e.g. English, Vietnamese).

### Step 3 — Map Context Sync pages

Edit `## 3. Context Sync`. This maps each Confluence page your project maintains to a local file path. `/sync-project` reads this mapping to know what to fetch, and each `/gen-*` command later reads the resulting local files as background reference for its category — e.g. Business Rules Principles guide how `/gen-business-rule` reasons about rules, Navigation patterns guide `/gen-navigation`, and so on. For each category you have Confluence pages for — Context, Business Rules (Principles / Shared References), UI Behavior (Principles / Shared References), Navigation, Messages — add one entry per page:
```
- <local-file-path>
  url: <confluence-page-url>
```
Leave categories you don't use empty — commands simply skip categories with nothing mapped.

### Step 4 — Fill in the Task Environment template

Edit `## 4. Task Environment`. This is the default template `/start` copies into every new feature's own `env_<slug>.md` and `context_<slug>.md` — what you set here becomes every feature's starting point, so BAs don't retype the same defaults each time:
- `context_<slug>.md template` — the context file(s) every new feature should start with by default (usually your project's domain overview — the same file mapped under Context Sync's "Context" category).
- `env_<slug>.md template` — the Confluence output page label(s) this project publishes BA Docs to (e.g. a single "BA Doc" page, or split into "BA Doc" / "Spec" / "Flow", matching how your team organizes Confluence). Leave `**BA Task Jira ticket:**` as its placeholder — that field is intentionally per-feature, filled in by `/start` each time, not set once here.

### Step 5 — Set Task Automation targets

Edit `## 5. Task Automation`. This defines what `/publish` automatically does at the end of every feature — what Jira status to move the ticket to, which Jira project it belongs to, and which Confluence parent page the finished BA Doc gets published under. Setting this once means every BA on the team gets the same consistent publish behavior without configuring it per feature.

### Step 6 — Sync project data

```
/sync-project
```

Fetches the Confluence pages mapped in Step 3 into local `project/` files, so Claude actually has the content on disk as background knowledge before generating documents — without this, the mappings above are just addresses with no content behind them yet. If MCP servers aren't connected yet, `/sync-project` connects them automatically first, then proceeds with the sync.

---

## 5. Available Commands

### BA Doc Gen Flow Commands

Used as part of the regular per-feature BA document generation flow described above.

| Command | Purpose |
|---|---|
| `/start <Feature Name>` | Initialize feature folder, env file, and context file |
| `/investigate <Feature Name>` | Generate the Idea file from project context, asking the user for anything missing |
| `/gen-brief <Feature Name>` | Generate Brief from the Idea file |
| `/gen-ac <Feature Name>` | Generate Acceptance Criteria |
| `/gen-business-rule <Feature Name>` | Generate Business Rules |
| `/gen-data-definition <Feature Name>` | Generate Data Definition |
| `/gen-navigation <Feature Name>` | Generate Navigation |
| `/gen-flow <Feature Name>` | Generate Flow |
| `/gen-ui-behavior <Feature Name>` | Generate UI Behavior |
| `/gen-messages <Feature Name>` | Generate Messages |
| `/gen-ba-doc <Feature Name>` | Run investigate through gen-messages and package back-to-back |
| `/package <Feature Name>` | Package all artifacts into a single BA Doc |
| `/publish <Feature Name>` | Publish BA Doc to Confluence and update Jira status |

### Other Commands

Used independently, as needed — project configuration and maintenance, not part of the BA doc gen flow.

| Command | Purpose |
|---|---|
| `/check <Feature Name>` | Show doc status and suggest next step |
| `/clear-project` | Delete synced context/reference files, reset project_config.md to its blank template, and clear workspace/ |
| `/clear-workspace` | Delete all feature folders in workspace/ |
| `/config-project` | Interactively fill in project_config.md via Q&A instead of manual editing |
| `/connect-mcp` | Connect to MCP servers listed in project_config.md |
| `/sync-project` | Fetch Confluence pages into local project files |

---

## 6. Folder Structure

```
AI-FW-Doc-Generation/
├── CLAUDE.md                              ← project instructions for Claude
├── .claude/
│   └── commands/                          ← slash command definitions (see Available Commands)
├── framework/                             ← reusable rules and styles, domain-agnostic
│   ├── rules/                             ← writing/content rules, one file per doc type
│   ├── styles/                            ← format rules, one file per doc type + style_general.md
│   └── templates/                         ← project_config_blank.md (blank project_config.md template)
├── project/                               ← project-level context
│   ├── project_config.md                  ← project config (tracked — committed as a placeholder template; fill in locally per project)
│   ├── context/                           ← domain overview, modules, user stories (not committed)
│   └── reference/                         ← spec sheets, Confluence exports (not committed)
│       ├── business-rules/                ← principles + shared references for Business Rules
│       ├── navigation/                    ← shared navigation patterns
│       ├── ui-behavior/                   ← principles + shared references for UI Behavior
│       └── messages/                      ← shared message templates and wording conventions
└── workspace/                             ← per-feature working area (not committed)
    └── <feature-name>/
        ├── input/                         ← env_<slug>.md, context_<slug>.md, idea_<slug>.md
        ├── docs/                          ← generated BA doc sections (Brief through Messages)
        └── ba_doc_<slug>.md               ← final packaged document
```
