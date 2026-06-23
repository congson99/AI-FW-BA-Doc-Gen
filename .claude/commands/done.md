---
name: "Complete Feature Task"
description: "Publish BA Doc to Confluence, update Jira status, and optionally clear the feature. Usage: /done <Feature Name>"
---

You are a Senior Business Analyst completing a feature task. Execute each step in order, pausing to interact with the user as instructed.

## Input

`$ARGUMENTS` is the **Feature name** exactly as typed by the user.

- If `$ARGUMENTS` is empty → ask the user: "What is the feature name?"

## Pre-flight

1. Derive folder name: kebab-case of Feature name (e.g. "Create PO" → `create-po`)
2. Derive file slug: replace `-` with `_` (e.g. `create-po` → `create_po`)
3. Read `workspace/<folder-name>/env_<slug>.md` — if missing, stop: "Environment file not found. Run `/start <Feature Name>` first."
4. Check `workspace/<folder-name>/ba_doc_<slug>.md` exists — if missing, stop: "BA Doc not found. Run `/gen-ba-doc <Feature Name>` first."

---

## Step 0 — Check Manual Tasks

- Read `workspace/<folder-name>/manual_tasks_<slug>.md`.
- If the file does not exist → skip this step and continue.
- If the file exists but is empty or contains only the heading with no tasks → continue.
- If the file contains any remaining tasks (lines starting with `- [ ]`) → stop and tell the user:
  > "The following manual tasks are still pending:
  > [list each `- [ ]` task]
  > Complete these tasks, then remove them from `manual_tasks_<slug>.md` before running /done again."

---

## Step 1 — Publish BA Doc to Confluence

### 1a. Validate the Confluence link

- Find the line under **Confluence output pages:** that starts with `- BA Doc:` in `env_<slug>.md`.
- Extract the URL value after `BA Doc:`.
- If the value is a placeholder (`<confluence-page-url>`), empty, or not a valid URL → stop this step and tell the user:
  > "No Confluence page link found. Please paste the Confluence page URL and I'll update `env_<slug>.md` and continue."
  - Wait for the user to provide the URL.
  - Update the `BA Doc:` line in `env_<slug>.md` with the provided URL.
  - Continue to 1b.

### 1b. Read the Confluence page

- Extract the numeric page ID from the URL. Confluence URLs follow the pattern: `https://<domain>/wiki/spaces/<SPACE>/pages/<pageId>/...`
- Use the Atlassian MCP tool `getConfluencePage` with the extracted page ID to fetch the current page.
- If the call fails or the page is not found → tell the user:
  > "Could not access the Confluence page (ID: `<pageId>`). Please check the link is correct and try again."
  - Wait for the user to provide a corrected URL, update `env_<slug>.md`, and retry 1b.

### 1c. Decide whether to overwrite

- Inspect the page body returned in 1b.
- If the body is empty, blank, or contains only the page title with no body content → proceed directly to 1d without asking.
- If the body already has content → ask the user:
  > "The Confluence page already has content. Overwrite it with the local BA Doc? (yes/no)"
  - **no** → skip to Step 2 and note: "Confluence page left unchanged."
  - **yes** → proceed to 1d.

### 1d. Update the Confluence page

- Read the full text of `workspace/<folder-name>/ba_doc_<slug>.md`.
- Convert the Markdown content to Confluence storage format (XHTML):
  - `# Heading` → `<h1>`, `## Heading` → `<h2>`, etc.
  - `**bold**` → `<strong>`
  - `- item` → `<ul><li>`
  - `` `code` `` → `<code>`
  - `---` → `<hr/>`
  - Preserve all text, numbering, and links exactly.
- Use the Atlassian MCP tool `updateConfluencePage` with:
  - The page ID from 1b
  - Version = current version number + 1
  - The converted body content
- Confirm: "✓ Confluence BA Doc updated."

---

## Step 2 — Update Jira BA Task Status

### 2a. Validate the Jira ticket link

- Find the line **BA Task Jira ticket:** in `env_<slug>.md`.
- Extract the URL value.
- If the value is a placeholder (`<jira-ticket-url>`), empty, or not a valid URL → skip this entire step and note: "No BA Task Jira ticket found. Skipping Jira update."

### 2b. Check current status

- Extract the issue key from the URL. Jira URLs follow the pattern: `https://<domain>/browse/<ISSUE-KEY>`
- Use the Atlassian MCP tool `getJiraIssue` to fetch the issue and read its `status.name`.
- If the status is **not** "To Do" or "In Progress" → note: "Jira ticket is already **<status>**. No transition needed." and skip to Step 3.

### 2c. Offer transition options

- Use the Atlassian MCP tool `getTransitionsForJiraIssue` to fetch the list of available transitions for the issue.
- Show the user the current status and available transitions:
  > "The Jira ticket is currently **<status>**. Choose a transition:
  > 1. <transition 1 name>
  > 2. <transition 2 name>
  > ...
  > 0. Skip — leave as is"
- Wait for the user's choice.
- If the user chooses a transition → use the Atlassian MCP tool `transitionJiraIssue` with the selected transition ID, then confirm: "✓ Jira ticket transitioned to **<new-status>**."
- If the user skips → note: "Jira ticket left as **<status>**."

---

## Step 3 — Clear Feature (optional)

Ask the user:
> "Do you want to clear the feature folder `workspace/<folder-name>/`? (yes/no)"

- **no** → skip.
- **yes** → follow the clear-feature logic:
  1. Confirm with the user: "Delete `workspace/<folder-name>/` and all its contents? (yes/no)"
  2. If confirmed → delete the folder and all contents, confirm: "✓ Deleted workspace/<folder-name>/"
  3. If cancelled → note: "Feature folder kept."

---

## Summary

After all steps are complete, display:

```
## ✓ Done — <Feature Name>

| Step | Result |
|---|---|
| Confluence BA Doc | <updated / unchanged / skipped — reason> |
| Jira ticket | <transitioned to X / already X / skipped — reason> |
| Feature folder | <cleared / kept> |
```
