# create_sound_cue

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_Lifecycle.cpp`

## Description

Creates a new Sound Cue asset. Optionally sets initial volume and pitch multipliers. The asset is saved to disk immediately.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the new Sound Cue |
| `path` | string | No | `/Game` | Content path for the asset |
| `volume_multiplier` | number | No | 0.75 | Volume multiplier |
| `pitch_multiplier` | number | No | 1.0 | Pitch multiplier |

## Response

```json
{
  "name": "MySoundCue",
  "asset_path": "/Game/MySoundCue.MySoundCue",
  "type": "SoundCue",
  "volume_multiplier": 0.75,
  "pitch_multiplier": 1.0
}
```

## Examples

### Create a basic Sound Cue
```json
{ "tool": "create_sound_cue", "arguments": { "name": "MySoundCue" } }
```

### Create with custom volume/pitch
```json
{
  "tool": "create_sound_cue",
  "arguments": { "name": "QuietCue", "path": "/Game/Audio", "volume_multiplier": 0.3, "pitch_multiplier": 1.2 }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `name` | `"name is required"` |
| Asset already exists | `"Asset already exists: <path>"` |
