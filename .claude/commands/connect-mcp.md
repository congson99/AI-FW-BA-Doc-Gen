---
name: "Connect MCP"
description: "Connect to MCP servers listed in project/project_config.md. Usage: /connect-mcp"
---

You are connecting to the MCP servers configured for this project.

## Steps

1. Check whether `project/project_config.md` exists:
   - If not → stop and inform the user: "project/project_config.md not found. Run /init first."

2. Read `project/project_config.md` and locate the `## 1. MCP Config` section. Parse all entries within that section. Each entry has this format:
   ```
   - <server-name>: <url>
   ```
   Stop parsing at the next `## ` heading.

3. Validate entries:
   - Skip any entry where the URL is still a placeholder (e.g. `<confluence-mcp-url>`).
   - If all entries are placeholders → stop and inform:
     ```
     No MCP URLs configured. Open project/project_config.md and fill in the URLs under "## 1. MCP Config".
     ```

4. For each valid entry, initiate the connection:
   - **Atlassian** → Paste the configured URL into the chat to trigger the Atlassian authentication flow. Inform the user:
     ```
     Connecting to Atlassian MCP...
     Initiating authentication — follow the prompts to complete the Atlassian connection.
     ```
     Then use the URL from the config to start the connection.
   - **Other servers** → Display the server name and URL, and instruct the user to add it manually to their Claude Code MCP settings if auto-connect is not supported.

5. Update `project/project_config.md` with the MCP connect timestamp:
   - Get the current date and time at the moment connection completes.
   - Check if a `## 0. Status` section already exists in the file:
     - **If it exists** → update or add the `Latest MCP connect:` line in place with the new timestamp.
     - **If it does not exist** → insert the following block immediately after the `# Project Config` title line (with one blank line before the next section):
       ```
       ## 0. Status
       Latest MCP connect: YYYY/MM/DD HH:MM:SS
       ---
       ```
   - Use the format `YYYY/MM/DD HH:MM:SS` for the timestamp.

6. Report results:
   ```
   MCP Connection Summary:

   ✓ Atlassian — connected
   ✗ <server-name> — failed: <reason>
   ⚠ <server-name> — requires manual setup (see Claude Code MCP settings)

   Skipped (no URL): <count> entries
   ```
