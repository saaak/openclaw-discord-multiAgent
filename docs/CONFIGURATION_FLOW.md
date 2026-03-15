# Configuration Flow (Discord example, not hard-coded)

This document shows how to stand up a multi-agent OpenClaw deployment using Discord as the concrete chat platform.

It is written so an AI model (or an operator) can complete setup with only:

- LLM provider config (or local gateway)
- bot token(s)
- guild/server ID
- channel IDs
- optional allowlist user IDs

## 0) Confirm role relationship style (before technical setup)

Before any config/workspace edits, confirm with the user:

- Relationship style for dispatcher vs worker agents (examples: modern company, guild, imperial court, or custom)
- Naming mapping for role IDs (which role maps to `coordinator`, which roles map to `specialist-*`)
- Dispatch/execute responsibility boundaries (who routes tasks, who performs execution)

This avoids naming/routing rework later and ensures role semantics are explicit.

## 1) Decide: single bot vs multi-bot

### Option A — Single bot (simpler)

- One Discord bot token.
- All agents share the same Discord identity.
- Works well for routing and automation, but message authorship is less explicit.

### Option B — One bot per agent (recommended for clarity)

- One Discord bot token per agent “account”.
- Each specialist publishes messages under its own identity.
- Requires creating multiple Discord applications/bots.

## 2) Create Discord apps/bots

For each bot you plan to run:

1. Create a Discord application.
2. Add a bot user.
3. Copy the bot token.
4. Enable only the gateway intents you need.

Notes:
- If you want the bot to read message content in servers, Discord may require enabling the **Message Content Intent**.
- Keep permissions minimal; rely on channel-level permission overwrites where possible.

## 3) Identify Discord IDs

You will need:

- `GUILD_ID`
- `COORDINATOR_CHANNEL_ID`
- One channel per specialist (optional but recommended)
- A shared “meeting room” channel (optional)

## 4) Fill the OpenClaw config template

Start from: `templates/openclaw.json`

### 3.1 Models/provider

Configure at least one provider + one model entry.

### 3.2 Agents

Define:

- `coordinator` agentId
- `specialist-*` agents (any number)

Each agent should have:
- `id`
- `workspace`
- (optional) `agentDir`
- model selection

### 3.3 Enable agent-to-agent communication

Set `tools.agentToAgent.enabled = true` and allow the involved agents.

### 3.4 Discord accounts

Under `channels.discord.accounts`, add:

- `coordinator` account with token
- each specialist account (if multi-bot)

Also configure per-guild channel allowlists so bots only receive events where needed.

### 3.5 Bindings (routing)

Add `bindings[]` entries to route inbound events to the intended agent based on:

- platform: `discord`
- accountId
- peer kind + id (channel)
- guildId

Practical routing patterns:

1. Coordinator channel → coordinator agent
2. Specialist channels → the matching specialist agent
3. Meeting room → either coordinator only, or all agents (depending on your workflow)

## 5) Create NEW workspaces and AGENTS.md (do not mutate existing workspaces)

Use templates under `templates/workspaces/`.

Important policy:

- For a new role relationship setup, create a new workspace set with new paths.
- Do **not** modify existing workspace folders for the new setup.
- Keep existing workspaces as-is to avoid breaking previously working deployments.

Minimum recommended:

- `workspaces/coordinator/AGENTS.md` — delegation rules (spawn vs send)
- `workspaces/specialist/AGENTS.md` — completion contract + publish rules

Then bind these new workspace paths in your runtime `openclaw.json` agent entries.

## 6) Start OpenClaw

Run your OpenClaw gateway in your environment (example):

```bash
openclaw start
```

## 6.1) Credential storage recommendation

- Keep your runtime `openclaw.json` outside of git, or commit only a placeholder/template file.
- Prefer environment variables or an external secret store for:
  - LLM API keys
  - Discord bot tokens
  - gateway auth token

If you must keep tokens in runtime `openclaw.json` for local testing, treat the file as a secret.

## 7) Validate the system

### Routing

1. Send a task in the coordinator channel.
2. Confirm the coordinator delegates.
3. Confirm the specialist publishes in the specialist channel.

### Identity (multi-bot)

If you run one bot per agent:

- Validate the message author is the specialist bot.
- If messages appear under the coordinator bot, ensure the send tool specifies the correct `accountId`.

## 8) Common failures

### “agentId not allowed”

- Add the target agent to:
  - `agents.list[].subagents.allowAgents` (for the coordinator)
  - `tools.agentToAgent.allow`

### Messages go to the wrong agent

- Fix `bindings[]` match rules.

### Specialist output appears as the wrong bot

- Ensure message sending includes the correct `accountId` for that specialist.

## 9) Notes on schema stability

OpenClaw is evolving. If a field name differs from your installed version:

1. Prefer the **official OpenClaw docs** for your version.
2. Compare with a working local `openclaw.json`.
3. Run `openclaw doctor` (if available) to validate the config.
