# get_request_log

Get the history of all MCP tool calls made in the current session. Useful for AI agents to review their own actions, debug failures, or build context about what has been done so far.

## Input

```json
{
  "limit": 10,
  "tool_filter": "create_blueprint",
  "success_filter": true,
  "include_details": false
}
```

| Parameter         | Required | Description                                                                                         |
| ----------------- | -------- | --------------------------------------------------------------------------------------------------- |
| `limit`           | no       | Maximum number of entries to return (most recent first). Omit or 0 to return all entries.           |
| `tool_filter`     | no       | Filter entries by tool name (e.g. `create_blueprint`). Omit to return all tools.                    |
| `success_filter`  | no       | Filter by success status: `true` = only successful, `false` = only failed. Omit to return both.    |
| `include_details` | no       | Include full input arguments, response content, and captured log messages. Defaults to `false`.     |

## Output

| Field          | Type  | Description                                        |
| -------------- | ----- | -------------------------------------------------- |
| `entries[]`    | array | Array of log entry descriptors (see below)         |
| `count`        | int   | Number of entries returned (after filtering)        |
| `total_in_log` | int   | Total number of entries in the log (before filters) |

## Entry Descriptor Fields

Always present:

| Field         | Type   | Description                                                  |
| ------------- | ------ | ------------------------------------------------------------ |
| `id`          | int    | Unique entry ID (monotonically increasing)                   |
| `timestamp`   | string | ISO-style timestamp of the request                           |
| `tool`        | string | Tool name (e.g. `get_blueprint`)                             |
| `duration_ms` | number | Execution time in milliseconds                               |
| `success`     | bool   | Whether the tool call succeeded                              |
| `undone`      | bool   | Whether this entry was reverted via the Monitor Panel        |

Present when non-empty:

| Field            | Type   | Description                                               |
| ---------------- | ------ | --------------------------------------------------------- |
| `error`          | string | Error message (only on failed calls)                      |
| `summary`        | string | Human-readable summary of key parameters                  |
| `asset_path`     | string | Primary asset path involved                               |
| `change_summary` | string | Description of what changed (mutating tools only)         |

Present only when `include_details` is `true`:

| Field      | Type   | Description                                             |
| ---------- | ------ | ------------------------------------------------------- |
| `input`    | object | Full input arguments as passed to the tool              |
| `response` | object | Full response content (parsed from inner JSON)          |
| `log`      | array  | Array of UE log messages captured during tool execution |

## Notes

- Entries for `get_request_log` itself are excluded to avoid recursion noise.
- Default mode (without `include_details`) returns a compact summary suitable for context building.
- Use `include_details: true` with `limit` to inspect specific recent calls without overwhelming the context window.
