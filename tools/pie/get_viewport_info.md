# get_viewport_info

Get information about editor viewports including camera position, rotation, FOV, type, and resolution.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `index` | integer | No | Viewport index (0-based). If omitted, returns all viewports. |

## Example

**Request (all viewports):**
```json
{
  "tool": "get_viewport_info",
  "arguments": {}
}
```

**Response:**
```json
{
  "viewports": [
    {
      "index": 0,
      "location": { "x": 0.0, "y": 0.0, "z": 200.0 },
      "rotation": { "pitch": -30.0, "yaw": 0.0, "roll": 0.0 },
      "fov": 90.0,
      "realtime": true,
      "width": 1920,
      "height": 1080,
      "type": "Perspective"
    }
  ],
  "count": 1
}
```

## Notes

- Returns error if no viewports are available (e.g., headless mode without viewports).
- Viewport types include `Perspective`, `OrthoXY`, `OrthoXZ`, `OrthoYZ`, `OrthoFreelook`, and their negative variants.
- Use a specific `index` to query a single viewport, or omit to get all.
