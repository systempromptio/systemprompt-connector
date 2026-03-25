# systemprompt.io connector

Connect your Cowork or Claude Code workspace to [systemprompt.io](https://systemprompt.io).

## Install

### Cowork

1. Download or clone this repo
2. Go to **Customize > Personal plugins > +** and upload the plugin folder
3. Configure your plugin token in the plugin environment settings
4. Run `/sync-skills` to sync your full marketplace

### Claude Code

1. Clone this repo into your plugins directory:
   ```bash
   git clone https://github.com/systempromptio/systemprompt-connector.git ~/.claude/plugins/systemprompt-connector
   ```
2. Set your token in `.env.plugin`
3. Run `/sync-skills` to sync your full marketplace

## What this does

- Connects the systemprompt.io MCP server for skill management
- Installs analytics hooks for session and tool tracking
- Provides base skills: marketplace onboarding, skill creator, agent creator, secrets manager
- `/sync-skills` pulls all your marketplace plugins and updates hooks with your credentials

## Get your token

Sign up at [systemprompt.io](https://systemprompt.io) and go to your control center to generate a plugin token.
