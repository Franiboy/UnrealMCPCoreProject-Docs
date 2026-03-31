# start_pie

Start a Play In Editor (PIE) session. Supports viewport, new window, or simulate-in-editor mode.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mode` | string | No | Play mode: `viewport` (default), `new_window`, or `simulate` |
| `start_location` | object | No | Start location `{x, y, z}` |
| `start_rotation` | object | No | Start rotation `{pitch, yaw, roll}` |

## Example

**Request:**
```json
{
  "tool": "start_pie",
  "arguments": {
    "mode": "viewport"
  }
}
```

**Response:**
```json
{
  "requested": true,
  "mode": "viewport",
  "status": "PIE session requested. Use is_pie_running to check status."
}
```

## Notes

- PIE start is asynchronous; use `is_pie_running` to check when the session is active.
- Returns an error if a PIE session is already running.
- `simulate` mode uses Simulate In Editor (no player pawn), useful for testing without gameplay input.
