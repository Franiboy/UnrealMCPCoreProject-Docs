# get_rendering_settings

Get global rendering settings from URendererSettings, including GI, reflections, shadows, anti-aliasing, Nanite, ray tracing, and default feature flags.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `category` | string | No | Filter category: `all` (default), `gi`, `reflections`, `shadows`, `anti_aliasing`, `nanite`, `ray_tracing`, `features` |

## Example

**Request:**
```json
{
  "tool": "get_rendering_settings",
  "arguments": {
    "category": "all"
  }
}
```

**Response:**
```json
{
  "global_illumination": {
    "method": 1,
    "method_name": "Lumen"
  },
  "reflections": {
    "method": 1,
    "method_name": "Lumen"
  },
  "shadows": {
    "method": 1,
    "method_name": "Virtual Shadow Maps"
  },
  "anti_aliasing": {
    "method": 2,
    "method_name": "TAA"
  },
  "nanite": {
    "enabled": true
  },
  "ray_tracing": {
    "enabled": false,
    "shadows": false
  },
  "default_features": {
    "bloom": true,
    "ambient_occlusion": true,
    "auto_exposure": true,
    "motion_blur": true,
    "forward_shading": false
  }
}
```

## Notes

- When `category` is omitted or set to `all`, all sections are returned.
- When a specific category is provided, only that section is included in the response.
- Settings are read from URendererSettings and reflect the project-wide rendering configuration.
- Method integer values correspond to engine enums (e.g., GI: 0=None, 1=Lumen, 2=Screen Space, 3=Plugin).
