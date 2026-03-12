---
name: openclaw-discord-multiagent
description: |
  A generalized multi-agent collaboration protocol and configuration template for OpenClaw.
  Uses Discord as a concrete example without hard-coding any server/channel IDs.
metadata:
  clawdbot:
    emoji: "🧩"
    requires:
      bins: []
---

# OpenClaw Multi-Agent Collaboration (Role-Agnostic)

## Purpose

Enable a **Coordinator + Specialist Agents** system where agents can:

1. Route tasks to the right specialist.
2. Execute work in parallel via asynchronous delegation.
3. Publish results to the right place with a clear source identity.
4. Optionally reply directly to the requester while still notifying the coordinator.

This spec is intentionally **role-agnostic**.

Examples of “skins” you can apply without changing the mechanics:
- Company: Coordinator + Engineering/Marketing/Research/Operations
- Guild: Quartermaster + Scout/Smith/Scribe/Healer
- Imperial court: Chancellor + Strategist/Artisan/Archivist/Steward

## Core Components

### 1) Agents

- **Coordinator**: receives requests, classifies them, delegates to specialists, aggregates results.
- **Specialists**: domain-focused agents that execute tasks and publish outputs.

### 2) Sessions & Tools (conceptual)

OpenClaw commonly uses two inter-agent communication modes:

- **Spawn (async delegation)**: send a task to a specialist, continue immediately; specialist completes and posts a callback/announcement.
- **Send (sync messaging)**: send a message that expects an immediate response.

Additionally, agents publish outputs to channels using a message-sending tool.

### 3) Routing & Binding

Route inbound messages by:

- **Platform channel** (e.g., “front desk”, “war room”, “engineering room”)
- **Agent bindings** that map (platform, account, peer/channel) → agentId

### 4) Publishing results

Specialists must:

1. Publish the full result to their “department” channel.
2. Optionally send a short summary to the requester.

When multiple bot accounts exist, specialists should publish using their own account identity.

## Protocol (Recommended)

### Task envelope

Forwarded tasks should include a structured header so specialists can reliably parse context:

```
[FORWARDED TASK]
From: <Coordinator>
Requester: <user handle / id>
Context: <where it came from>
Task: <verbatim user request>

Constraints:
- publish to: <target channel>
- also notify coordinator
```

### Source priority for specialists

Specialists should prioritize:
1. Forwarded tasks from the coordinator
2. Direct user questions in their own channel
3. Cross-team meeting room mentions

### Completion contract

Specialists should return:
- A one-line summary
- Where the full result was published (channel + message link/id if available)
- Any artifacts produced (files/links)

## Configuration outcomes (Success criteria)

You know the system works when:

1. A request in the coordinator channel triggers delegation.
2. The specialist publishes in the right specialist channel.
3. Messages appear under the correct bot identity (if multi-account is used).
4. The coordinator still receives a completion signal (even if direct reply is enabled).

## Files in this template

- `docs/CONFIGURATION_FLOW.md`
- `docs/AGENT_COLLABORATION_PROTOCOL.md`
- `templates/openclaw.json`
- `templates/workspaces/**/AGENTS.md`
