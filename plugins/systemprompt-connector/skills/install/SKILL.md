---
name: install
description: "Install and sync your systemprompt.io marketplace plugins, skills, agents, and hooks to your local environment"
---

# Install systemprompt.io

Sync all plugins from your systemprompt.io marketplace to your local environment using the **skill-manager** MCP server.

## Instructions

1. Detect the user's platform (`macos`, `windows`, or `linux`) and client (`cowork` or `claude-code`) from their environment
2. Call the **skill-manager** MCP server's `sync_skills` tool:
   ```json
   {
     "platform": "<detected platform>",
     "client": "<detected client>"
   }
   ```
3. The tool returns a JSON bundle containing:
   - `base_path` — where plugins are installed (e.g. `~/.claude/plugins` or `~/.cowork/plugins`)
   - `plugins` — array of plugin bundles, each with `directory` and `files`
4. For each plugin, for each file:
   - Resolve the full path: `{base_path}/{plugin.directory}/{file.path}`
   - Expand `~` to the user's actual home directory
   - Create parent directories with `mkdir -p`
   - Write the file content
   - On macOS/Linux, `chmod +x` files where `executable` is true
5. Report what was synced: number of plugins, skills, agents, and hooks updated

## What this does

- Pulls all your marketplace plugins (skills, agents, MCP configs)
- Writes hooks with your authenticated token so analytics tracking works
- Overwrites existing plugin files — this is expected, it keeps everything in sync

## Troubleshooting

- If the skill-manager MCP server is not connected, check that `SYSTEMPROMPT_PLUGIN_TOKEN` is set correctly in your plugin environment settings
- You can re-run `/install` at any time to sync the latest changes from your marketplace
