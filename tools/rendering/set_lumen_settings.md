# set_lumen_settings

Configure Lumen global illumination and reflection settings via console variables. Provides friendly key names for common Lumen parameters and accepts raw `r.Lumen.*` CVar names for advanced tuning.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `settings` | object | **Yes** | Key/value pairs of Lumen settings to apply. Supported keys: `gi_method` (0–3), `reflection_method` (0–2), `trace_mesh_sdfs` (bool), `software_ray_tracing` (bool), `hardware_ray_tracing` (bool), `scene_lighting_quality` (float), `scene_detail` (float), `scene_view_distance` (float), `final_gather_quality` (float), `final_gather_lighting_update_speed` (float), `max_trace_distance` (float), `ray_lighting_mode` (0–2), `reflection_quality` (float). Raw `r.Lumen.*` CVar names are also accepted. |

## Example

**Request:**
```json
{
  "tool": "set_lumen_settings",
  "arguments": {
    "settings": {
      "gi_method": 1,
      "reflection_method": 1,
      "hardware_ray_tracing": false,
      "scene_lighting_quality": 1.0,
      "final_gather_quality": 1.0,
      "max_trace_distance": 20000.0
    }
  }
}
```

**Response:**
```json
{
  "applied": [
    { "key": "gi_method", "cvar": "r.DynamicGlobalIlluminationMethod", "value": 1 },
    { "key": "reflection_method", "cvar": "r.ReflectionMethod", "value": 1 },
    { "key": "hardware_ray_tracing", "cvar": "r.Lumen.HardwareRayTracing", "value": 0 },
    { "key": "scene_lighting_quality", "cvar": "r.Lumen.DiffuseIndirect.Allow", "value": 1.0 },
    { "key": "final_gather_quality", "cvar": "r.Lumen.FinalGather.Quality", "value": 1.0 },
    { "key": "max_trace_distance", "cvar": "r.Lumen.MaxTraceDistance", "value": 20000.0 }
  ],
  "applied_count": 6
}
```

## Notes

- Friendly key names are mapped to their corresponding `r.Lumen.*` console variables automatically.
- `gi_method` and `reflection_method` control the top-level rendering method (not Lumen-specific): 0=None, 1=Lumen, 2=Screen Space, 3=Plugin (GI only).
- Boolean values are converted to `0`/`1` for the underlying CVar.
- Raw `r.Lumen.*` CVar names can be passed directly for parameters not covered by the friendly key list.
- If a CVar name is invalid or cannot be set, it is reported in an optional `errors` array in the response.
- Changes take effect immediately but may require a viewport refresh to be visible.
