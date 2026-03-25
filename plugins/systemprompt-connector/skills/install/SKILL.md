---
name: install
description: "Install and sync your systemprompt.io marketplace plugins, skills, agents, and hooks to your local environment"
---

# Install systemprompt.io

Sync your complete marketplace from systemprompt.io to your local environment using the **skill-manager** MCP server.

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
   - `base_path` — the marketplaces directory (e.g. `~/.cowork/plugins/marketplaces`)
   - `marketplace_file` — the marketplace manifest (`.claude-plugin/marketplace.json`)
   - `plugins` — array of plugin bundles, each with files already nested under `plugins/{id}/`
4. Write all files relative to `{base_path}/systemprompt-marketplace/`:
   - First write the marketplace manifest: `{base_path}/systemprompt-marketplace/{marketplace_file.path}`
   - Then for each plugin, write each file: `{base_path}/systemprompt-marketplace/{file.path}`
   - Expand `~` to the user's actual home directory
   - Create parent directories with `mkdir -p`
   - On macOS/Linux, `chmod +x` files where `executable` is true
5. Report what was synced: number of plugins, skills, agents, and hooks updated

## Result structure

After install, the local marketplace looks like:
```
{base_path}/systemprompt-marketplace/
  .claude-plugin/marketplace.json
  plugins/systemprompt/.claude-plugin/plugin.json
  plugins/systemprompt/skills/...
  plugins/systemprompt/hooks/hooks.json
  plugins/{other-plugin-id}/...
```

This matches the exact structure Cowork and Claude Code expect for marketplace installations.

## What this does

- Writes a complete marketplace with all your plugins (skills, agents, MCP configs)
- Hooks are written with your authenticated token so analytics tracking works
- Overwrites existing marketplace files — this keeps everything in sync with the remote

## Troubleshooting

- If the skill-manager MCP server is not connected, check that `SYSTEMPROMPT_PLUGIN_TOKEN` is set correctly in your plugin environment settings
- You can re-run `/install` at any time to sync the latest changes from your marketplace
