# Changelog

## 2026-06-07 (latest)

### Improved

- **`list_tasks` now supports a `limit` parameter** — pass `limit: N` to control how many tasks are returned (default: 100, max: 500). Previously all tasks were returned with no cap, which could produce very large responses for big projects.

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
