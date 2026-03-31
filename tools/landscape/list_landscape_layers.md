# list_landscape_layers

List Landscape Layer Info assets. Returns layer name, physical material, hardness, weight-blend mode, and asset path.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `path_prefix` | string | No | Content path prefix to filter (default: `/Game`) |
| `name_filter` | string | No | Filter by asset name (case-insensitive substring match) |
| `include_engine` | boolean | No | Include engine/plugin content (default: `false`) |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `count` | number | Number of layers found |
| `path_prefix` | string | The path prefix used for filtering |
| `name_filter` | string | The name filter used (only present if provided) |
| `layers` | array | Array of layer info objects |

### layers array items

| Field | Type | Description |
|-------|------|-------------|
| `asset_path` | string | Full asset path of the LandscapeLayerInfoObject |
| `asset_name` | string | Asset name |
| `layer_name` | string | Landscape layer name |
| `hardness` | number | Erosion resistance (0.0 to 1.0) |
| `physical_material` | string | Physical material path (or "None") |
| `no_weight_blend` | boolean | Whether weight blending is disabled |
| `min_collision_relevance_weight` | number | Minimum weight for dominant physical layer |
| `debug_color` | object | Layer usage debug color (`r`, `g`, `b`, `a`) |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "list_landscape_layers",
    "arguments": {}
  }
}
```

Response:

```json
{
  "count": 3,
  "path_prefix": "/Game",
  "layers": [
    {
      "asset_path": "/Game/Landscape/LI_Grass.LI_Grass",
      "asset_name": "LI_Grass",
      "layer_name": "Grass",
      "hardness": 0.5,
      "physical_material": "/Game/Physics/PM_Grass.PM_Grass",
      "no_weight_blend": false,
      "min_collision_relevance_weight": 0.0,
      "debug_color": { "r": 0.0, "g": 1.0, "b": 0.0, "a": 1.0 }
    },
    {
      "asset_path": "/Game/Landscape/LI_Rock.LI_Rock",
      "asset_name": "LI_Rock",
      "layer_name": "Rock",
      "hardness": 1.0,
      "physical_material": "None",
      "no_weight_blend": false,
      "min_collision_relevance_weight": 0.0,
      "debug_color": { "r": 0.5, "g": 0.5, "b": 0.5, "a": 1.0 }
    }
  ]
}
```

## Errors

This tool does not produce errors — an empty result set returns `count: 0`.
