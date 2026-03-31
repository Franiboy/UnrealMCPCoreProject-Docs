# list_physics_materials

List Physical Material assets in the project with optional filtering.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `path_prefix` | string | No | Content path prefix to filter (default: `/Game`). |
| `name_filter` | string | No | Filter by name (case-insensitive substring match). |
| `include_engine` | boolean | No | Include engine/plugin content (default: false). |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `physics_materials` | array | Array of physical material summaries |
| `count` | number | Number of results |

### physics_materials array items

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Asset name |
| `asset_path` | string | Full asset path |
| `class` | string | Class name |
| `friction` | number | Friction coefficient |
| `restitution` | number | Restitution (bounciness) |
| `density` | number | Density in g/cm3 |
| `surface_type` | string | Surface type name |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "list_physics_materials",
    "arguments": {}
  }
}
```

Response:

```json
{
  "physics_materials": [
    {
      "name": "PM_Ice",
      "asset_path": "/Game/Physics/PM_Ice.PM_Ice",
      "class": "PhysicalMaterial",
      "friction": 0.1,
      "restitution": 0.2,
      "density": 0.92,
      "surface_type": "Default"
    }
  ],
  "count": 1
}
```

## Errors

This tool always succeeds (returns an empty array if no materials match).
