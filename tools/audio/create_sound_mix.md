# create_sound_mix

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_Lifecycle.cpp`

## Description

Creates a new Sound Mix asset. The asset is saved to disk immediately. Sound Mix settings (EQ, sound class overrides) can be configured later via editing tools.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the new Sound Mix |
| `path` | string | No | `/Game` | Content path for the asset |

## Response

```json
{
  "name": "MySoundMix",
  "asset_path": "/Game/MySoundMix.MySoundMix",
  "type": "SoundMix"
}
```

## Examples

### Create a basic Sound Mix
```json
{ "tool": "create_sound_mix", "arguments": { "name": "Mix_Combat" } }
```

### Create in a subfolder
```json
{ "tool": "create_sound_mix", "arguments": { "name": "Mix_Menu", "path": "/Game/Audio/Mixes" } }
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `name` | `"name is required"` |
| Asset already exists | `"Asset already exists: <path>"` |
