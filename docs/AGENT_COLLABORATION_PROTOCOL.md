# Agent Collaboration Protocol (generalized)

This document describes the collaboration mechanics extracted from a working OpenClaw multi-agent setup and generalized so it can be re-themed into many scenarios.

## Roles

### Coordinator

Responsibilities:

- Triage tasks quickly.
- Choose the right specialist.
- Delegate work using **spawn** (preferred) or **send**.
- Ensure outputs are published and discoverable.

Non-responsibilities:

- The coordinator should not “pretend to have delegated”. If specialist work is required, it must actually delegate.

### Specialists

Responsibilities:

- Execute the delegated task end-to-end.
- Publish the full result to the specialist channel.
- Provide a concise summary for the requester.
- When possible, include reproducible steps, artifacts, and links.

## Communication modes

### Spawn (async delegation)

Use when:
- The task is non-trivial.
- The specialist needs time and tool usage.
- You want the system to continue operating while the specialist works.

Coordinator sends:
- `agentId`
- `task` (with envelope)
- timeout
- callback options (optional)

Specialist does:
- publish to specialist channel
- notify coordinator (and optionally the requester)

### Send (sync messaging)

Use when:
- You need a quick answer, confirmation, or a small piece of information.
- You do not need background execution.

## Recommended task envelope

```
[FORWARDED TASK]
From: coordinator
Requester: <id/handle>
Origin: <platform + channel>

Task:
<the user request>

Deliverables:
- Publish full output to: <specialist channel>
- Provide summary: <one paragraph>

Constraints:
- Keep outputs structured
- Include assumptions explicitly
```

## Publishing outputs

### Where

- Specialists publish full outputs in their own “department” channel.
- Coordinator can optionally cross-link a short summary to the intake channel.

### Under which identity

If running multiple bot accounts:

- Specialists should send messages using their own `accountId` so the message author clearly indicates the specialist agent.

## Direct reply vs coordinator relay

Two patterns:

1. **Direct reply (preferred for UX)**
   - Specialist replies to the requester directly (where the request came from).
   - Coordinator still receives a completion signal for tracking.

2. **Coordinator relay (compatibility / strict control)**
   - Specialist posts only to specialist channel.
   - Coordinator relays the summary back to requester.

Choose per deployment based on moderation/security needs.

## Reliability recommendations

These are recommended operational patterns (implementation may vary):

- Include a taskId in forwarded tasks.
- Include a completion status: success / failure / timeout.
- For failures, return a structured error with next steps.
- If callbacks can be lost, implement retry + dead-letter handling.
