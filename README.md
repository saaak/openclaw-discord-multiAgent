# [OpenClaw] Discord Multi-Agent Template

中文说明见：[`README.zh-CN.md`](README.zh-CN.md)

This folder is a **role-agnostic** template for running a **multi-agent OpenClaw deployment** on a chat platform (Discord used as the concrete example).

It distills the core collaboration patterns from an existing OpenClaw setup into a reusable “skill-style” spec + configuration flow.

## Copy/Paste Prompt (recommended)

### For Humans

Copy and paste this prompt to your LLM agent (Claude Code, AmpCode, Cursor, etc.):

```
You are setting up an OpenClaw multi-agent deployment using this GitHub repository:
https://github.com/saaak/openclaw-discord-multiAgent

Read:
- docs/CONFIGURATION_FLOW.md
- docs/AGENT_COLLABORATION_PROTOCOL.md

Then produce a ready-to-use openclaw.json by filling templates/openclaw.json.
Use only these inputs I will provide: LLM baseUrl+apiKey, Discord bot token(s), guildId, channel IDs, (optional) allowlist user IDs.
Do NOT hard-code any other IDs, and do NOT include secrets in committed files.
Finally, output a short validation checklist to confirm routing + specialist publishing works.
```

### For LLM Agents

Open and follow the local guide:

- `docs/CONFIGURATION_FLOW.md`

## What you get

- `SKILL.md` — a generalized, reusable multi-agent collaboration “skill” (works for companies, guilds, fictional courts, etc.).
- `docs/CONFIGURATION_FLOW.md` — step-by-step setup using minimal required inputs (tokens, guild/channel IDs, allowlists).
- `docs/AGENT_COLLABORATION_PROTOCOL.md` — the routing + handoff protocol (spawn vs send, callback, publishing results).
- `templates/openclaw.json` — a **sanitized** OpenClaw config template with placeholders.
- `templates/credentials/*.json` — allowlist templates.
- `templates/workspaces/**/AGENTS.md` — role-agnostic agent instruction templates.

## Minimal inputs you must provide

1. **LLM provider config** (base URL + API key) OR a local model endpoint.
2. **Discord bot tokens**
   - Option A: 1 token (single-bot, multi-agent routing via bindings)
   - Option B: N tokens (one bot per agent account, best for clear “message authorship”)
3. **Discord IDs**: guild/server ID + channel IDs.
4. Optional: **allowlist user IDs** (recommended for private deployments).

## Safety

- Do **not** commit tokens or API keys.
- If you suspect any credential leakage: rotate the token(s) immediately.

---

Start here: `docs/CONFIGURATION_FLOW.md`.
