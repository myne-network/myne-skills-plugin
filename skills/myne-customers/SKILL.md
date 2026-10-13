---
name: myne-customers
description: "Use when listing or finding customers for the bound myne brand via User MCP, reading a customer's recent activity, or reading the brand's loyalty program configuration. Covers customers_list_recent vs customers_search, customers_get, customers_events, loyalty_program_summary, pagination, limits, and masked contact fields."
---

# myne customers

Look up customers on the brand bound to the User MCP session.

## When to use

- “Show recent customers”
- “Find customer by id / external id”
- “What has this customer been doing lately?” (recent activity)
- “What is the brand's loyalty program?” (provider names, points label, tiers)
- Gathering a small customer set before summarizing activity

Prefer establishing session context (`user_me` / `brand_summary`) if brand binding is unclear.

## Tools

| Tool | When |
| --- | --- |
| `customers_list_recent` | Browse recent customers; optional `limit` (1–50) and `cursor` |
| `customers_search` | Lookup by `customer_ids` and/or `external_ids` (max 25 each) |
| `customers_get` | Single customer by `customer_id` (masked contact) |
| `customers_events` | Recent activity for one customer (`customer_id`; optional `limit` 1–50) — transactions, events, messages as summary rows |
| `loyalty_program_summary` | Brand loyalty config: active points/cashback providers, points label, reward tier names (config only) |

All require OAuth scope `user.read`. Brand is taken from the session — do not invent a brand id.

## Strategy

1. **Recent browse** → `customers_list_recent` with a small `limit` (e.g. 10–20). Paginate with returned `cursor` only when the user wants more.
2. **Known ids** → `customers_search` with `customer_ids` and/or `external_ids`. Do not pass empty arrays; omit unused keys.
3. Do not invent customer ids, emails, phones, or names. If search returns nothing, say so and offer list-recent or a corrected id.

## Field rules

- Contact fields from User MCP are **masked** (email/phone). Treat them as redacted; do not “unmask” or invent full values.
- Safe fields typically include id, names, masked email/phone, `last_activity_at`, `created_at`.
- `customers_events` returns summary rows only (type, timestamp, source, location, amount, event name, direction) — no payment details, addresses, or message bodies. For what a customer said, use the conversations skill's `conversations_get`.
- `loyalty_program_summary` is configuration only (providers, points label, tier names) — never customer balances or credentials.
- Do not claim access to raw contact data, full CRM history, or tools that are not in the live `tools/list` for this session.

## Common pitfalls

- Calling search without any ids
- Assuming list-recent returns the whole database (it is capped and cursor-based)
- Mixing up myne customer id vs POS/external id — use the matching search argument
- Narrating OAuth or masking internals to the user unless they ask
