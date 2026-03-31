# create_sound_class

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_Lifecycle.cpp`

## Description

Creates a new Sound Class asset. Optionally sets initial volume and pitch on the class properties. The asset is saved to disk immediately.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the new Sound Class |
| `path` | string | No | `/Game` | Content path for the asset |
| `volume` | number | No | 1.0 | Default volume |
| `pitch` | number | No | 1.0 | Default pitch |

## Response

```json
{
  "name": "MySoundClass",
  "asset_path": "/Game/MySoundClass.MySoundClass",
  "type": "SoundClass",
  "volume": 1.0,
  "pitch": 1.0
}
```

## Examples

### Create a basic Sound Class
```json
{ "tool": "create_sound_class", "arguments": { "name": "SC_Music" } }
```

### Create with custom volume
```json
{
  "tool": "create_sound_class",
  "arguments": { "name": "SC_Ambient", "path": "/Game/Audio", "volume": 0.5 }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `name` | `"name is required"` |
| Asset already exists | `"Asset already exists: <path>"` |
