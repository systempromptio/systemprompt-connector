---
name: publish
description: "Publish local plugin changes (skills, agents, MCP servers) back to your systemprompt.io marketplace"
---

# Publish to systemprompt.io

Push local plugin changes back to your systemprompt.io marketplace using the **skill-manager** MCP server.

## Instructions

### 1. Get current remote state
Call `list_plugins` from the **skill-manager** MCP server to get all plugins with their skills, agents, and MCP servers.

### 2. Find local plugin directories
Detect the user's platform and client to resolve the plugins base path:
- **Cowork macOS/Linux:** `~/.cowork/plugins/`
- **Cowork Windows:** `~/AppData/Roaming/Cowork/plugins/`
- **Claude Code macOS/Linux:** `~/.claude/plugins/`
- **Claude Code Windows:** `~/AppData/Roaming/Claude/plugins/`

Read each plugin directory looking for:
- `skills/*/SKILL.md` — skill files (the content between the frontmatter `---` blocks is metadata, the rest is the skill content)
- `agents/*.md` — agent files (the file content is the system prompt)
- `.mcp.json` — MCP server configurations

### 3. Diff local vs remote
For each local skill/agent, match against the remote list by name:
- **New** (exists locally but not remotely): needs `create_skill` or `create_agent`
- **Changed** (exists both places, content differs): needs `update_skill` or `update_agent`
- **Unchanged**: skip
- **Deleted** (exists remotely but not locally): report but do NOT auto-delete — ask the user first

### 4. Push changes
For each plugin, use the matching `target_plugin_id` from step 1:

**New skills:**
```json
create_skill({
  "name": "<skill-name>",
  "description": "<from frontmatter>",
  "content": "<full SKILL.md content>",
  "target_plugin_id": "<plugin-id>"
})
```

**Updated skills:**
```json
update_skill({
  "skill_id": "<skill-id from remote list>",
  "content": "<full SKILL.md content>"
})
```

**New agents:**
```json
create_agent({
  "name": "<agent-name>",
  "description": "<first line or summary>",
  "system_prompt": "<full agent .md content>"
})
```

**Updated agents:**
```json
update_agent({
  "agent_id": "<agent-id from remote list>",
  "system_prompt": "<full agent .md content>"
})
```

### 5. Report results
Summarize what was published:
- Number of skills created / updated
- Number of agents created / updated
- Any items that exist remotely but not locally (potential deletions — list them for the user)

## Important

- Always confirm with the user before deleting remote items
- Do NOT publish the `install` or `publish` skills themselves — these are connector-only skills
- If a plugin cannot be matched by name, ask the user which plugin to target
- If the skill-manager MCP server is not connected, tell the user to check their plugin token
