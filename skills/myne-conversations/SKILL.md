---
name: myne-conversations
description: "Use when listing recent conversation threads via User MCP or previewing a draft reply. Names and ids only — no email, phone, or message bodies on list. Does not send."
---

# myne conversations

Recent inbox threads and draft reply previews on the bound brand.

## When to use

- “What conversations are recent?”
- Preview a draft reply before the user sends in-app

Establish session context first if brand binding is unclear.

## Tools

| Tool | When | Scope |
| --- | --- | --- |
| `conversations_list_recent` | Recent threads (names + ids; `message_id` is UUID string) | `user.read` + conversations view |
| `messages_preview` | Draft reply preview (no send) | `user.draft` + conversations edit |

## Rules

- List returns **names and ids only** — never email, phone, addresses, or message bodies.
- `type` is the message type (e.g. SMS/EMAIL); `channel` is the inbox channel (e.g. personal/shared), not a duplicate of `type`.
- `message_id` is the latest message UUID in the thread (string), not a numeric id.
- Do not invent contact details from names.
- `messages_preview` does **not** send; live send is not available on User MCP in this phase.
- Use thread ids from list results; do not invent `thread_id` values.
