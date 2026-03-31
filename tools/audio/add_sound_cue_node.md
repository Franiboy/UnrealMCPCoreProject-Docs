# add_sound_cue_node

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_SoundCueEdit.cpp`

## Description

Adds a node to a Sound Cue graph. Supports 10 node types: WavePlayer, Random, Mixer, Modulator, Attenuation, Delay, Concatenator, Switch, Looping, and DistanceCrossFade. Optionally sets type-specific properties at creation time and can designate the new node as root.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Sound Cue |
| `node_type` | string | **Yes** | | Node type (see Supported Types below) |
| `set_as_root` | boolean | No | `false` | Set this node as the root/first node |
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
| `int_parameter_name` | string | No | | (Switch) Integer parameter name |

### Supported Node Types

| Type | UE5 Class | Description |
|------|-----------|-------------|
| `WavePlayer` | `USoundNodeWavePlayer` | Plays a Sound Wave |
| `Random` | `USoundNodeRandom` | Randomly picks a child |
| `Mixer` | `USoundNodeMixer` | Mixes multiple children |
| `Modulator` | `USoundNodeModulator` | Randomises volume/pitch |
| `Attenuation` | `USoundNodeAttenuation` | Distance-based attenuation |
| `Delay` | `USoundNodeDelay` | Delays playback |
| `Concatenator` | `USoundNodeConcatenator` | Plays children sequentially |
| `Switch` | `USoundNodeSwitch` | Selects child by integer parameter |
| `Looping` | `USoundNodeLooping` | Loops its child |
| `DistanceCrossFade` | `USoundNodeDistanceCrossFade` | Cross-fades by distance |

## Response

```json
{
  "index": 0,
  "class": "SoundNodeWavePlayer",
  "name": "SoundNodeWavePlayer_0",
  "is_root": false,
  "asset_path": "/Game/MySoundCue.MySoundCue",
  "total_nodes": 1,
  "looping": false
}
```

## Examples

### Add a WavePlayer node
```json
{ "tool": "add_sound_cue_node", "arguments": { "asset_path": "/Game/MySoundCue", "node_type": "WavePlayer" } }
```

### Add a Mixer as root
```json
{
  "tool": "add_sound_cue_node",
  "arguments": { "asset_path": "/Game/MySoundCue", "node_type": "Mixer", "set_as_root": true }
}
```

### Add a Delay with timing
```json
{
  "tool": "add_sound_cue_node",
  "arguments": { "asset_path": "/Game/MySoundCue", "node_type": "Delay", "delay_min": 0.5, "delay_max": 2.0 }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"Missing required parameter 'asset_path'"` |
| Missing `node_type` | `"Missing required parameter 'node_type'"` |
| Unknown node type | `"Unknown node_type '<type>'. Valid types: ..."` |
| Sound Cue not found | `"Sound Cue not found: '<path>'"` |
