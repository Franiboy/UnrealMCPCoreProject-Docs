# export_landscape_heightmap

Export the heightmap of a Landscape actor to a raw binary file (uint16 little-endian).

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file_path` | string | Yes | Absolute path for the output .raw file. |
| `actor_label` | string | No | Label or name of the Landscape actor. If omitted, uses the first Landscape found. |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `actor_label` | string | Display label of the Landscape actor |
| `file_path` | string | Path to the exported file |
| `width` | number | Heightmap width in pixels |
| `height` | number | Heightmap height in pixels |
| `file_size_bytes` | number | Size of the exported file in bytes |
| `format` | string | Always `"raw_uint16_le"` |

## File Format

The exported file is a flat array of `uint16` values in little-endian byte order. Each value represents the height at one vertex. The data is stored row-major, with `width * height` values totaling `width * height * 2` bytes.

A value of `32768` (0x8000) represents the landscape's zero height. Values above are positive elevation, values below are negative.

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "export_landscape_heightmap",
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
  "file_size_bytes": 32258,
  "format": "raw_uint16_le"
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
| Write failure | "Failed to write file: {path}" |
