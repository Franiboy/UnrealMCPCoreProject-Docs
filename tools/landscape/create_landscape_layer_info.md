# create_landscape_layer_info

Create a new Landscape Layer Info asset (`ULandscapeLayerInfoObject`). Used for weight-based landscape painting layers.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | string | **Yes** | Asset name for the layer info |
| `path` | string | No | Content path to create the asset in (default: `/Game`) |
| `layer_name` | string | No | Layer name used for painting (defaults to asset name) |
| `hardness` | number | No | Hardness 0.0-1.0 (default 0.5) — controls erosion resistance |
| `no_weight_blend` | boolean | No | If true, layer is non-weight-blended (default false) |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `asset_path` | string | Full object path of the created asset |
| `asset_name` | string | Asset name |
| `layer_name` | string | Layer name for painting |
| `hardness` | number | Hardness value |
| `no_weight_blend` | boolean | Weight-blend mode |
| `debug_color` | object | Randomly generated debug color (`r`, `g`, `b`, `a`) |

## Notes

- Layer info assets are used to define paint layers on a landscape.
- Assign a layer info to a landscape via `set_landscape_property` or the landscape material.
- Non-weight-blended layers are painted independently without affecting other layers' weights.
- A random debug color is auto-generated for visualization in the editor.

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "create_landscape_layer_info",
    "arguments": {
      "name": "LI_Grass",
      "path": "/Game/Landscape",
      "layer_name": "Grass",
      "hardness": 0.5,
      "no_weight_blend": false
    }
  }
}
```

Response:

```json
{
  "asset_path": "/Game/Landscape/LI_Grass.LI_Grass",
  "asset_name": "LI_Grass",
  "layer_name": "Grass",
  "hardness": 0.5,
  "no_weight_blend": false,
  "debug_color": { "r": 0.42, "g": 0.78, "b": 0.15, "a": 1.0 }
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing name | "name is required" |
| Duplicate asset | "Asset already exists: {path}" |
| Package creation failure | "Failed to create package: {path}" |
| Object creation failure | "Failed to create ULandscapeLayerInfoObject" |
