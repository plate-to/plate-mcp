# Plate MCP Server

[Plate](https://plate.to) is a lightweight project and task manager built for automation. Plan work, track tasks, and keep projects moving forward with people and AI agents working side by side. This MCP server lets you manage workspaces, projects, and tasks directly from Claude, Cursor, Windsurf, and any MCP-compatible AI client.

**Endpoint:** `https://plate.to/mcp`

---

## Setup

Add this to your MCP configuration:

```json
{
  "mcpServers": {
    "plate": {
      "type": "http",
      "url": "https://plate.to/mcp"
    }
  }
}
```

| Client | Config location |
|--------|----------------|
| Claude Code | `.mcp.json` in project root, or `~/.claude/.mcp.json` globally |
| Claude Desktop | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Cursor | Settings → Cursor Settings → MCP |
| Windsurf | Windsurf Settings → MCP Servers |

After adding the server, your client will open a browser window to authorize via your Plate account. No API key setup required — OAuth 2.0 with PKCE handles everything automatically.

---

## Tools

All tools return structured output (JSON Schema defined via `outputSchema`), compatible with OpenAI, ChatGPT Desktop, and any client that supports the MCP structured content spec.

### Read tools (always available)

| Tool | Description |
|------|-------------|
| `list_workspaces` | List all workspaces you belong to |
| `list_projects` | List projects in a workspace |
| `list_sections` | List sections in a project |
| `list_tasks` | List tasks in a project (up to 100 by default, configurable with `limit`), or find by public number (`workspaceId + number`) |
| `get_task` | Get full task details — accepts internal ID, prefixed ref (`SCD-426`), or `workspaceId + number` |
| `list_statuses` | List workflow statuses for a workspace |
| `list_members` | List workspace members |

### Write tools (require `write` scope)

| Tool | Description |
|------|-------------|
| `create_task` | Create a task in a project section (use `update_task` for existing tasks) |
| `update_task` | Update task name, status, assignee, section, or description |
| `complete_task` | Mark a task as done or reopen it |
| `delete_task` | Permanently delete a task |
| `create_project` | Create a new project — returns existing project if name already exists (`created: false`) |
| `update_project` | Rename a project or update its description |
| `create_section` | Add a section to a project — returns existing section if name already exists (`created: false`) |
| `update_section` | Rename a section |
| `create_comment` | Post a comment on a task |
| `delete_comment` | Delete a comment |

---

## Authentication

Uses **OAuth 2.0 with PKCE**. On first use, your client opens a browser to sign into your Plate account. You choose a scope (`read` or `read write`) and approve access. Tokens refresh automatically — you won't need to re-authorize unless the refresh token expires (30 days).

To revoke access: **Workspace Settings → Apps** in Plate.

---

## Documentation

Full tool reference and parameter details: [plate.to/docs/mcp](https://plate.to/docs/mcp)

Questions: [hello@plate.to](mailto:hello@plate.to)
