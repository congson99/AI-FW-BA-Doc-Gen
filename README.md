# DC32 BA Documentation Claude Tool

v2.1

---

## Table of Contents

1. [Overview](#1-overview)
2. [Setup Environment](#2-setup-environment-one-time-only)
3. [Configure the Project](#3-configure-the-project-for-ba-leader)
4. [Generating BA Documents](#4-generating-ba-documents)
5. [Available Commands](#5-available-commands)
6. [Folder Structure](#6-folder-structure)

---

## 1. Overview

**DC32 BA Documentation Claude Tool** is an AI-assisted framework built on Claude Code that lets Business Analysts automatically generate a complete BA documentation set — using already-analyzed project data plus clarifying questions along the way — then publish it straight to Confluence. A few things are fixed by design, the same for every project:

- The BA documentation set has a fixed architecture.
- The doc generation flow is fixed.
- Only Atlassian is supported as an MCP connection.

### Document Architecture

1. Brief
2. Acceptance Criteria (AC)
3. Business Rules
4. Data Definition
5. Navigation
6. Flow
7. UI Behavior
8. Messages

### Generation Flow

1. `/start` — initialize the feature folder, env file, and context file
2. `/gen-ba-doc` — runs the following commands in sequence to generate the BA doc:
   - `/investigate` — generate the Idea file from project context
   - `/gen-brief` — generate Brief
   - `/gen-ac` — generate Acceptance Criteria
   - `/gen-business-rule` — generate Business Rules
   - `/gen-data-definition` — generate Data Definition
   - `/gen-navigation` — generate Navigation
   - `/gen-flow` — generate Flow
   - `/gen-ui-behavior` — generate UI Behavior
   - `/gen-messages` — generate Messages
   - `/package` — combine all docs into a single BA Doc
3. `/publish` — publish the BA Doc to Confluence and automatically run whatever tasks were configured (e.g. update Jira status, update Confluence page content, etc.)

See [4. Generating BA Documents](#4-generating-ba-documents) for the full walkthrough.

**Where to start:**
- If you're a BA and need to generate a BA doc for a project that's already set up: go to [2. Setup Environment](#2-setup-environment-one-time-only), then [4. Generating BA Documents](#4-generating-ba-documents).
- If you're the BA leader setting up a brand-new project: go to [2. Setup Environment](#2-setup-environment-one-time-only), then [3. Configure the Project](#3-configure-the-project-for-ba-leader). Do it once, then share `project/project_config.md` with everyone on the team.

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

Run `/sync-project` to fetch the Confluence pages mapped in `project/project_config.md` into local `project/` files. If MCP servers aren't connected yet, `/sync-project` connects them automatically first, then proceeds with the sync.

> If `project/project_config.md` doesn't have any content yet, contact your team leader to get the right file for this project.

> Re-run `/sync-project` any time the source data changes to pull the latest content locally.

---

## 3. Configure the Project (for BA leader)

> This section is intended only for the BA leader who is setting up a new project or updating its configuration. If you are only here to generate documents for a project that has already been configured, you may skip this section and proceed directly to [4. Generating BA Documents](#4-generating-ba-documents).

Configure it once, then push `project/project_config.md` to the repo so the whole team can clone and reuse it — only needs to be done once per project (or again whenever the configuration needs updating).

```
/config-project
```

Run this to configure interactively — asks one question at a time and builds `project/project_config.md` as you go.

> `project/project_config.md` is not meant to be edited by hand — `/config-project` is the only supported way to set it up or change it. Skip anything you don't have yet; run it again any time to fill in the rest or change values.

---

## 4. Generating BA Documents

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
| `/clear-project` | Delete synced context/reference files, reset project_config.md to its unconfigured state, and clear workspace/ |
| `/clear-workspace` | Delete all feature folders in workspace/ |
| `/config-project` | Interactively build project_config.md via Q&A (the only supported way to configure it) |
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
│   └── styles/                            ← format rules, one file per doc type + style_general.md
├── project/                               ← project-level context
│   ├── project_config.md                  ← project config (tracked — committed unconfigured; run /config-project to set it up locally per project)
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
