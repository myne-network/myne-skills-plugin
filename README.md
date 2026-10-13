# myne Skills Plugin

Cursor and Claude Code plugin that connects to the public myne **User MCP** and ships skills for brand staff workflows.

## What you get

- **MCP:** `https://mcp.myne.network/mcp` (OAuth Bearer for brand staff)
- **Skills:** session grounding, customers, campaigns, automations, journeys, conversations, signals, drafts, safety defaults

This plugin does **not** include the in-app HuckAI/Analyst skill library. Skills here only teach agents how to use User MCP tools from an MCP host.

## Install (Cursor)

### Local development

```bash
mkdir -p ~/.cursor/plugins/local
cp -R /path/to/myne-skills-plugin ~/.cursor/plugins/local/myne
```

Cursor rejects symlinks whose target is outside `~/.cursor/plugins/local`. Re-run the `cp` (or use `rsync -a --delete`) after local edits.

Reload Cursor (**Developer: Reload Window**). Open **Customize** and confirm the `myne` plugin loaded. Connect the MCP server and complete OAuth on first use.

### Marketplace (when published)

```text
/add-plugin myne
```

Or install from [cursor.com/marketplace](https://cursor.com/marketplace).

## Install (Claude Code)

### Local development

```bash
claude plugin validate /path/to/myne-skills-plugin
claude --plugin-dir /path/to/myne-skills-plugin
```

Confirm skills under `/myne:…`. Run `/mcp`, connect **myne**, and complete OAuth on first use (User MCP uses dynamic client registration — no static `clientId` in this plugin).

### Marketplace (when published)

Install from the Claude plugin marketplace when a listing is available (command TBD). Until then, use `--plugin-dir` above.

## Manual MCP (without plugin)

Cursor uses `mcp.json`; Claude Code uses `.mcp.json`. Both point at the same endpoint:

```json
{
  "mcpServers": {
    "myne": {
      "url": "https://mcp.myne.network/mcp"
    }
  }
}
```

Claude’s `.mcp.json` also sets `"type": "http"` on the server entry.

## Skills

| Skill | Purpose |
| --- | --- |
| `myne-session` | `user_me`, `brand_summary` — identity and brand binding |
| `myne-customers` | `customers_list_recent`, `customers_search`, `customers_get` |
| `myne-campaigns` | Campaign list/get, previews, draft upsert |
| `myne-automations` | Automation list/get + inactive drafts |
| `myne-journeys` | Journey list/get + draft upsert |
| `myne-conversations` | Recent threads (names/ids), message preview, draft upsert, draft send (email subject needs staff confirm) |
| `myne-signals` | Signals dashboard id + name list |
| `myne-drafts` | All `user.draft` write tools and rules |
| `myne-safety` | Masking, scopes, elicitation, no invented PII |

## Tool matrix (User MCP)

| Tool | Scope | Permission |
| --- | --- | --- |
| `user_me`, `brand_summary` | `user.read` | membership |
| `customers_*` | `user.read` | customers / view |
| `campaigns_list`, `campaigns_get`, `campaigns_preview_audience` | `user.read` | campaigns / view |
| `campaigns_preview_content`, `campaigns_draft_upsert` | `user.draft` | campaigns / edit |
| `automations_list`, `automations_get` | `user.read` | automations / view |
| `automations_draft_upsert` | `user.draft` | automations / edit |
| `journeys_list`, `journeys_get` | `user.read` | journeys / view |
| `journeys_draft_upsert` | `user.draft` | journeys / edit |
| `conversations_list_recent` | `user.read` | conversations / view |
| `messages_preview`, `messages_draft_upsert` | `user.draft` | conversations / edit |
| `messages_send` | `user.send` | conversations / edit |
| `signals_dashboards_list` | `user.read` | signals / view |
| `promotions_draft_upsert` | `user.draft` | brand_settings / edit |

Customer contact fields are **masked** on every egress path. Conversation list tools omit email/phone keys entirely. Draft tools never send, schedule, or activate live work.

## Connected apps (in-product)

Staff can review and revoke User MCP grants at **Settings → Integrations → Connected AI** (`/settings/integration/connected-ai`).

## Related services

| Repo | Role |
| --- | --- |
| [mcp-user-tools](https://github.com/myne-network/mcp-user-tools) | Hosted User MCP server |
| This repo | Cursor + Claude packaging (manifests, MCP config, skills) |

## Publish checklist

1. Verify OAuth + tools against prod User MCP from a Cursor and Claude Code install of this plugin
2. Cursor: submit at [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish)
3. Claude: publish when ready via Claude’s plugin marketplace process
4. Bump `version` in `.cursor-plugin/plugin.json` and `.claude-plugin/plugin.json` for each release

## License

MIT
