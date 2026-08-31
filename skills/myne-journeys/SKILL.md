---
name: myne-journeys
description: "Use when listing or inspecting journey definitions via User MCP, or drafting journey definitions. Covers journeys_list, journeys_get, and journeys_draft_upsert. No enrollee PII."
---

# myne journeys

Browse and draft journey definitions on the bound brand.

## When to use

- “What journeys exist?” or “show stages for this journey”
- Create/update a journey definition from AI (does not enrol customers)

Establish session context first if brand binding is unclear.

## Tools

| Tool | When | Scope |
| --- | --- | --- |
| `journeys_list` | Paginated summaries (name, counts) | `user.read` + journeys view |
| `journeys_get` | Definition + stages by `journey_id` | `user.read` + journeys view |
| `journeys_draft_upsert` | Create/update definition | `user.draft` + journeys edit |

## Rules

- Never return or request enrollee / instance PII — counts only on list.
- Get includes stages (`id`, `label`, `sort_order`, `is_terminal`, `color`) — not board cards.
- Draft upsert does **not** enrol customers; enrolment stays in-app.
- Do not invent `journey_id` values.
