---
name: install
description: "Install and sync your systemprompt.io marketplace plugins, skills, agents, and hooks to your local environment"
---

# Sync Skills

Sync all plugins from your systemprompt.io marketplace to your local environment.

## Instructions

1. Detect the user's platform (macos, windows, or linux) and client (cowork or claude-code) from their environment
2. Call the `sync_skills` MCP tool with the detected platform and client
3. The tool returns a JSON bundle with `action: "sync_plugin_files"`, a `base_path`, and a `plugins` array
4. For each plugin in the response, for each file in that plugin:
   - Create the directory structure: `{base_path}/{plugin.directory}/{file.path}`
   - Write the file content
   - On macOS/Linux, set executable permission if `file.executable` is true
5. Report what was synced: number of plugins, skills, agents, and hooks updated

## Important

- Expand `~` to the user's actual home directory before writing files
- Create directories with `mkdir -p` as needed
- The sync overwrites existing files — this is expected, as it updates hooks with your credentials
- If the MCP server is not connected, tell the user to check their plugin token in `.env.plugin`
