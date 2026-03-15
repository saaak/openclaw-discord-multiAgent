# OpenClaw Setup Prompt (Single Source of Truth)

Use this document as the **only authoritative instruction set** for LLM-assisted setup.

## Goal

Collect required user configuration safely and update `openclaw.json` with minimal, additive changes.

## Required Reading Order

1. OpenClaw official config reference:
   - https://docs.openclaw.ai/gateway/configuration-reference
2. Project setup guide:
   - `docs/CONFIGURATION_FLOW.md`
3. Collaboration protocol:
   - `docs/AGENT_COLLABORATION_PROTOCOL.md`

## Execution Rules

1. **Read existing config first**
   - Load the user's current `openclaw.json` before proposing changes.

2. **Never invent values**
   - Do not hallucinate IDs, API endpoints, model names, channels, or credentials.
   - If required values are missing, ask the user.

3. **Collect missing fields explicitly**
   - If a form tool is available, provide a form.
   - If no form tool is available, provide a clear required-fields checklist.

4. **Handle existing settings carefully**
   - For existing LLM model, Discord channel, workspace, and similar settings, ask whether to reuse existing values or create new ones.
   - If `llmProvider` is already configured, do **not** ask the user to re-enter provider credentials/config by default; ask whether to reuse the existing `llmProvider` config.

5. **Update strategy: additive first**
   - After all required values are confirmed, directly edit `openclaw.json`.
   - Prefer adding new blocks/entries over modifying existing ones.
   - Do not overwrite or delete unrelated existing settings.

6. **Missing-value fallback**
   - If any required field still cannot be confirmed, keep `<REQUIRED_VALUE>` placeholders and list missing items explicitly.

7. **Security constraints**
   - Never commit secrets.
   - Never output real credentials in logs/examples if redaction is possible.

## Output Requirements

After updating config, provide:

1. A short summary of what was changed in `openclaw.json`
2. A list of fields that still require user input (if any)
3. A brief validation checklist for routing and specialist publishing
