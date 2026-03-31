# set_viewport_camera

Move the editor viewport camera to a new position, rotation, and/or field of view.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `location` | object | No* | Camera location `{x, y, z}` |
| `rotation` | object | No* | Camera rotation `{pitch, yaw, roll}` |
| `fov` | number | No* | Field of view in degrees |
| `index` | integer | No | Viewport index (0-based, default: 0) |

\* At least one of `location`, `rotation`, or `fov` is required.

## Example

**Request:**
```json
{
  "tool": "set_viewport_camera",
  "arguments": {
    "location": { "x": 100, "y": 200, "z": 300 },
    "rotation": { "pitch": -15, "yaw": 45, "roll": 0 },
    "fov": 90
  }
}
```

**Response:**
```json
{
  "index": 0,
  "location": { "x": 100.0, "y": 200.0, "z": 300.0 },
  "rotation": { "pitch": -15.0, "yaw": 45.0, "roll": 0.0 },
  "fov": 90.0,
  "changed": ["location", "rotation", "fov"]
}
```

## Notes

- Returns error if no viewports are available or if the index is out of range.
- Partial updates are supported: you can set only location, only rotation, or only FOV.
- The response includes the final camera state after the update.
