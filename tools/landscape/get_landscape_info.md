# get_landscape_info

Inspect a Landscape actor in the current level: size, components, layers, material, LOD settings, collision settings, Nanite, and edit layers.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `actor_label` | string | No | Label or name of the Landscape actor. If omitted, returns info for the first Landscape found in the level. |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `actor_name` | string | Internal name of the Landscape actor |
| `actor_label` | string | Display label of the Landscape actor |
| `class` | string | Class name (e.g. "Landscape", "LandscapeStreamingProxy") |
| `location` | object | Actor world location (`x`, `y`, `z`) |
| `component_count` | number | Number of landscape components |
| `component_size_quads` | number | Quads per component side |
| `subsection_size_quads` | number | Quads per subsection side |
| `num_subsections` | number | Subsections per component side |
| `grid_columns` | number | Number of components across X |
| `grid_rows` | number | Number of components across Y |
| `total_vertices_x` | number | Total vertex resolution in X |
| `total_vertices_y` | number | Total vertex resolution in Y |
| `landscape_material` | string | Path to the landscape material (or "None") |
| `landscape_hole_material` | string | Path to the hole material (if set) |
| `lod_settings` | object | LOD configuration (see below) |
| `collision` | object | Collision configuration (see below) |
| `nanite_enabled` | boolean | Whether Nanite rendering is enabled |
| `target_layers` | array | Paint layers assigned to this landscape |
| `target_layer_count` | number | Number of target layers |
| `has_layers_content` | boolean | Whether edit layers are active (ALandscape only) |
| `edit_layers` | array | Edit layer details (ALandscape only) |
| `edit_layer_count` | number | Number of edit layers |

### lod_settings object

| Field | Type | Description |
|-------|------|-------------|
| `max_lod_level` | number | Maximum LOD level (-1 = max available) |
| `lod0_screen_size` | number | LOD 0 screen size threshold |
| `lod0_distribution_setting` | number | LOD 0 distribution setting |
| `lod_distribution_setting` | number | Other LODs distribution setting |
| `static_lighting_lod` | number | LOD level for lightmass |

### collision object

| Field | Type | Description |
|-------|------|-------------|
| `collision_mip_level` | number | LOD for collision tests |
| `simple_collision_mip_level` | number | LOD for simple collision |
| `generate_overlap_events` | boolean | Whether overlap events are generated |
| `used_for_navigation` | boolean | Whether used for navigation |

### target_layers array items

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Layer name |
| `layer_info_asset` | string | Path to the LandscapeLayerInfoObject |
| `hardness` | number | Erosion resistance (0-1) |
| `physical_material` | string | Physical material path |
| `no_weight_blend` | boolean | Whether weight blending is disabled |

### edit_layers array items

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Edit layer name |
| `guid` | string | Edit layer GUID |
| `visible` | boolean | Visibility state |
| `locked` | boolean | Lock state |
| `heightmap_alpha` | number | Heightmap blend alpha |
| `weightmap_alpha` | number | Weightmap blend alpha |
| `brush_count` | number | Number of brushes in this layer |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "get_landscape_info",
    "arguments": {}
  }
}
```

Response:

```json
{
  "actor_name": "Landscape_0",
  "actor_label": "Landscape",
  "class": "Landscape",
  "location": { "x": 0, "y": 0, "z": 0 },
  "component_count": 16,
  "component_size_quads": 63,
  "subsection_size_quads": 63,
  "num_subsections": 1,
  "grid_columns": 4,
  "grid_rows": 4,
  "total_vertices_x": 253,
  "total_vertices_y": 253,
  "landscape_material": "/Game/Materials/M_Landscape.M_Landscape",
  "lod_settings": {
    "max_lod_level": -1,
    "lod0_screen_size": 0.5,
    "lod0_distribution_setting": 1.25,
    "lod_distribution_setting": 3.0,
    "static_lighting_lod": 0
  },
  "collision": {
    "collision_mip_level": 0,
    "simple_collision_mip_level": 0,
    "generate_overlap_events": false,
    "used_for_navigation": true
  },
  "nanite_enabled": false,
  "target_layers": [
    {
      "name": "Grass",
      "layer_info_asset": "/Game/Landscape/LI_Grass.LI_Grass",
      "hardness": 0.5,
      "no_weight_blend": false
    }
  ],
  "target_layer_count": 1,
  "has_layers_content": false,
  "edit_layers": [],
  "edit_layer_count": 0
}
```

## Errors

| Condition | Error |
|-----------|-------|
| No editor world | "No editor world available" |
| No landscape in level | "No Landscape actor found in the current level" |
| Actor label not found | "No Landscape actor found matching '{label}'" |
