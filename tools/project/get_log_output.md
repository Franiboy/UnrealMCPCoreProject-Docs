# get_log_output

Retrieve recent Output Log entries from a persistent ring buffer that captures all log categories. Supports filtering by category (substring match) and minimum verbosity level. Entries are returned in chronological order (oldest first).

The ring buffer starts capturing when the plugin registers its Project tools and holds up to 2000 entries.

## Input

```json
{}
```

| Parameter   | Required | Default | Description                                                                 |
| ----------- | -------- | ------- | --------------------------------------------------------------------------- |
| `category`  | no       | —       | Filter entries by log category (case-insensitive substring match).          |
| `verbosity` | no       | `Log`   | Minimum verbosity level: `Fatal`, `Error`, `Warning`, `Display`, `Log`, `Verbose`, `VeryVerbose`. Only entries at or above this severity are returned. |
| `max_lines` | no       | `100`   | Maximum number of entries to return (1–2000).                               |

## Output

| Field             | Type   | Description                                              |
| ----------------- | ------ | -------------------------------------------------------- |
| `count`           | number | Number of entries returned (after filtering)             |
| `total_buffered`  | number | Total entries currently in the ring buffer               |
| `entries`         | array  | Array of log entry objects (see below)                   |
| `category_filter` | string | The category filter applied (only present if specified)  |
| `verbosity_filter`| string | The verbosity level used for filtering                   |
| `max_lines`       | number | The max_lines limit that was applied                     |

### Entry object

| Field       | Type   | Description                                              |
| ----------- | ------ | -------------------------------------------------------- |
| `category`  | string | Log category name (e.g. `LogMCP`, `LogTemp`, `LogBlueprintUserMessages`) |
| `verbosity` | string | Verbosity level: `Fatal`, `Error`, `Warning`, `Display`, `Log`, `Verbose`, `VeryVerbose` |
| `message`   | string | The log message text                                     |

## Examples

### Get recent log entries (defaults)

```json
{}
```

### Filter by LogMCP category

```json
{
  "category": "LogMCP"
}
```

### Get only warnings and errors

```json
{
  "verbosity": "Warning"
}
```

### Combine filters with a limit

```json
{
  "category": "LogMCP",
  "verbosity": "Log",
  "max_lines": 50
}
```

## Errors

This tool does not fail — it always returns available log entries (which may be an empty array if nothing matches the filters or the buffer is empty).
