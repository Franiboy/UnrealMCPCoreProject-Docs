# is_pie_running

Check if a Play In Editor session is currently active.

## Parameters

None.

## Example

**Request:**
```json
{
  "tool": "is_pie_running",
  "arguments": {}
}
```

**Response (no session):**
```json
{
  "running": false,
  "simulating": false,
  "queued": false,
  "paused": false
}
```

**Response (active session):**
```json
{
  "running": true,
  "simulating": false,
  "queued": false,
  "paused": false,
  "elapsed_time": 12.5,
  "instance_count": 1,
  "player": {
    "pawn_class": "DefaultPawn",
    "location": { "x": 100.0, "y": 200.0, "z": 50.0 }
  }
}
```

## Notes

- `running` is true when `GEditor->PlayWorld` exists.
- `simulating` is true when using Simulate In Editor mode.
- `queued` is true if a play session request is pending but not yet started.
- Player info (pawn class, location) is only available during a non-simulate PIE session.
