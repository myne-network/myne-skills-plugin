---
name: myne-automations
description: "Use when creating or updating inactive automation drafts via User MCP. Requires user.draft scope and automations edit permission."
---

# myne automations

Draft automations on the bound brand — inactive only.

## Tool

| Tool | When |
| --- | --- |
| `automations_draft_upsert` | Create (`name` required) or update (`automation_id`) an **inactive** automation |

Requires OAuth `user.draft` and brand permission automations / edit.

## Rules

- Always pass `is_enabled: false` semantics — the dispatcher enforces inactive state.
- Never enable, schedule, or run automations from MCP.
- Use returned `in_app_url` for flow review in myne.
- Optional `client_request_id` dedupes retries within one hour.
