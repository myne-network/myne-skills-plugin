---
name: myne-safety
description: "Use for any myne User MCP work involving customer or brand data. Read-only default, no invented PII, respect scopes, and only claim write/draft/send capabilities when matching tools and scopes are actually available."
---

# myne safety

Guardrails for brand staff using myne User MCP from an MCP host.

## Defaults

- Prefer **read** tools (`user.read`) unless the user explicitly asks to draft or send something.
- Never invent customers, contacts, brand metadata, or permissions.
- Never paste access tokens, refresh tokens, or OAuth secrets into chat or files.
- Masked email/phone from tools stay masked in your reasoning and replies unless the user already provided a fuller value themselves.

## Scopes

| Scope | Meaning |
| --- | --- |
| `user.read` | Identity, brand summary, customers, campaigns, automations, journeys, conversations, signals reads; audience previews |
| `user.draft` | Draft/inactive writes and content/message previews that mutate nothing live |
| `user.send` | Live inbox message send (`messages_send`) — queues real outbound contact |

If a write, draft, or send tool is **not** in the live tool list, do not claim you can create, mutate, or send via User MCP. Say what is available instead.

When draft tools exist and the session has `user.draft`:

- OAuth **`user.draft`** is opt-in at consent — upgrading from read-only requires explicit re-consent in `/mcp/authorize` (step-up), not a silent scope expansion.
- Draft tools create **inactive/draft** records only; send, schedule, and activate happen **in-app** unless `user.send` is also granted.
- **Interim:** host MCP elicitation is **not** required for `user.draft` tools (draft upserts and preview tools) on stateless clients such as Cursor — OAuth scope + permissions + `client_request_id` still apply. Never pass `confirmed=true`.

When `messages_send` exists and the session has `user.send`:

- OAuth **`user.send`** is a separate opt-in — never assume it from `user.draft` alone.
- Call **`messages_draft_upsert` first** to persist a draft and obtain `draft_id`; preview-only replies cannot be sent.
- MCP **elicitation** uses live-send copy — confirm the user understands real outbound contact is queued.
- After send, use `in_app_url` for inbox handoff; do not claim delivery until the app shows sent/queued status.

## What this plugin is not

- Not the in-app HuckAI / Analyst skill library
- Not internal myne eng MCP (`mcp-tools`, `mcp-internal-tools`, analyst SQL)
- Not a substitute for the myne web app for full CRM editing, campaign send, or automation activation

## Failure handling

- Auth / 401 → ask the user to reconnect the myne plugin MCP and complete OAuth
- 403 / forbidden → say the session lacks permission; do not retry with guessed args
- Elicitation required ( **`messages_send` only** during interim) → explain the client must support MCP elicitation for live send, or complete the action in-app via `in_app_url`
- Empty search → report empty; offer a different id or recent list
- Unknown tool name → check `tools/list`; do not invent tool names
