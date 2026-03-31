# create_sound_attenuation

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_Lifecycle.cpp`

## Description

Creates a new Sound Attenuation Settings asset. Optionally sets the attenuation shape, inner radius (shape extents), and falloff distance. The asset is saved to disk immediately.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the new Sound Attenuation |
| `path` | string | No | `/Game` | Content path for the asset |
| `shape` | string | No | `Sphere` | Attenuation shape: `Sphere`, `Capsule`, `Box`, `Cone` |
| `inner_radius` | number | No | 400 | Inner radius / shape extents (in cm) |
| `falloff_distance` | number | No | 3600 | Falloff distance (in cm) |

## Response

```json
{
  "name": "MyAttenuation",
  "asset_path": "/Game/MyAttenuation.MyAttenuation",
  "type": "SoundAttenuation",
  "shape": "Sphere",
  "inner_radius": 400,
  "falloff_distance": 3600
}
```

## Examples

### Create with default settings
```json
{ "tool": "create_sound_attenuation", "arguments": { "name": "Atten_Default" } }
```

### Create with custom shape and distances
```json
{
  "tool": "create_sound_attenuation",
  "arguments": {
    "name": "Atten_Close",
    "path": "/Game/Audio",
    "shape": "Sphere",
    "inner_radius": 200,
    "falloff_distance": 1000
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `name` | `"name is required"` |
| Asset already exists | `"Asset already exists: <path>"` |
| Invalid shape | `"Invalid shape: '<value>' (valid: Sphere, Capsule, Box, Cone)"` |
