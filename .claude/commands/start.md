---
name: "Setup BA Environment"
description: "Initialize feature folder and env file before generating BA artifacts. Usage: /start <Feature Name>"
---

You are a Senior Business Analyst setting up the working environment for a new feature.

## Input

`$ARGUMENTS` is the **Feature name** exactly as typed by the user.

- If `$ARGUMENTS` is empty → ask the user: "What is the feature name?"

## Pre-flight

1. Check whether `## 0. Status` in `project/project_config.md` contains a `Latest sync:` line with a real timestamp (not a placeholder):
   - If not found → stop and inform the user:
     > "Project has not been synced yet. Please run /sync-project before starting a feature."

---

## Feature Name Normalization

Before any steps, normalize the feature name:

1. Apply title case: capitalize the first letter of every word.
2. Preserve known domain acronyms in UPPERCASE. Recognized acronyms for this project: `PO`, `PR`, `IR`, `SI`, `BA`, `SKU`, `ID`. Any word that matches one of these (case-insensitive) must be uppercased in full.
   - Examples: `create po` → `Create PO`, `update pr item` → `Update PR Item`, `view si` → `View SI`
3. After normalizing, check if the feature name looks valid:
   - Use `project/context/project.md` (if it exists) to cross-reference against known feature names, modules, and User Stories.
   - If the name seems like a typo, abbreviation, or doesn't match any known domain concept → show the normalized name and ask: "Did you mean **`<Normalized Feature Name>`**? Confirm to continue, or type the correct name."
   - If the name is clear and recognizable → proceed silently with the normalized name.
4. Use the confirmed, normalized feature name for all subsequent steps.

## Steps

1. Derive folder name: kebab-case of Feature name (e.g. "Create Product Category" → `create-product-category`)
2. Derive file slug: replace `-` with `_` in folder name (e.g. `create-product-category` → `create_product_category`)
3. Check if `workspace/<folder-name>/` already exists:
   - If it exists → list all files currently in the folder, then warn the user:
     > "Feature folder `workspace/<folder-name>/` already exists with the following files:
     > [list each file]
     > Running /start again will delete all of these and reinitialize the folder. Continue? (yes/no)"
   - **no** → stop. Do not change anything.
   - **yes** → delete all files and subfolders in `workspace/<folder-name>/`, then continue to step 4.
4. Create folders `workspace/<folder-name>/input/` and `workspace/<folder-name>/docs/` if they do not exist.
5. Read `project/project_config.md` and locate the `### Language` subsection under `## 1. Project Setup`. Resolve the "Document language" value — if missing, unset, or still a placeholder, resolve it as `English`. This is resolved once here and cached into `env_<slug>.md` (step below) so that `/investigate` and every `/gen-*` command can read it straight from the feature's own env file instead of re-reading `project/project_config.md` every time.

6. Read `project/project_config.md` and locate the `## 3. Task Environment` section. It contains one labeled code block: `### env_<slug>.md template`.

   Create `workspace/<folder-name>/input/env_<slug>.md` with:
   - Line 1: `**Feature name:** <normalized Feature name>`
   - Line 2: blank
   - Line 3: `**Document language:** <resolved Document language from step 5>`
   - Line 4: blank
   - Line 5 onwards: the contents of the `env_<slug>.md template` code block, verbatim, without any modification.
   - If `project/project_config.md` does not exist or the `env_<slug>.md template` block is not found → create the file with only: `**Feature name:** <normalized Feature name>` and `**Document language:** English`

   Create `workspace/<folder-name>/input/context_<slug>.md` with:
   - `# Context Files` as the header, followed by one `- <path>` line for every file found in `project/context/` (recursively). This is just a starting default — the BA can add or remove lines afterward for anything specific to this feature.
   - If `project/context/` contains no files → create the file with only `# Context Files` and a blank line.

7. Confirm:
```
✓ workspace/<folder-name>/input/env_<slug>.md
✓ workspace/<folder-name>/input/context_<slug>.md

Next: Fill in the placeholders in env_<slug>.md (Jira ticket, Confluence pages) and list every relevant context file in context_<slug>.md.
Then run /investigate <Feature Name> to continue.
```
