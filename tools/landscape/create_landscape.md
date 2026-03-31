# create_landscape

Create a new flat Landscape actor in the current level. Configurable grid size, section layout, position, and scale.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | string | No | Display label for the Landscape actor |
| `component_count_x` | integer | No | Number of components in X (1-32, default 1) |
| `component_count_y` | integer | No | Number of components in Y (1-32, default 1) |
| `sections_per_component` | integer | No | Sections per component (1 or 2, default 1) |
| `quads_per_section` | integer | No | Quads per section (7, 15, 31, 63, 127, or 255; default 63) |
| `location_x` | number | No | World X location (default 0) |
| `location_y` | number | No | World Y location (default 0) |
| `location_z` | number | No | World Z location (default 0) |
| `scale_x` | number | No | Scale X (default 100) |
| `scale_y` | number | No | Scale Y (default 100) |
| `scale_z` | number | No | Scale Z (default 100) |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `actor_name` | string | Internal name of the spawned Landscape actor |
| `actor_label` | string | Display label of the Landscape actor |
| `component_count_x` | number | Components in X |
| `component_count_y` | number | Components in Y |
| `sections_per_component` | number | Sections per component |
| `quads_per_section` | number | Quads per section |
| `total_vertices_x` | number | Total heightmap resolution in X |
| `total_vertices_y` | number | Total heightmap resolution in Y |
| `location` | object | World position (`x`, `y`, `z`) |
| `scale` | object | Actor scale (`x`, `y`, `z`) |

## Notes

- The landscape is created with a flat heightmap at mid-point elevation (32768).
- `quads_per_section` is snapped to the nearest valid value if an invalid number is provided.
- Total vertex resolution = `component_count * sections_per_component * quads_per_section + 1` per axis.
- Default scale of 100 gives standard 1m vertex spacing with default section settings.
- The landscape is an actor, not a content asset — use `delete_actor` to remove it.

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "create_landscape",
    "arguments": {
      "label": "MyTerrain",
      "component_count_x": 4,
      "component_count_y": 4,
      "quads_per_section": 63,
      "sections_per_component": 1
    }
  }
}
```

Response:

```json
{
  "actor_name": "Landscape_0",
  "actor_label": "MyTerrain",
  "component_count_x": 4,
  "component_count_y": 4,
  "sections_per_component": 1,
  "quads_per_section": 63,
  "total_vertices_x": 253,
  "total_vertices_y": 253,
  "location": { "x": 0, "y": 0, "z": 0 },
  "scale": { "x": 100, "y": 100, "z": 100 }
}
```

## Errors

| Condition | Error |
|-----------|-------|
| No editor world | "No editor world available" |
| Spawn failure | "Failed to spawn ALandscape actor" |
