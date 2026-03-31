# set_sound_mix_properties

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_Config.cpp`

## Description

Sets properties on an existing Sound Mix asset. Supports EQ toggle, 4-band EQ settings, timing (initial delay, fade in/out, duration), and sound class effect overrides. Only provided properties are modified; others are left unchanged.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Sound Mix |
| `apply_eq` | boolean | No | | Whether to apply the EQ effect |
| `eq_priority` | number | No | | EQ priority (higher overrides lower) |
| `eq_settings` | object | No | | 4-band EQ settings (see below) |
| `initial_delay` | number | No | | Delay in seconds before the mix is applied |
| `fade_in_time` | number | No | | Time in seconds for the mix to fade in |
| `duration` | number | No | | Duration in seconds (-1 = indefinite) |
| `fade_out_time` | number | No | | Time in seconds for the mix to fade out |
| `sound_class_effects` | array | No | | Array of sound class adjusters (see below) |

At least one property besides `asset_path` must be provided.

### `eq_settings` Object

| Key | Type | Description |
|-----|------|-------------|
| `frequency_center_0` | number | Center frequency in Hz for band 0 |
| `gain_0` | number | Boost/cut of band 0 (0.0–10.0) |
| `bandwidth_0` | number | Bandwidth of band 0 (0.0–2.0) |
| `frequency_center_1` | number | Center frequency in Hz for band 1 |
| `gain_1` | number | Boost/cut of band 1 |
| `bandwidth_1` | number | Bandwidth of band 1 |
| `frequency_center_2` | number | Center frequency in Hz for band 2 |
| `gain_2` | number | Boost/cut of band 2 |
| `bandwidth_2` | number | Bandwidth of band 2 |
| `frequency_center_3` | number | Center frequency in Hz for band 3 |
| `gain_3` | number | Boost/cut of band 3 |
| `bandwidth_3` | number | Bandwidth of band 3 |

### `sound_class_effects` Array Element

| Key | Type | Description |
|-----|------|-------------|
| `sound_class_path` | string | Asset path of a Sound Class to adjust |
| `volume_adjuster` | number | Volume multiplier (default 1.0) |
| `pitch_adjuster` | number | Pitch multiplier (default 1.0) |
| `low_pass_filter_frequency` | number | LPF cutoff frequency |
| `apply_to_children` | boolean | Apply adjuster to child classes |
| `voice_center_channel_volume_adjuster` | number | Center channel volume multiplier |

## Response

```json
{
  "name": "MX_Underwater",
  "asset_path": "/Game/MX_Underwater.MX_Underwater",
  "type": "SoundMix",
  "apply_eq": true,
  "eq_priority": 5.0,
  "eq_settings": {
    "frequency_center_0": 200.0,
    "gain_0": 2.5,
    "bandwidth_0": 1.5,
    "frequency_center_1": 1000.0,
    "gain_1": 1.0,
    "bandwidth_1": 1.0,
    "frequency_center_2": 2000.0,
    "gain_2": 1.0,
    "bandwidth_2": 1.0,
    "frequency_center_3": 10000.0,
    "gain_3": 1.0,
    "bandwidth_3": 1.0
  },
  "initial_delay": 0.0,
  "fade_in_time": 2.0,
  "duration": -1.0,
  "fade_out_time": 3.0,
  "sound_class_effects": [],
  "changed_properties": ["apply_eq", "eq_priority", "fade_in_time", "fade_out_time"]
}
```

## Examples

### Enable EQ with custom band settings
```json
{
  "tool": "set_sound_mix_properties",
  "arguments": {
    "asset_path": "/Game/MX_Underwater",
    "apply_eq": true,
    "eq_settings": { "frequency_center_0": 200, "gain_0": 2.5, "bandwidth_0": 1.5 }
  }
}
```

### Set timing and sound class override
```json
{
  "tool": "set_sound_mix_properties",
  "arguments": {
    "asset_path": "/Game/MX_Combat",
    "fade_in_time": 1.0,
    "duration": 30.0,
    "fade_out_time": 2.0,
    "sound_class_effects": [
      { "sound_class_path": "/Game/SC_Music", "volume_adjuster": 0.3, "apply_to_children": true }
    ]
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"asset_path is required"` |
| Asset not found | `"Sound Mix not found: <path>"` |
| No properties specified | `"No properties specified to change. Provide at least one of: ..."` |
