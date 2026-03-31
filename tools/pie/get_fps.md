# get_fps

Get current frame rate and frame time statistics.

## Parameters

None.

## Example

**Request:**
```json
{
  "tool": "get_fps",
  "arguments": {}
}
```

**Response:**
```json
{
  "delta_time": 0.016,
  "current_fps": 62.5,
  "average_fps": 60.0,
  "average_frame_time_ms": 16.67,
  "frame_time_ms": 16.0,
  "pie_active": false
}
```

## Notes

- `current_fps` is derived from the current delta time.
- `average_fps` and `average_frame_time_ms` use the engine's global average tracking (`GAverageFPS`, `GAverageMS`).
- When a PIE session is active, `pie_active` is true and `time_dilation` is included.
