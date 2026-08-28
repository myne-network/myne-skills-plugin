---
name: myne-drafts
description: "Use when creating draft/inactive records via User MCP write tools. Covers promotions, journeys, campaigns, and automations. Requires user.draft scope and MCP elicitation."
---

# myne drafts

Create or update **draft/inactive** records only.

## Write tools

| Tool | Permission | Notes |
| --- | --- | --- |
| `automations_draft_upsert` | automations / edit | Inactive automation |
| `campaigns_draft_upsert` | campaigns / edit | Draft campaign (conversation send) |
| `promotions_draft_upsert` | brand_settings / edit | `active: false` only |
| `journeys_draft_upsert` | journeys / edit | Journey definition, no enrolment |

Preview tools (`campaigns_preview_*`, `messages_preview`) also require `user.draft` where noted.

## Requirements

1. OAuth scope **`user.draft`** on the grant (opt-in at consent).
2. Host **MCP elicitation** — writes fail closed without it (no `confirmed=true`).
3. Feature **edit** permission for the domain.
4. Pass **`client_request_id`** on every draft write — retries with the same id return the stored response for one hour (Postgres-backed, cross Lambda instance).

## Automation / campaign drafts

- **Create** requires at least one **`step`**; **`triggers`** optional for automations (campaign drafts get a default conversation-send trigger when omitted).
- **Update** with only `name` / `description` / `instructions` leaves the existing trigger/step graph unchanged; include `triggers` / `steps` to replace them.
- Writes go through automations-microservice validation — invalid graphs return 400, never silent drop.

## Response shape

Draft tools return:

```json
{ "draft_id": "...", "status": "draft|inactive", "preview": {}, "in_app_url": "..." }
```

Repeat in responses and skills copy: **nothing is sent, scheduled, or activated from AI.**
