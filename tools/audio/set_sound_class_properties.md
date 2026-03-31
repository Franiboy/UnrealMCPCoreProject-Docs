# set_sound_class_properties

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_Config.cpp`

## Description

Sets properties on an existing Sound Class asset. Supports volume, pitch, low-pass filter, attenuation distance scale, LFE bleed, priority flags, reverb, and 2D reverb send. Only provided properties are modified; others are left unchanged.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Sound Class |
| `volume` | number | No | | Volume multiplier |
| `pitch` | number | No | | Pitch multiplier |
| `low_pass_filter_frequency` | number | No | | Lowpass filter cutoff frequency in Hz |
| `attenuation_distance_scale` | number | No | | Attenuation distance scale multiplier |
| `lfe_bleed` | number | No | | Amount to bleed to the LFE channel |
| `always_play` | boolean | No | | Whether to inflate priority to always play |
| `is_ui_sound` | boolean | No | | Whether this sound plays when the game is paused |
| `reverb` | boolean | No | | Whether sounds send to the master reverb submix |
| `default_2d_reverb_send_amount` | number | No | | Reverb send amount for unattenuated (2D) sounds |

At least one property besides `asset_path` must be provided.

## Response

```json
{
  "name": "SC_Music",
  "asset_path": "/Game/SC_Music.SC_Music",
  "type": "SoundClass",
  "volume": 0.5,
  "pitch": 1.0,
  "low_pass_filter_frequency": 20000.0,
  "attenuation_distance_scale": 1.0,
  "lfe_bleed": 0.0,
  "always_play": false,
  "is_ui_sound": false,
  "reverb": true,
  "default_2d_reverb_send_amount": 0.0,
  "changed_properties": ["volume"]
}
```

## Examples

### Set volume and pitch
```json
{
  "tool": "set_sound_class_properties",
  "arguments": { "asset_path": "/Game/SC_Music", "volume": 0.5, "pitch": 1.2 }
}
```

### Enable always-play and UI sound
```json
{
  "tool": "set_sound_class_properties",
  "arguments": { "asset_path": "/Game/SC_UI", "always_play": true, "is_ui_sound": true }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"asset_path is required"` |
| Asset not found | `"Sound Class not found: <path>"` |
| No properties specified | `"No properties specified to change. Provide at least one of: ..."` |
