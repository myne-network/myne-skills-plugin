---
name: myne-campaigns
description: "Use when listing or inspecting conversation campaigns via User MCP. Covers campaigns_list, campaigns_get, audience/content previews, and draft upserts. Masked stats only — no recipient export."
---

# myne campaigns

Browse and draft conversation campaigns on the bound brand.

## When to use

- “List recent campaigns” or “show campaign stats”
- Preview audience size or template content before editing in-app
- Create/update a **draft** campaign from AI (never send or schedule live)

Establish session context first if brand binding is unclear.

## Tools

| Tool | When | Scope |
| --- | --- | --- |
| `campaigns_list` | Paginated summaries (status, counts, dates) | `user.read` + campaigns view |
| `campaigns_get` | One campaign by `automation_id` — stats only | `user.read` + campaigns view |
| `campaigns_preview_audience` | Count + masked sample (max 10) | `user.read` + campaigns view |
| `campaigns_preview_content` | Render preview for a step | `user.draft` + campaigns edit |
| `campaigns_draft_upsert` | Create/update inactive draft | `user.draft` + campaigns edit |

## Rules

- Recipient **lists are never exported** — only counts and masked samples.
- Draft tools never set scheduled/sent/active status; activation is **in-app only**.
- Use `in_app_url` from draft/preview responses to hand off for review.
- Do not invent `automation_id` values.
