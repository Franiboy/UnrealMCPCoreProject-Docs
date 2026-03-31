# pause_pie

Pause or unpause the current PIE session.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `paused` | boolean | No | `true` to pause, `false` to unpause. If omitted, toggles the current state. |

## Example

**Request:**
```json
{
  "tool": "pause_pie",
  "arguments": {
    "paused": true
  }
}
```

**Response:**
```json
{
  "paused": true,
  "status": "PIE paused"
}
```

## Notes

- Returns an error if no PIE session is currently running.
- When no `paused` argument is provided, the current pause state is toggled.
