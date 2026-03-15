# [OpenClaw] Discord Multi-Agent Template

[English](README.md) | [简体中文](README.zh-CN.md)

This folder is a **role-agnostic** template for running a **multi-agent OpenClaw deployment** on a chat platform (Discord used as the concrete example).

It distills the core collaboration patterns from an existing OpenClaw setup into a reusable “skill-style” spec + configuration flow.

## Copy/Paste Prompt (recommended)

### For Humans

Copy and paste this prompt to your LLM agent (Claude Code, AmpCode, Cursor, etc.):

```
You are setting up an OpenClaw multi-agent deployment using this GitHub repository:
https://github.com/saaak/openclaw-discord-multiAgent

Fetch and read ONLY these docs first (do not clone/download the whole repository):
- https://docs.openclaw.ai/gateway/configuration-reference
- https://raw.githubusercontent.com/saaak/openclaw-discord-multiAgent/main/docs/CONFIGURATION_FLOW.md
- https://raw.githubusercontent.com/saaak/openclaw-discord-multiAgent/main/docs/AGENT_COLLABORATION_PROTOCOL.md

Then read the user's existing openclaw.json first.
If the user has not provided required config values, ask for them (do not invent/hallucinate values).
If a form tool is available, present a form for required fields; if no form tool is available, send a clear required-fields checklist for the user to fill.
For LLM model, Discord channels, workspace, and similar settings: if the system already has existing values, ask the user whether to reuse existing ones or create new ones.
After all required values are confirmed, directly update openclaw.json.
Prefer additive changes; do not overwrite or remove unrelated existing settings.
If any required value is still missing, keep placeholders like <REQUIRED_VALUE> and explicitly list missing items.
Do NOT hard-code extra IDs, and do NOT include secrets in committed files.
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

## Collaboration capabilities

- Supports **agent-to-agent invocation** for specialist handoff.
- Supports **task dispatching** from coordinator to specialist agents.
- Supports **centralized aggregation** by a unified coordinator agent before publishing responses.

## Demo screenshots

### Dispatch instructions

![Dispatch instructions](imgs/DispatchInstructions.png)

### Response command

![Response command](imgs/ResponseCommand.png)

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
