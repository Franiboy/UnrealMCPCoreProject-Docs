# take_screenshot

Capture a screenshot of the editor or PIE viewport.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `filename` | string | No | Output filename (without extension). Saved under `Saved/Screenshots/`. Default: auto-generated timestamp name. |

## Example

**Request:**
```json
{
  "tool": "take_screenshot",
  "arguments": {
    "filename": "my_screenshot"
  }
}
```

**Response:**
```json
{
  "requested": true,
  "path": "C:/Projects/MyGame/Saved/Screenshots/my_screenshot.png",
  "status": "Screenshot requested. File will be saved once the next frame renders."
}
```

## Notes

- Screenshots are saved as PNG files in the project's `Saved/Screenshots/` directory.
- The screenshot is requested asynchronously; the file will be written on the next rendered frame.
- Works in both editor and PIE viewports.
