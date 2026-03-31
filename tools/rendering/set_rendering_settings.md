# set_rendering_settings

Modify global rendering settings via console variables. Accepts friendly key names that map to engine CVars, or raw CVar names directly.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `settings` | object | **Yes** | Key/value pairs of settings to apply. Supported friendly keys: `gi_method` (0–3), `reflection_method` (0–2), `shadow_method` (0–1), `anti_aliasing_method` (0, 1, 2, 4), `nanite_enabled` (bool), `ray_tracing_enabled` (bool), `forward_shading` (bool), `bloom_enabled` (bool), `ao_enabled` (bool), `auto_exposure_enabled` (bool), `motion_blur_enabled` (bool). Raw CVar names (e.g. `r.AntiAliasingMethod`) are also accepted. |

## Example

**Request:**
```json
{
  "tool": "set_rendering_settings",
  "arguments": {
    "settings": {
      "gi_method": 1,
      "nanite_enabled": true,
      "bloom_enabled": true,
      "forward_shading": false
    }
  }
}
```

**Response:**
```json
{
  "applied": [
    { "key": "gi_method", "cvar": "r.DynamicGlobalIlluminationMethod", "value": 1 },
    { "key": "nanite_enabled", "cvar": "r.Nanite", "value": 1 },
    { "key": "bloom_enabled", "cvar": "r.DefaultFeature.Bloom", "value": 1 },
    { "key": "forward_shading", "cvar": "r.ForwardShading", "value": 0 }
  ],
  "applied_count": 4
}
```

## Notes

- Friendly key names are mapped to their corresponding engine console variables automatically.
- Raw CVar names can be passed directly for settings not covered by the friendly key list.
- Boolean values are converted to `0`/`1` for the underlying CVar.
- If a CVar name is invalid or cannot be set, it is reported in an optional `errors` array in the response.
- Changes take effect immediately but may require a viewport refresh to be visible.
