# set_sound_attenuation

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_Config.cpp`

## Description

Sets properties on an existing Sound Attenuation Settings asset. Supports attenuation shape, distance model, falloff, spatialization, occlusion, air absorption, and reverb send. Only provided properties are modified; others are left unchanged.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Sound Attenuation |
| `shape` | string | No | | Attenuation shape: `Sphere`, `Capsule`, `Box`, `Cone` |
| `inner_radius` | number | No | | Inner radius / shape extents in cm |
| `falloff_distance` | number | No | | Falloff distance in cm |
| `distance_algorithm` | string | No | | Distance model: `Linear`, `Logarithmic`, `Inverse`, `LogReverse`, `NaturalSound`, `Custom` |
| `attenuate` | boolean | No | | Enable volume attenuation |
| `spatialize` | boolean | No | | Enable 3D spatialization |
| `spatialization_algorithm` | string | No | | Spatialization method: `Default` (panning) or `HRTF` (binaural) |
| `enable_occlusion` | boolean | No | | Enable realtime occlusion tracing |
| `occlusion_low_pass_filter_frequency` | number | No | | LPF frequency (Hz) when occluded |
| `occlusion_volume_attenuation` | number | No | | Volume attenuation when occluded (0.0–1.0) |
| `occlusion_interpolation_time` | number | No | | Time in seconds to interpolate to occluded state |
| `enable_air_absorption` | boolean | No | | Enable air absorption (LPF as function of distance) |
| `enable_reverb_send` | boolean | No | | Enable distance-based reverb send |

At least one property besides `asset_path` must be provided.

## Response

```json
{
  "name": "ATT_Footsteps",
  "asset_path": "/Game/ATT_Footsteps.ATT_Footsteps",
  "type": "SoundAttenuation",
  "shape": "Sphere",
  "inner_radius": 500.0,
  "falloff_distance": 5000.0,
  "distance_algorithm": "Linear",
  "attenuate": true,
  "spatialize": true,
  "spatialization_algorithm": "Default",
  "enable_occlusion": false,
  "occlusion_low_pass_filter_frequency": 5000.0,
  "occlusion_volume_attenuation": 0.0,
  "occlusion_interpolation_time": 0.20000000298023224,
  "enable_air_absorption": false,
  "enable_reverb_send": true,
  "changed_properties": ["shape", "inner_radius", "falloff_distance"]
}
```

## Examples

### Set shape and radius
```json
{
  "tool": "set_sound_attenuation",
  "arguments": { "asset_path": "/Game/ATT_Footsteps", "shape": "Sphere", "inner_radius": 500, "falloff_distance": 5000 }
}
```

### Enable HRTF spatialization with occlusion
```json
{
  "tool": "set_sound_attenuation",
  "arguments": {
    "asset_path": "/Game/ATT_Voice",
    "spatialize": true,
    "spatialization_algorithm": "HRTF",
    "enable_occlusion": true,
    "occlusion_low_pass_filter_frequency": 3000,
    "occlusion_volume_attenuation": 0.5
  }
}
```

### Change distance algorithm
```json
{
  "tool": "set_sound_attenuation",
  "arguments": { "asset_path": "/Game/ATT_Ambient", "distance_algorithm": "Logarithmic" }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"asset_path is required"` |
| Asset not found | `"Sound Attenuation not found: <path>"` |
| Invalid `shape` | `"Invalid shape: '<value>' (valid: Sphere, Capsule, Box, Cone)"` |
| Invalid `distance_algorithm` | `"Invalid distance_algorithm: '<value>' (valid: Linear, Logarithmic, ...)"` |
| Invalid `spatialization_algorithm` | `"Invalid spatialization_algorithm: '<value>' (valid: Default, HRTF)"` |
| No properties specified | `"No properties specified to change. Provide at least one of: ..."` |
