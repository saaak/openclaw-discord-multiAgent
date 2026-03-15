# OpenClaw Setup Prompt (Single Source of Truth)

Use this document as the **only authoritative instruction set** for LLM-assisted setup.

## Goal

Collect required user configuration safely, confirm agent role relationship semantics, and update the **user's runtime `openclaw.json`** with minimal, additive changes.

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

3. **Confirm role relationship contract before configuration edits**
   - Before creating/updating any agent/workspace entries, ask the user what relationship style should define dispatcher vs worker agents.
   - Ask explicitly for semantic style (examples: modern company, guild, imperial court, or user-defined style).
   - Ask for naming mapping under that style (e.g., `coordinator`/`specialist-*` equivalents) and confirm who dispatches tasks vs who executes tasks.

4. **Collect missing fields explicitly**
   - If a form tool is available, provide a form.
   - If no form tool is available, provide a clear required-fields checklist.

5. **Handle existing settings carefully**
   - For existing LLM model, Discord channel, and similar settings, ask whether to reuse existing values or create new ones.
   - If `llmProvider` is already configured, do **not** ask the user to re-enter provider credentials/config by default; ask whether to reuse the existing `llmProvider` config.

6. **Target the correct config file**
   - Edit the user's real runtime config file (`openclaw.json`) in their environment.
   - Do **not** treat `templates/openclaw.json` as the runtime file; it is a reference template only.

7. **Workspace policy: create new, do not mutate existing**
   - After role relationship + required config are confirmed, create a **new workspace set** for the new role mapping.
   - Do **not** modify existing workspace folders for this new setup.
   - Bind the new roles in config via new `workspace` paths and corresponding routing/binding entries.

8. **Update strategy: additive first**
   - After all required values are confirmed, directly edit the user's runtime `openclaw.json`.
   - Prefer adding new blocks/entries over modifying existing ones.
   - Do not overwrite or delete unrelated existing settings.

9. **Missing-value fallback**
   - If any required field still cannot be confirmed, keep `<REQUIRED_VALUE>` placeholders and list missing items explicitly.

10. **Security constraints**
   - Never commit secrets.
   - Never output real credentials in logs/examples if redaction is possible.

## Output Requirements

After updating config, provide:

1. A short summary of what was changed in the user's runtime `openclaw.json`
2. A list of fields that still require user input (if any)
3. The confirmed role relationship contract (style + role mapping)
4. A brief validation checklist for routing and specialist publishing
