---
name: install
description: "Download your remote systemprompt.io marketplace and install it locally as a complete marketplace with all plugins, skills, agents, and hooks"
---

# Install Remote Marketplace

Download your remote systemprompt.io marketplace and install it locally. This pulls all your plugins, skills, agents, hooks, and MCP server configs from the remote server and writes them to your local marketplace directory so Cowork or Claude Code can use them.

## Instructions

1. Detect the user's platform (`macos`, `windows`, or `linux`) and client (`cowork` or `claude-code`) from their environment
2. Call the **skill-manager** MCP server's `sync_skills` tool:
   ```json
   {
     "platform": "<detected platform>",
     "client": "<detected client>"
   }
   ```
3. The tool returns a complete marketplace bundle from the remote server containing:
   - `base_path` — the local marketplaces directory (e.g. `~/.cowork/plugins/marketplaces`)
   - `marketplace_file` — the marketplace manifest (`.claude-plugin/marketplace.json`)
   - `plugins` — array of plugin bundles, each with files nested under `plugins/{id}/`
4. Write all files relative to `{base_path}/systemprompt-marketplace/`:
   - First write the marketplace manifest: `{base_path}/systemprompt-marketplace/{marketplace_file.path}`
   - Then for each plugin, write each file: `{base_path}/systemprompt-marketplace/{file.path}`
   - Expand `~` to the user's actual home directory
   - Create parent directories with `mkdir -p`
   - On macOS/Linux, `chmod +x` files where `executable` is true
5. Report what was installed: number of plugins, skills, agents, and hooks written

## Result

After install, your local marketplace mirrors your remote systemprompt.io marketplace:
```
{base_path}/systemprompt-marketplace/
  .claude-plugin/marketplace.json
  plugins/systemprompt/.claude-plugin/plugin.json
  plugins/systemprompt/skills/...
  plugins/systemprompt/hooks/hooks.json
  plugins/{other-plugin-id}/...
```

This is the exact structure Cowork and Claude Code expect for installed marketplaces.

## What gets installed

- All your remote plugins with their skills, agents, and MCP server configs
- **Hooks with your authenticated token** — the remote server generates hooks pre-configured with your personal JWT token, so analytics tracking works immediately without manual configuration
- Plugin environment files (`.env.plugin`) with your token and API URL
- The marketplace manifest that ties all plugins together

This is critical: the hooks returned by the MCP server contain your real token. Writing these files locally is what activates session tracking, tool usage analytics, and all other hook-based features for your account.

Re-run `/install` any time to pull the latest changes from your remote marketplace.

## Troubleshooting

- If the skill-manager MCP server is not connected, check that `SYSTEMPROMPT_PLUGIN_TOKEN` is set correctly in your plugin environment settings
