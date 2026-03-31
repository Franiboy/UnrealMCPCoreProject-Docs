# set_landscape_property

Set one or more properties on a Landscape actor (LOD, collision, navigation, Nanite, streaming).

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `properties` | object | Yes | Object with property key/value pairs (see below). |
| `actor_label` | string | No | Label or name of the Landscape actor. If omitted, uses the first Landscape found. |

### Supported Properties

#### LOD

| Key | Type | Description |
|-----|------|-------------|
| `max_lod_level` | number | Maximum LOD level (-1 = max available) |
| `lod0_screen_size` | number | LOD 0 screen size threshold (clamped 0.1-10.0) |
| `lod0_distribution_setting` | number | LOD 0 distribution (clamped 1.0-10.0) |
| `lod_distribution_setting` | number | Other LODs distribution (clamped 1.0-10.0) |
| `static_lighting_lod` | number | LOD level for lightmass |
| `lod_blend_range` | number | LOD blend range (clamped 0.01-1.0) |

#### Collision

| Key | Type | Description |
|-----|------|-------------|
| `collision_mip_level` | number | LOD for collision tests |
| `simple_collision_mip_level` | number | LOD for simple collision |
| `generate_overlap_events` | boolean | Whether overlap events are generated |

#### Navigation

| Key | Type | Description |
|-----|------|-------------|
| `used_for_navigation` | boolean | Whether used for navigation |
| `fill_collision_under_landscape` | boolean | Fill collision under landscape for navmesh |

#### Rendering

| Key | Type | Description |
|-----|------|-------------|
| `nanite_enabled` | boolean | Enable/disable Nanite rendering |

#### Other

| Key | Type | Description |
|-----|------|-------------|
| `streaming_distance_multiplier` | number | Texture streaming distance multiplier |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `actor_label` | string | Display label of the Landscape actor |
| `properties_set` | number | Number of properties that were set |
| `values` | object | Confirmed values for each property set |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "set_landscape_property",
    "arguments": {
      "properties": {
        "max_lod_level": 4,
        "nanite_enabled": true,
        "collision_mip_level": 1
      }
    }
  }
}
```

Response:

```json
{
  "actor_label": "Landscape",
  "properties_set": 3,
  "values": {
    "max_lod_level": 4,
    "nanite_enabled": true,
    "collision_mip_level": 1
  }
}
```

## Errors

| Condition | Error |
|-----------|-------|
| No editor world | "No editor world available" |
| No landscape in level | "No Landscape actor found in the current level" |
| Actor label not found | "No Landscape actor found matching '{label}'" |
| Missing properties object | "'properties' object is required" |
| No recognized properties | "No recognized properties in 'properties' object" |
