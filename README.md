# Notion MCP Actor

**47 tools for AI agents that manage Notion workspaces.** Wraps the `notion-pp-mcp` binary as an Apify Standby MCP actor with PPE billing.

## What It Does

Notion MCP wraps the [Notion Printing Press CLI](https://github.com/mvanhorn/printing-press-library) as an MCP server, giving AI agents full access to:

- **Pages** - Create, read, update, move, archive pages; retrieve page properties
- **Databases** - Query and create databases, tables, and datasets
- **Blocks** - Manage block children, update, delete, append content
- **Comments** - Create, list, update, delete comments on pages and blocks
- **Users** - Get user info, workspace users, self info
- **Views** - Create and manage views, run view queries
- **Search** - Search pages and databases by title
- **OAuth** - Manage OAuth tokens for third-party integrations
- **File Uploads** - Upload and manage files in Notion
- **Workflows** - Archive pages and update workflow status
- **Analytics** - Analyze locally synced data with group-by and summary
- **SQL** - Query synced Notion data with SQL

## Why Use It

- **AI Agent Integration** - Standard MCP protocol, works with any MCP client (Claude, Cursor, etc.)
- **Offline SQLite Store** - Syncs Notion data locally for fast querying without API rate limits
- **PPE Billing** - Pay per tool call via Apify PPE; every call $0.03, search and create tools $0.05 (see Pricing)
- **47 Tools** - Full Notion API coverage via well-designed CLI interface

## Quick Start

### 1. Get an Apify Actor

Run it on the Apify Store: https://apify.com/red.cars/notion-mcp

To pull the source locally:

```bash
apify pull FwmemTEAo83gbAj9N
```

### 2. Set Environment Variable

Set `NOTION_API_TOKEN` in your Apify actor settings or `.env`:

```
NOTION_API_TOKEN=secret_your_notion_integration_token
```

### 3. Configure MCP Client

Start a standby run of the actor, then point your MCP client at the actor's standby URL (shown on the run page while the run is live). In Claude Code, Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "notion": {
      "url": "<standby URL from the run page>"
    }
  }
}
```

Or use the Apify MCP gateway, which starts the actor for you:

```json
{
  "mcpServers": {
    "apify-actors": {
      "command": "npx",
      "args": ["-y", "@apify/actors-mcp-server", "--actors=FwmemTEAo83gbAj9N", "--token=$APIFY_TOKEN"]
    }
  }
}
```

## MCP Tools Reference

| Category | Tool | Description | PPE Price |
|----------|------|-------------|-----------|
| **Pages** | `pages_retrieve_a` | Retrieve a page by ID | $0.03 |
| | `pages_patch` | $0.03 |
| | `pages_post` | $0.05 |
| | `pages_move_page` | $0.03 |
| | `pages_markdown_retrieve_page` | $0.03 |
| | `pages_markdown_update_page` | $0.03 |
| | `pages_properties_retrieve_a_page_property` | $0.03 |
| **Databases** | `databases_retrieve` | Get database metadata | $0.03 |
| | `databases_query` | $0.03 |
| | `databases_create` | $0.05 |
| | `databases_update` | $0.03 |
| **Blocks** | `blocks_retrieve_a` | Get a block by ID | $0.03 |
| | `blocks_children_get_block` | $0.03 |
| | `blocks_update_a` | $0.03 |
| | `blocks_children_patch_block` | $0.03 |
| | `blocks_delete_a` | $0.03 |
| **Comments** | `comments_list` | List comments | $0.03 |
| | `comments_retrieve` | $0.03 |
| | `comments_create_a` | $0.05 |
| | `comments_update_a` | $0.03 |
| | `comments_delete_a` | $0.03 |
| **Users** | `users_get` | Get user by ID | $0.03 |
| | `users_get_self` | $0.03 |
| | `users_get_userid` | $0.03 |
| **Search** | `search` | Search pages and databases | $0.03 |
| | `notion_search_post` | $0.05 |
| **Views** | `views_list` | List views in a database | $0.03 |
| | `views_retrieve_a` | $0.03 |
| | `views_queries_get_view_query_results` | $0.03 |
| | `views_create` | $0.05 |
| | `views_update_a` | $0.03 |
| | `views_queries_create_view_query` | $0.05 |
| | `views_queries_delete_view_query` | $0.03 |
| **Sync** | `sync` | Sync data from Notion to local SQLite | $0.03 |
| | `sync_pages` | $0.03 |
| | `changed` | $0.03 |
| | `stale` | $0.03 |
| **Analytics** | `analytics` | Analyze synced data | $0.03 |
| **SQL** | `sql` | Query synced data with SQL | $0.03 |
| **Data Sources** | `data_sources_query_post_database` | Query a data source | $0.03 |
| | `data_sources_retrieve_a` | $0.03 |
| | `data_sources_create_a_database` | $0.05 |
| | `data_sources_update_a` | $0.03 |
| **File Uploads** | `file_uploads_list` | List file uploads | $0.03 |
| | `file_uploads_retrieve` | $0.05 |
| | `file_uploads_create_file` | $0.05 |
| | `file_uploads_send_upload_file` | $0.05 |
| | `file_uploads_complete_file_upload` | $0.05 |
| **OAuth** | `oauth_create_a_token` | $0.05 |
| | `oauth_introspect_token` | $0.03 |
| | `oauth_revoke_token` | $0.03 |
| **Workflows** | `workflow_archive` | $0.03 |
| | `workflow_status` | $0.03 |
| **Other** | `import` | $0.05 |
| | `custom_emojis_list` | $0.03 |
| | `blocks_query_meeting_notes` | $0.03 |

## Pricing

Pay per tool call through Apify PPE. One call bills one event from the store listing:

| Event | Price | Tools that bill it |
|---|---|---|
| Search Database | $0.05 | `search`, `notion_search_post`, and any tool with "search" in its name |
| Create Page | $0.05 | Creates, imports, uploads and appends (`pages_post`, `databases_create`, `views_create`, `comments_create_a`, `import`, file uploads, block appends) |
| List Databases | $0.03 | List tools (`comments_list`, `views_list`, `file_uploads_list`, `custom_emojis_list`) |
| Query Page | $0.03 | Query tools (`databases_query`, `data_sources_query_post_database`, view queries) |
| Update Page | $0.03 | Updates, patches and moves (`pages_patch`, `pages_move_page`, `databases_update`, `blocks_update_a`) |
| Archive Page | $0.03 | Deletes, archives and revokes (`blocks_delete_a`, `comments_delete_a`, `workflow_archive`, `oauth_revoke_token`) |
| Get Page | $0.03 | Every other read tool (retrieves, `sync`, `changed`, `stale`, `analytics`, `sql`) |

The charge happens when the call starts, so a failed call still bills. `tools/list` (discovery) is free.

## FAQ

**What is Notion MCP?**
An MCP server that runs on Apify and gives AI agents 47 tools for managing a Notion workspace: pages, databases, blocks, comments, users, views, file uploads, OAuth, plus an offline SQLite store with SQL and analytics.

**Do I need a Notion account or API key?**
You need a Notion integration token. Create an internal integration at notion.so/my-integrations, connect it to your workspace and to the pages or databases you want it to reach, then set `NOTION_API_TOKEN` (a `secret_...` value) as an actor environment variable. No Notion paid plan is required for API access.

**How much does a call cost?**
Most tools bill $0.03 per call. Search tools and create tools bill $0.05 per call. See the Pricing table above. Listing tools (`tools/list`) is free.

**Does a failed call still bill?**
Yes. The charge happens when the call starts, before the tool runs. A failed call still bills one event, and a retry bills again.

**Which MCP clients can use it?**
Any MCP client that can make HTTP calls to an MCP endpoint works: Claude Code, Cursor, and other MCP-compatible agents. The Quick Start shows both the direct HTTP config and the Apify MCP gateway.

**Can it write to my workspace, not just read?**
Yes. It creates, updates, moves and archives pages, updates databases, appends and edits blocks, creates and updates comments, manages file uploads and OAuth tokens, and creates views.

**What is the offline SQLite store?**
Run `sync` once and your workspace data lands in a local SQLite store inside the actor. `sql`, `analytics`, `changed` and `stale` then work offline, with no Notion API rate limits. Re-run `sync` to refresh.

**How do I find pages and databases?**
`search` finds pages and databases by title, `list_databases` lists your databases, and `databases_query` queries one database with filters and sorts.

**How do I get my results?**
MCP calls return results in the response itself. Direct invocation also pushes every result to the run's default dataset, so you can export JSON, CSV or Excel from the run page.

**Is there a free way to try it?**
Yes. Open the actor on the Apify Store and use the input field for a single direct tool call, or call `tools/list` over MCP to see the full tool list for free.

## Direct API Invocation

You can also call tools directly via HTTP without MCP:

```bash
curl -X POST "https://api.apify.com/v2/acts/FwmemTEAo83gbAj9N/runs" \
  -H "Authorization: Bearer $APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{ "tool": "search", "args": { "query": "roadmap" } }'
```

The run runs one tool call, bills its event, pushes the result to the run's dataset, and exits.bash
# Install dependencies
npm install

# Build TypeScript
npm run build

# Run locally (requires Apify SDK)
node dist/main.js

# Test with MCP protocol
echo '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{}}' | notion-pp-mcp
```

## GitHub Topics

`notion` `mcp` `mcp-server` `notion-api` `ai-agents`

## Support

- **Issues**: https://github.com/wdavalos/notion-mcp/issues
- **Apify**: https://apify.com/red.cars/notion-mcp
- **Notion PP CLI**: https://github.com/mvanhorn/printing-press-library