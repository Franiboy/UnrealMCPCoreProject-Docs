# set_sound_cue_node_property

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_SoundCueEdit.cpp`

## Description

Sets properties on a Sound Cue node. Available properties depend on the node type. Also supports `set_as_root` on any node.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Sound Cue |
| `node_index` | integer | **Yes** | | Index of the node to modify |
| `set_as_root` | boolean | No | | Set this node as the cue's root |
| `sound_wave` | string | No | | (WavePlayer) Sound Wave asset path |
| `looping` | boolean | No | | (WavePlayer) Enable looping |
| `volume_min` | number | No | | (Modulator) Volume min |
| `volume_max` | number | No | | (Modulator) Volume max |
| `pitch_min` | number | No | | (Modulator) Pitch min |
| `pitch_max` | number | No | | (Modulator) Pitch max |
| `delay_min` | number | No | | (Delay) Delay min in seconds |
| `delay_max` | number | No | | (Delay) Delay max in seconds |
| `loop_count` | number | No | | (Looping) Number of loops |
| `loop_indefinitely` | boolean | No | | (Looping) Loop forever |
| `attenuation_asset` | string | No | | (Attenuation) Sound Attenuation asset path |
| `override_attenuation` | boolean | No | | (Attenuation) Override default attenuation |
| `randomize_without_replacement` | boolean | No | | (Random) Avoid repeats |
| `weights` | number[] | No | | (Random) Per-child weight array |
| `input_volumes` | number[] | No | | (Mixer/Concatenator) Per-input volume array |
| `int_parameter_name` | string | No | | (Switch) Integer parameter name |

## Response

```json
{
  "index": 1,
  "class": "SoundNodeModulator",
  "name": "SoundNodeModulator_0",
  "is_root": false,
  "volume_min": 0.3,
  "volume_max": 0.9,
  "pitch_min": 0.8,
  "pitch_max": 1.2,
  "properties_set": 4
}
```

## Examples

### Set looping on a WavePlayer
```json
{
  "tool": "set_sound_cue_node_property",
  "arguments": { "asset_path": "/Game/MySoundCue", "node_index": 0, "looping": true }
}
```

### Set Modulator volume/pitch range
```json
{
  "tool": "set_sound_cue_node_property",
  "arguments": { "asset_path": "/Game/MySoundCue", "node_index": 1, "volume_min": 0.3, "volume_max": 0.9 }
}
```

### Set a node as root
```json
{
  "tool": "set_sound_cue_node_property",
  "arguments": { "asset_path": "/Game/MySoundCue", "node_index": 2, "set_as_root": true }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"Missing required parameter 'asset_path'"` |
| Missing `node_index` | `"Missing required parameter 'node_index'"` |
| Index out of range | `"node_index <n> out of range (0..<max>)"` |
| No properties set | `"No valid properties set for node type '<class>'"` |
| Sound Wave not found | `"SoundWave not found: '<path>'"` |
| Sound Cue not found | `"Sound Cue not found: '<path>'"` |
