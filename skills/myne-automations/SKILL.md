---
name: myne-automations
description: "Use when listing, inspecting, or drafting inactive automations via User MCP. Covers automations_list, automations_get, and automations_draft_upsert. Conversation campaigns use myne-campaigns instead."
---

# myne automations

Browse and draft **non-campaign** automations on the bound brand.

## When to use

- “List automations” or “is this automation enabled?”
- Inspect trigger/step structure before editing in-app
- Create/update an **inactive** automation draft (never enable or run)

Use **myne-campaigns** for conversation-send campaigns (`campaigns_*` tools).

Establish session context first if brand binding is unclear.

## Tools

| Tool | When | Scope |
| --- | --- | --- |
| `automations_list` | Paginated summaries (name, enabled, dates) | `user.read` + automations view |
| `automations_get` | Structure summary by `automation_id` | `user.read` + automations view |
| `automations_draft_upsert` | Create/update inactive draft | `user.draft` + automations edit |

## Rules

- List/get **exclude** conversation campaigns — those stay under `campaigns_*`.
- Get returns trigger types and step types only — no `action_data` or recipient payloads.
- Draft tools never enable, schedule, or run automations; activation is **in-app only**.
- Use `in_app_url` from draft responses for flow review.
- Do not invent `automation_id` values.
- Optional `client_request_id` dedupes retries within one hour.
