---
name: "Check Feature Status"
description: "Check which BA documents have been generated for a feature and suggest the next step. Usage: /check <Feature Name>"
---

You are a Senior Business Analyst reviewing documentation progress for a feature.

## Input

`$ARGUMENTS` is the **Feature name** exactly as typed by the user.

- If `$ARGUMENTS` is empty → list all feature folders under `workspace/` and ask: "Which feature do you want to check?"

## Feature Name Normalization

1. Apply title case and uppercase known acronyms: `PO`, `PR`, `IR`, `SI`, `BA`, `SKU`, `ID`.
2. Derive folder name: kebab-case (e.g. "Update PO" → `update-po`)
3. Derive file slug: replace `-` with `_` (e.g. `update-po` → `update_po`)

## Steps

### 1. Check if the feature folder exists

- If `workspace/<folder-name>/` does not exist → stop and output:

```
Feature `<Feature Name>` not found.

→ Run /start <Feature Name> to initialize it.
```

### 2. Scan the folder for known files

Check for the existence of each file below (true/false):

| # | File | Label |
|---|---|---|
| 1 | `env_<slug>.md` | Environment |
| 2 | `idea_<slug>.md` | Idea |
| 3 | `brief_<slug>.md` | Brief |
| 4 | `ac_<slug>.md` | Acceptance Criteria |
| 5 | `business_rule_<slug>.md` | Business Rules |
| 6 | `data_definition_<slug>.md` | Data Definition |
| 7 | `navigation_<slug>.md` | Navigation |
| 8 | `flow_<slug>.md` | Flow |
| 9 | `ui_behavior_<slug>.md` | UI Behavior |
| 10 | `messages_<slug>.md` | Messages |
| 11 | `ba_doc_<slug>.md` | BA Doc |

### 3. For files that exist, detect issues

- **env file**: check if it still contains unfilled placeholders like `<jira-ticket-url>` or `<confluence-page-url>`. If yes → flag as "⚠ has unfilled placeholders".
- **idea file**: check if it still contains `<describe the feature goal` or `<field>` placeholder content. If yes → flag as "⚠ not yet filled in".
- All other files: if the file exists, treat it as complete (✓).

### 4. Determine the next step

Use this priority order — stop at the first condition that is true:

1. `env_<slug>.md` missing → next: `/start <Feature Name>`
2. `env_<slug>.md` has unfilled placeholders → next: "Fill in the placeholders in `env_<slug>.md`"
3. `idea_<slug>.md` missing or not yet filled → next: "Fill in `idea_<slug>.md`", then `/gen-brief <Feature Name>`
4. `brief_<slug>.md` missing → next: `/gen-brief <Feature Name>`
5. `ac_<slug>.md` missing → next: `/gen-ac <Feature Name>`
6. `business_rule_<slug>.md` missing → next: `/gen-business-rule <Feature Name>`
7. `data_definition_<slug>.md` missing → next: `/gen-data-definition <Feature Name>`
8. `navigation_<slug>.md` missing → next: `/gen-navigation <Feature Name>`
9. `flow_<slug>.md` missing → next: `/gen-flow <Feature Name>`
10. `ui_behavior_<slug>.md` missing → next: `/gen-ui-behavior <Feature Name>`
11. `messages_<slug>.md` missing → next: `/gen-messages <Feature Name>`
12. `ba_doc_<slug>.md` missing → next: `/gen-ba-doc <Feature Name>`
13. All files exist → next: `/done <Feature Name>`

### 5. Output the status report

Print the report in this exact format:

```
## Feature Status — <Feature Name>

| Doc | Status |
|---|---|
| Environment | ✓ Ready / ⚠ Has unfilled placeholders / ✗ Missing |
| Idea | ✓ Ready / ⚠ Not yet filled in / ✗ Missing |
| Brief | ✓ Ready / ✗ Missing |
| Acceptance Criteria | ✓ Ready / ✗ Missing |
| Business Rules | ✓ Ready / ✗ Missing |
| Data Definition | ✓ Ready / ✗ Missing |
| Navigation | ✓ Ready / ✗ Missing |
| Flow | ✓ Ready / ✗ Missing |
| UI Behavior | ✓ Ready / ✗ Missing |
| Messages | ✓ Ready / ✗ Missing |
| BA Doc | ✓ Ready / ✗ Missing |

→ Next step: <command or action to take>
```

- Use `✓ Ready` when the file exists and has no detected issues.
- Use `⚠ <short issue description>` when the file exists but has a problem.
- Use `✗ Missing` when the file does not exist.
- The "→ Next step" line must be actionable: either a slash command the user can copy-paste, or a clear instruction.
