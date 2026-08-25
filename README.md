# myne Skills Plugin

Cursor plugin that connects to the public myne **User MCP** and ships skills for brand staff workflows.

## What you get

- **MCP:** `https://mcp.myne.network/mcp` (OAuth Bearer for brand staff)
- **Skills:** session grounding, customer list/search, safety defaults

This plugin does **not** include the in-app HuckAI/Analyst skill library. Skills here only teach agents how to use User MCP tools from Cursor.

## Install (Cursor)

### Local development

```bash
mkdir -p ~/.cursor/plugins/local
ln -sfn /path/to/myne-skills-plugin ~/.cursor/plugins/local/myne
```

Reload Cursor (**Developer: Reload Window**). Open **Customize** and confirm the `myne` plugin loaded. Connect the MCP server and complete OAuth on first use.

### Marketplace (when published)

```text
/add-plugin myne
```

Or install from [cursor.com/marketplace](https://cursor.com/marketplace).

## Manual MCP (without plugin)

Equivalent to this plugin’s `mcp.json`:

```json
{
  "mcpServers": {
    "myne": {
      "url": "https://mcp.myne.network/mcp"
    }
  }
}
```

## Skills

| Skill | Purpose |
| --- | --- |
| `myne-session` | `user_me`, `brand_summary` — identity and brand binding |
| `myne-customers` | `customers_list_recent`, `customers_search` |
| `myne-safety` | Read-only default, masking, scopes, no invented PII |

## Related services

| Repo | Role |
| --- | --- |
| [mcp-user-tools](https://github.com/myne-network/mcp-user-tools) | Hosted User MCP server |
| This repo | Cursor packaging (manifest, MCP config, skills) |

## Publish checklist

1. Verify OAuth + tools against prod User MCP from a Cursor install of this plugin
2. Submit at [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish)
3. Bump `version` in `.cursor-plugin/plugin.json` for each marketplace update

## License

MIT
