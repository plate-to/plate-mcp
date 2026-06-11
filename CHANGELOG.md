# Changelog

## 2026-06-11 (latest)

### Added

- **`list_activity`** — task change history over a time range. Answers time-based questions like "which tasks were completed last week" or "what did a teammate do". Each row gives the task, the status transition, who made the change, and the task's owner and assignee. Filters: `from`/`to` (ISO dates), `actorId` (a userId), `type` (`task_status_changed`, or `completed` for transitions into a Done status), and `projectId`.

### Fixed

- Task changes made via the MCP server are now attributed to the signed-in user, so "what did &lt;person&gt; do" includes work done from ChatGPT/Claude/Cursor.

## 2026-06-07

### Added

- **8 new batch tools for bulk operations** — reduces confirmation prompts in ChatGPT and other AI clients that ask once per tool call. Use these instead of calling single-item tools in a loop:
  - `create_tasks` — create up to 50 tasks atomically
  - `update_tasks` — update up to 50 tasks atomically
  - `complete_tasks` — complete (or reopen) up to 100 tasks at once
  - `delete_tasks` — permanently delete up to 50 tasks (all validated before any are deleted)
  - `create_sections` — create up to 30 sections; duplicate names are returned as-is (`created: false`)
  - `update_sections` — rename up to 30 sections atomically
  - `create_comments` — add up to 50 comments to tasks at once
  - `delete_comments` — delete up to 50 comments (all validated before any are deleted)

  All batch tools return structured output with an `items` array. Errors include an `itemIndex` field identifying which item caused the failure.

## 2026-06-07

### Improved

- **`list_tasks` now supports a `limit` parameter** — pass `limit: N` to control how many tasks are returned (default: 100, max: 500). Previously all tasks were returned with no cap, which could produce very large responses for big projects.
- **`DELETE /mcp` now returns 405** instead of 404. The server runs in stateless mode and does not support session deletion. Clients that probe this method will receive a clean `405 Method Not Allowed` with an `Allow: GET, POST` header.

## 2026-06-07

### Improved

- **`get_task` now returns `descriptionText`** — a Markdown version of the task description, computed on the fly from the internal rich text format. Use this field to read and display descriptions. Handles headings, bullet and numbered lists, blockquotes, code blocks, and inline formatting (bold, italic, code, strikethrough). The raw `description` field is still included for clients that need the full node tree.

## 2026-06-07 (initial)

### Improved

- **`get_task` now accepts public task references** — pass `taskId: "SCD-426"` directly and the server resolves it automatically across all your workspaces. No longer need to look up the internal ID first.
- **`get_task` and `list_tasks` support number-based lookup** — pass `workspaceId + number: 426` as an alternative to the internal task ID.
- **`create_project` and `create_section` are now idempotent** — if a project or section with the same name already exists, the existing one is returned instead of creating a duplicate. The response includes a `created` field (`true` / `false`) so you can tell which case occurred.
- **All tools return structured output** — every tool declares an `outputSchema`, enabling structured content in clients that support the MCP spec (ChatGPT, Claude, and others).
- **`openWorldHint: false`** added to all tool annotations.
