---
name: "Sync Project Files"
description: "Fetch content from Confluence pages into local project files based on project/sync_config.md. Usage: /sync"
---

You are syncing project context files from Confluence into the local `project/` folder.

## Steps

1. Check whether `project/sync_config.md` exists:
   - If not → stop and inform the user: "project/sync_config.md not found. Run /init first."

2. Read `project/sync_config.md` and parse all entries. Each entry has this format:
   ```
   - <local-file-path>
     url: <confluence-page-url>
   ```

3. Validate entries:
   - Skip any entry where `url:` is still a placeholder (e.g. `<confluence-page-url>`).
   - If all entries are placeholders → stop and inform: "No Confluence URLs found in project/sync_config.md. Fill in the URLs first."

4. For each valid entry:
   a. Fetch the Confluence page content using the provided URL.
   b. Convert the page content to clean Markdown.
   c. Write the result to the specified local file path, creating the file if it does not exist.
   d. Track success or failure per entry.

5. Report results:
```
Sync complete:

✓ project/context/project.md — fetched from <url>
✓ project/reference/<filename>.md — fetched from <url>
✗ project/context/<filename>.md — failed: <reason>

Skipped (no URL): <count> entries
```

6. After syncing, scan the following folders for **orphaned files** — `.md` files that exist locally but have no matching entry in `project/sync_config.md`:
   - `project/context/`
   - `project/reference/` (top-level files only)
   - `project/reference/business-rules/principles/`
   - `project/reference/business-rules/shared-references/`
   - `project/reference/ui-behavior/principles/`
   - `project/reference/ui-behavior/shared-references/`
   - `project/reference/navigation/`
   - `project/reference/messages/`

   If orphaned files are found → ask the user:
   ```
   The following local files are not mapped in sync_config.md:
     - project/reference/business-rules/old-rules.md
     - ...

   Delete these files? (yes/no)
   ```
   - **yes** → delete all listed files and confirm: `✓ Deleted <count> orphaned file(s).`
   - **no** → leave them untouched and note: `Orphaned files kept.`

   If no orphaned files are found → skip this step silently.
