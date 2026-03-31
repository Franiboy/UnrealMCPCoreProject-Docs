# set_nanite_settings

Configure Nanite per-mesh or global settings. When an `asset_path` is provided, modifies Nanite settings on a specific Static Mesh asset. When omitted, modifies global Nanite settings via console variables.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `asset_path` | string | No | Static Mesh asset path for per-mesh settings. If omitted, modifies global Nanite settings. |
| `settings` | object | **Yes** | Key/value pairs of settings to apply. Per-mesh keys: `enabled` (bool), `preserve_area` (bool), `explicit_tangents` (bool), `lerp_uvs` (bool), `position_precision` (int). Global mode also accepts raw `r.Nanite.*` CVar names. |

## Example

**Request (per-mesh):**
```json
{
  "tool": "set_nanite_settings",
  "arguments": {
    "asset_path": "/Game/Meshes/SM_Building",
    "settings": {
      "enabled": true,
      "preserve_area": true,
      "position_precision": 0
    }
  }
}
```

**Response (per-mesh):**
```json
{
  "mode": "per_mesh",
  "asset_path": "/Game/Meshes/SM_Building",
  "nanite_enabled": true,
  "applied": [
    { "key": "enabled", "value": true },
    { "key": "preserve_area", "value": true },
    { "key": "position_precision", "value": 0 }
  ],
  "applied_count": 3
}
```

**Request (global):**
```json
{
  "tool": "set_nanite_settings",
  "arguments": {
    "settings": {
      "enabled": true,
      "r.Nanite.MaxPixelsPerEdge": 1.0
    }
  }
}
```

**Response (global):**
```json
{
  "mode": "global",
  "applied": [
    { "key": "enabled", "cvar": "r.Nanite", "value": 1 },
    { "key": "r.Nanite.MaxPixelsPerEdge", "cvar": "r.Nanite.MaxPixelsPerEdge", "value": 1.0 }
  ],
  "applied_count": 2
}
```

## Notes

- When `asset_path` is provided, the tool operates in **per-mesh** mode and modifies Nanite build settings on the specified Static Mesh.
- When `asset_path` is omitted, the tool operates in **global** mode and applies settings via console variables.
- Per-mesh keys (`preserve_area`, `explicit_tangents`, `lerp_uvs`, `position_precision`) are only valid in per-mesh mode.
- In global mode, raw `r.Nanite.*` CVar names can be passed directly for advanced configuration.
- The Static Mesh asset is saved after per-mesh changes are applied.
