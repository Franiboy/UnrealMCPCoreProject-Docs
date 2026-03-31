# create_physics_material

Create a new Physical Material asset with optional initial properties.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | string | Yes | Name of the Physical Material asset. |
| `path` | string | No | Content path (default: `/Game`). |
| `friction` | number | No | Friction coefficient (default: 0.7, min: 0). |
| `restitution` | number | No | Restitution / bounciness (default: 0.3, range: 0-1). |
| `density` | number | No | Density in g/cm3 (default: 1.0, min: 0). |
| `surface_type` | string | No | Surface type name (e.g. "Default", "SurfaceType1"). Default: "SurfaceType_Default". |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Created asset name |
| `asset_path` | string | Full asset path |
| `friction` | number | Friction value |
| `restitution` | number | Restitution value |
| `density` | number | Density value |
| `surface_type` | string | Surface type name |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "create_physics_material",
    "arguments": {
      "name": "PM_Ice",
      "path": "/Game/Physics",
      "friction": 0.1,
      "restitution": 0.2,
      "density": 0.92
    }
  }
}
```

Response:

```json
{
  "name": "PM_Ice",
  "asset_path": "/Game/Physics/PM_Ice.PM_Ice",
  "friction": 0.1,
  "restitution": 0.2,
  "density": 0.92,
  "surface_type": "Default"
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing name | "'name' is required" |
| Asset already exists | "Physical Material already exists: {path}" |
| Creation failed | "Failed to create Physical Material: {path}" |
