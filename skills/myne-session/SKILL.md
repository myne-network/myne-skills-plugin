---
name: myne-session
description: "Use when starting myne work, checking who is authenticated, or confirming which brand the User MCP session is bound to. Covers user_me and brand_summary before customer or other brand-scoped tools."
---

# myne session

Establish session context before other myne User MCP tools.

## When to use

- First turn after connecting User MCP
- User asks who they are, which brand is connected, or whether auth worked
- Before customer lookups when brand context is unclear

## Tools

| Tool | Purpose |
| --- | --- |
| `user_me` | Staff identity and brand binding for the connected session |
| `brand_summary` | Safe brand metadata (name, timezone) |

Both require OAuth scope `user.read`. No arguments.

## Workflow

1. Call `user_me` to confirm the authenticated staff user and brand binding.
2. Call `brand_summary` when you need brand name or timezone for the reply.
3. Do **not** invent brand id, brand name, staff identity, or permissions.
4. If auth fails or tools are missing, tell the user to reconnect User MCP in their host (plugin / MCP settings) and complete OAuth — do not guess.

## Response habits

- Lead with the bound brand and staff identity when the user asks “who am I / which brand?”
- Keep replies short; these tools are grounding, not a full CRM dump.
- If the session has no usable brand binding, stop and say so before calling customer tools.
