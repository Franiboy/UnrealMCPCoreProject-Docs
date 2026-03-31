# import_landscape_heightmap

Import a raw binary heightmap file onto an existing Landscape actor. The file must match the landscape's vertex resolution exactly.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Absolute path to the .raw heightmap file (uint16 little-endian). |
| `actor_label` | string | No | Label or name of the Landscape actor. If omitted, uses the first Landscape found. |

## File Format

The input file must be a flat array of `uint16` values in little-endian byte order, matching the landscape's vertex resolution. The expected file size is `width * height * 2` bytes, where width and height correspond to the landscape's total vertex count along each axis.

Use `export_landscape_heightmap` to get a correctly sized file, or `get_landscape_info` to determine the expected resolution (`total_vertices_x` x `total_vertices_y`).

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `actor_label` | string | Display label of the Landscape actor |
| `file_path` | string | Path to the imported file |
| `width` | number | Heightmap width in pixels |
| `height` | number | Heightmap height in pixels |
| `pixels_imported` | number | Total number of height values applied |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "import_landscape_heightmap",
    "arguments": {
      "file_path": "C:/Temp/landscape_height.raw"
    }
  }
}
```

Response:

```json
{
  "actor_label": "Landscape",
  "file_path": "C:/Temp/landscape_height.raw",
  "width": 127,
  "height": 127,
  "pixels_imported": 16129
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing file_path | "file_path is required" |
| No editor world | "No editor world available" |
| No landscape in level | "No Landscape actor found in the current level" |
| Actor label not found | "No Landscape actor found matching '{label}'" |
| No LandscapeInfo | "Failed to get LandscapeInfo" |
| No extent | "Failed to get landscape extent" |
| File not found | "Failed to read file: {path}" |
| Size mismatch | "File size mismatch: expected N bytes (WxH uint16), got M bytes" |
