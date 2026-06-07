# Changelog

## 2026-06-07

### Improved

- **`get_task` now accepts public task references** — pass `taskId: "SCD-426"` directly and the server resolves it automatically across all your workspaces. No longer need to look up the internal ID first.
- **`get_task` and `list_tasks` support number-based lookup** — pass `workspaceId + number: 426` as an alternative to the internal task ID.
- **`create_project` and `create_section` are now idempotent** — if a project or section with the same name already exists, the existing one is returned instead of creating a duplicate. The response includes a `created` field (`true` / `false`) so you can tell which case occurred.
- **All tools return structured output** — every tool declares an `outputSchema`, enabling structured content in clients that support the MCP spec (ChatGPT, Claude, and others).
- **`openWorldHint: false`** added to all tool annotations.
