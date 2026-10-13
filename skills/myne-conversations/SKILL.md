---
name: myne-conversations
description: "Use when listing recent conversation threads via User MCP, drafting a reply, or sending a drafted reply. Names and ids only on list — no email, phone, or message bodies on list. Email drafts need a staff-confirmed subject before send."
---

# myne conversations

Recent inbox threads, draft reply previews, and draft sends on the bound brand.

## When to use

- “What conversations are recent?”
- Preview a draft reply before the user sends in-app
- Draft and send an inbox reply when the user asks

Establish session context first if brand binding is unclear.

## Tools

| Tool | When | Scope |
| --- | --- | --- |
| `conversations_list_recent` | Recent threads (names + ids; `message_id` is UUID string) | `user.read` + conversations view |
| `messages_preview` | Draft reply preview (no send) | `user.draft` + conversations edit |
| `messages_draft_upsert` | Create/update inbox draft reply (no send) | `user.draft` + conversations edit |
| `messages_send` | Queue a persisted draft for live send (needs `draft_id`) | `user.send` + conversations edit |

## Rules

- List returns **names and ids only** — never email, phone, addresses, or message bodies.
- `type` is the message type code (e.g. SMS/EMAIL) and `type_label` is its display name — write `type_label` to the user, not the code; `channel` is the inbox channel (e.g. personal/shared), not a duplicate of `type`.
- `message_id` is the latest message UUID in the thread (string), not a numeric id.
- Do not invent contact details from names.
- `messages_preview` does **not** send; only `messages_send` queues live contact, and it requires `user.send` plus a draft created by `messages_draft_upsert`.
- `messages_draft_upsert` persists a **DRAFT** reply only — optional `messages_preview` before upsert; include `client_request_id` on every write.
- **One draft per customer per channel** (SMS/EMAIL/INSTAGRAM_DM), not per thread — same as in-app inbox. `thread_id` resolves customer + channel; drafting in another thread for the same customer/channel overwrites the existing draft.
- Use thread ids from list results; do not invent `thread_id` values.

## Email subject is a staff decision

For email drafts:

1. If the user gave a subject, use it. If not, **suggest a short subject** and stop — ask accept / change / cancel before writing the draft.
2. Call `messages_draft_upsert` with both `body` and the confirmed `subject`.
3. **Never** copy the body into `subject` on your own, and never leave `subject` empty hoping send will fix it.
4. Do **not** call `messages_send` while the draft preview shows `subject: null` — complete step 1–2 first.
