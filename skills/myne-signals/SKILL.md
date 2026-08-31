---
name: myne-signals
description: "Use when listing Signals dashboard pages via User MCP. Returns id and name only."
---

# myne signals

List saved Signals dashboard pages for the bound brand.

## When to use

- “What dashboards exist in Signals?”

Establish session context first if brand binding is unclear.

## Tools

| Tool | When | Scope |
| --- | --- | --- |
| `signals_dashboards_list` | Dashboard id + name | `user.read` + signals view |

## Rules

- Responses are **id and name only** — no widgets, queries, or user ids.
- Do not invent dashboard ids.
- Opening or editing dashboards happens in-app.
