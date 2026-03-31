# list_audio_assets

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_Query.cpp`

## Description

Lists audio assets in the project. Supports filtering by asset type (SoundWave, SoundCue, MetaSoundSource, SoundClass, SoundMix, SoundAttenuation), name substring, and content path prefix. SoundWave assets include lightweight metadata (channels, sample rate, duration) from the Asset Registry without loading the asset.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `type_filter` | string | No | (all types) | Filter by audio type: `SoundWave`, `SoundCue`, `MetaSoundSource`, `SoundClass`, `SoundMix`, `SoundAttenuation` |
| `path_prefix` | string | No | `/Game` | Content path prefix to filter |
| `name_filter` | string | No | | Case-insensitive substring filter on asset name |
| `include_engine` | boolean | No | `false` | Include engine/plugin content |

## Response

```json
{
  "count": 14,
  "type_filter": "SoundWave",
  "assets": [
    {
      "name": "Explosion01",
      "path": "/Game/Audio/Explosion01.Explosion01",
      "type": "SoundWave",
      "num_channels": 1,
      "sample_rate": 44100,
      "duration": 3.17
    }
  ]
}
```

## Examples

### List all audio assets
```json
{ "tool": "list_audio_assets", "arguments": {} }
```

### List only Sound Cues
```json
{ "tool": "list_audio_assets", "arguments": { "type_filter": "SoundCue" } }
```

### Search by name
```json
{ "tool": "list_audio_assets", "arguments": { "name_filter": "Explosion" } }
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Unknown `type_filter` value | `"Unknown type_filter '<value>'. Valid types: SoundWave, SoundCue, MetaSoundSource, SoundClass, SoundMix, SoundAttenuation"` |
