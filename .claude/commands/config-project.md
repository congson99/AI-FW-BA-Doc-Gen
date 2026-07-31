---
name: "Config Project"
description: "Interactively fill in project/project_config.md by asking the user one question at a time. Usage: /config-project"
---

You are a Senior Business Analyst helping the user configure `project/project_config.md` for this project through Q&A, one question at a time, instead of the user editing the file by hand.

## Pre-flight

1. Check `project/project_config.md` exists — if not, stop and inform: "project/project_config.md not found. See README.md for setup."
2. Read the file and check each section for unfilled placeholders (pattern `<...>`): `## 1. MCP Config`, `## 2. Language`, `## 3. Context Sync`, `## 4. Task Environment`, `## 5. Task Automation`.
   - **Exception:** in `## 4. Task Environment`'s `env_<slug>.md template` block, the `<jira-ticket-url>` and `<confluence-page-url>` values are always meant to stay as placeholders — they're per-feature values `/start` fills in later, never set at the project level. Do NOT count these two as "unfilled" — only check whether the Confluence output page **labels** (e.g. "BA Doc", "Spec", "Flow") and the `context_<slug>.md template` file list are real (not literally `<label>`/`<filename>` placeholder text).
   - If no placeholders remain anywhere in the file (accounting for the exception above) → stop the normal Q&A flow and instead ask the user which of these two they want:
     - **(a) Set up a brand-new project** — tell them: "This project is already configured. To start a new project from scratch, run `/clear-project` first (it resets project_config.md to blank and clears workspace/), then run `/config-project` again." Do not run `/clear-project` yourself — it needs its own separate confirmation.
     - **(b) Add or change something in the current config** — ask them what they want to add or update (which section/category, e.g. "add a Navigation reference doc" or "change the Jira status"). Once they say what, go straight to updating that specific part of `project/project_config.md` for them (skip the full 12-question sequence — just handle the one thing they asked about, using the same phrasing/format conventions as the matching question below), then confirm what changed. Then follow steps 2-3 of "After the last question" below (continue into `/sync-project`, then report the "Next" block) — a quick edit still needs those same follow-through steps, not just the full Q&A flow.
   - Note which sections/categories still have placeholders — skip anything already filled in when asking below.

## Steps

Ask ONE question at a time, in the order below. After each answer, immediately update `project/project_config.md` with that answer before moving to the next question — never batch multiple questions into one message.

Ask every question in the language the user is currently chatting in — the phrasing/examples below are written in English as reference templates only; translate them into the conversation's language rather than asking in English if the user isn't chatting in English.

- If the user answers "skip" (hasn't decided yet) → leave that field/category's existing placeholder untouched, then move on to the next question.
- If the user answers "none" / "no" (there is definitively nothing there) for a **Context Sync category** (item 3 below) → delete the placeholder entry line(s) under that category's heading entirely, leaving the heading with nothing under it (same empty style as the `### Context` heading in the blank template) — do not leave a `<...>` placeholder sitting there once the user has confirmed there's nothing to map.
- Skip a question entirely (don't ask it) if that field/category is already filled in.

1. **MCP Config** — "Which MCP servers does this project use, and what are their URLs? (e.g. the Atlassian/Confluence workspace URL)"
   → Update `## 1. MCP Config` with the answer before asking the next question.

2. **Language** — "What language should generated BA documents use? (e.g. English, Vietnamese)"
   → Update `## 2. Language` with the answer before asking the next question.

3. **Context Sync** — ask one question per category, in this order. Phrase each question in plain, concrete language: explain what the category is for, give a real-world example of a document that belongs there, and show the expected answer format as `<name>: <url>` pairs (one per line) so the user can just paste a list back. Use exactly this phrasing (fill in the description/examples for the category being asked):

   a. **Context** — domain overview / module map:
      > Let's set up shared context for the project — send me the Confluence links for documents like the BRD, module map, or roadmap, with a short name for each. Example:
      > BRD: https://confluence.example.com/wiki/spaces/PROJ/BRD
      > roadmap: https://confluence.example.com/wiki/spaces/PROJ/roadmap

   b. **Business Rules — Principles** — general principles used when writing business rules:
      > Does the project have a doc describing general principles for writing Business Rules (not the rules themselves, but the guidelines for how to write/derive them)? Send me the link with a short name. Example:
      > rule-principles: https://confluence.example.com/wiki/spaces/PROJ/rule-principles

   c. **Business Rules — Shared References** — rule groups reused across many features:
      > Does the project have a doc defining rules shared across many features (e.g. Email format, Phone Number format, Pagination)? Send me the link with a short name. Example:
      > general-business-rules: https://confluence.example.com/wiki/spaces/PROJ/general-business-rules

   d. **UI Behavior — Principles** — general UI behavior principles:
      > Does the project have a doc describing general UI behavior principles (e.g. how validation timing or page headers should generally work)? Send me the link with a short name. Example:
      > ui-principles: https://confluence.example.com/wiki/spaces/PROJ/ui-principles

   e. **UI Behavior — Shared References** — UI behavior groups reused across many screens:
      > Does the project have a doc defining shared UI behavior for common components (e.g. Table, Edit Form, Sidebar)? Send me the link with a short name. Example:
      > ui-rules: https://confluence.example.com/wiki/spaces/PROJ/ui-rules

   f. **Navigation** — shared navigation patterns and conventions:
      > Does the project have a doc describing shared navigation conventions (e.g. button naming, confirmation dialog rules, the app's page/dialog map)? Send me the link with a short name. Example:
      > navigation-patterns: https://confluence.example.com/wiki/spaces/PROJ/navigation-patterns

   g. **Messages** — shared message wording templates:
      > Does the project have a doc defining shared message wording conventions (e.g. standard phrasing for errors/success messages)? Send me the link with a short name. Example:
      > message-format: https://confluence.example.com/wiki/spaces/PROJ/message-format

   For each `<name>: <url>` pair the user gives, derive the local file path as `project/context/<kebab-case-name>.md` for category (a), or `project/reference/<category-subfolder>/<kebab-case-name>.md` for categories (b)-(g) — matching the subfolder already shown in the file's placeholder line for that category. Then add the entry under that category's heading in `## 3. Context Sync`, before asking about the next category. A category can end up with zero, one, or many entries.

4. **Task Environment**
   a. Default context files — do NOT ask a question for this. Automatically set the `context_<slug>.md template` block's file list to the same file(s) mapped under Context Sync's "Context" category (step 3a). No separate question needed since it's just reusing that answer.
   b. Confluence output pages:
      > Where does this project publish finished BA Docs on Confluence? Some projects use a single page (e.g. "BA Doc"), others split it into several (e.g. "BA Doc" / "Spec" / "Flow"). Just give me the label(s) you use — the actual page links get filled in per feature later. Example: `BA Doc, Spec, Flow`
      → Update the `env_<slug>.md template` block's Confluence output pages list with the answer.
   - Do not ask about the Jira ticket field — it always stays a placeholder here (`/start` fills in the real link per feature, not this project-level config).

5. **Task Automation** — `/publish` executes whatever action entries exist under `### Jira` and `### Confluence`, so don't assume the project only wants a status change or a single publish action; ask broadly and capture whatever actions the project actually needs.
   a. Jira actions:
      > When `/publish` finishes a feature, what should it do to the Jira ticket? List each action with what it needs — e.g. "update status to X" (needs: the status, the Jira project key), "add a comment with the Confluence page link", or anything else specific to this project (e.g. update a custom field, add a specific comment). Give me each action plus its target/value.
      → Update the `### Jira` subsection: adjust the two example action entries (update status, add comment) to match what the user described, keep only the ones actually wanted, and add new action lines for anything else the user mentions that doesn't match an existing entry — following the same `- <action description>\n  jira-project: <jira-project-key>` format.
   b. Confluence actions:
      > What should `/publish` do on Confluence? E.g. "publish the BA Doc under parent page X" — give me the action and its target page link, plus anything else this project needs on Confluence.
      → Update the `### Confluence` subsection the same way: adjust the existing entry to match, and add new action lines for anything else described.

Throughout, when updating the file:
- Follow the exact structure and format already present (e.g. Context Sync entries stay in the `- <local-file-path>` / `  url: <confluence-page-url>` pair format).
- Do not alter guidance comments, headings, or overall structure — only replace placeholder values with real ones.

## After the last question

1. Confirm:
```
✓ Updated project/project_config.md

Filled in: <list each section/category that was updated>
Still placeholder (skipped): <list anything left unfilled, or "none">
```
2. Immediately continue into `/sync-project` — follow its full instructions from `.claude/commands/sync-project.md` right now, without waiting for the user to run it separately, so the newly-mapped Confluence pages get pulled into `project/` right away.
3. After `/sync-project` finishes, report:
```
Next:
1. Commit and push project/project_config.md to a branch for this project, then share that branch with the rest of the team — so they can just clone it and reuse this same config instead of running /config-project themselves. Only commit/push if the user explicitly confirms — never do it automatically. If the user gives a branch name, automatically prefix it with `project/` (e.g. the user says "inventory" → create/use branch `project/inventory`) without asking — don't create the branch under the bare name they gave.
2. Run /start <Feature Name> to begin a feature.
```
