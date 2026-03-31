# get_sound_cue_info

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_Query.cpp`

## Description

Inspects a Sound Cue asset and returns detailed information including its node graph, connections, volume/pitch multipliers, attenuation settings, sound class, and duration. Each node includes its class, name, root status, and child indices. Wave-player nodes additionally include the referenced SoundWave path and looping flag.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Full asset path (e.g. `/Game/Audio/Explosion_Cue`) |

## Response

```json
{
  "name": "Explosion_Cue",
  "asset_path": "/Game/StarterContent/Audio/Explosion_Cue.Explosion_Cue",
  "volume_multiplier": 0.9,
  "pitch_multiplier": 1.0,
  "duration": 3.25,
  "max_distance": 2200.0,
  "override_attenuation": true,
  "sound_class": "Master",
  "node_count": 4,
  "nodes": [
    {
      "index": 0,
      "class": "SoundNodeWavePlayer",
      "name": "SoundNodeWavePlayer_1",
      "is_root": false,
      "sound_wave": "/Game/StarterContent/Audio/Explosion01.Explosion01",
      "looping": false
    },
    {
      "index": 1,
      "class": "SoundNodeRandom",
      "name": "SoundNodeRandom_0",
      "is_root": false,
      "children": [0]
    },
    {
      "index": 2,
      "class": "SoundNodeModulator",
      "name": "SoundNodeModulator_0",
      "is_root": true,
      "children": [1]
    }
  ]
}
```

## Examples

### Inspect a Sound Cue
```json
{
  "tool": "get_sound_cue_info",
  "arguments": {
    "asset_path": "/Game/StarterContent/Audio/Explosion_Cue"
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"Missing required parameter 'asset_path'"` |
| Empty `asset_path` | `"Parameter 'asset_path' must not be empty"` |
| Asset not found or wrong type | `"Sound Cue not found: '<path>'"` |
