# import_landscape_layer_weight

Import a raw binary weight map for a paint layer onto an existing Landscape actor. The file must match the landscape's vertex resolution exactly.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Absolute path to the .raw weight map file (uint8 per pixel). |
| `layer_name` | string | Yes | Name of the landscape layer to paint (must be assigned to the landscape). |
| `layer_info_asset` | string | No | Asset path to the LandscapeLayerInfoObject (fallback if layer_name lookup fails). |
| `actor_label` | string | No | Label or name of the Landscape actor. If omitted, uses the first Landscape found. |

## File Format

The input file must be a flat array of `uint8` values (0-255), one byte per vertex, matching the landscape's vertex resolution. The expected file size is `width * height` bytes. A value of 255 means full weight for this layer at that vertex; 0 means no weight.

Use `get_landscape_info` to determine the expected resolution (`total_vertices_x` x `total_vertices_y`).

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `actor_label` | string | Display label of the Landscape actor |
| `layer_name` | string | Name of the layer that was painted |
| `layer_info_asset` | string | Asset path of the layer info object used |
| `file_path` | string | Path to the imported file |
| `width` | number | Weight map width in pixels |
| `height` | number | Weight map height in pixels |
| `pixels_imported` | number | Total number of weight values applied |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "import_landscape_layer_weight",
    "arguments": {
      "file_path": "C:/Temp/grass_weight.raw",
      "layer_name": "Grass"
    }
  }
}
```

Response:

```json
{
  "actor_label": "Landscape",
  "layer_name": "Grass",
  "layer_info_asset": "/Game/Landscape/LI_Grass.LI_Grass",
  "file_path": "C:/Temp/grass_weight.raw",
  "width": 127,
  "height": 127,
  "pixels_imported": 16129
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing file_path | "file_path is required" |
| Missing layer_name | "layer_name is required" |
| No editor world | "No editor world available" |
| No landscape in level | "No Landscape actor found in the current level" |
| Actor label not found | "No Landscape actor found matching '{label}'" |
| No LandscapeInfo | "Failed to get LandscapeInfo" |
| Layer not found | "Layer not found: '{name}'. Assign the layer to the landscape first." |
| No extent | "Failed to get landscape extent" |
| File not found | "Failed to read file: {path}" |
| Size mismatch | "File size mismatch: expected N bytes (WxH uint8), got M bytes" |
