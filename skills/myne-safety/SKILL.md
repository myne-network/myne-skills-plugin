---
name: myne-safety
description: "Use for any myne User MCP work involving customer or brand data. Read-only default, no invented PII, respect scopes, and only claim write/draft capabilities when matching tools and user.draft are actually available."
---

# myne safety

Guardrails for brand staff using myne User MCP from an MCP host.

## Defaults

- Prefer **read** tools (`user.read`): `user_me`, `brand_summary`, `customers_list_recent`, `customers_search`.
- Never invent customers, contacts, brand metadata, or permissions.
- Never paste access tokens, refresh tokens, or OAuth secrets into chat or files.
- Masked email/phone from tools stay masked in your reasoning and replies unless the user already provided a fuller value themselves.

## Scopes

| Scope | Meaning |
| --- | --- |
| `user.read` | Identity, brand summary, customer list/search |
| `user.draft` | Reserved for future draft/write tools |

If a write or draft tool is **not** in the live tool list, do not claim you can create or mutate CRM entities via User MCP. Say what is available instead.

When draft tools exist and the session has `user.draft`, follow those tool schemas exactly and confirm destructive or customer-visible actions with the user before calling them.

## What this plugin is not

- Not the in-app HuckAI / Analyst skill library
- Not internal myne eng MCP (`mcp-tools`, `mcp-internal-tools`, analyst SQL)
- Not a substitute for the myne web app for full CRM editing

## Failure handling

- Auth / 401 → ask the user to reconnect the myne plugin MCP and complete OAuth
- 403 / forbidden → say the session lacks permission; do not retry with guessed args
- Empty search → report empty; offer a different id or recent list
- Unknown tool name → check `tools/list`; do not invent tool names
