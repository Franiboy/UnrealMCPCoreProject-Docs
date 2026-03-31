# create_metasound

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_Lifecycle.cpp`

## Description

Creates a new MetaSound Source asset with a default graph. Uses the MetaSound factory for proper initialization of the document, interfaces, and graph structure. The asset is saved to disk immediately.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the new MetaSound Source |
| `path` | string | No | `/Game` | Content path for the asset |

## Response

```json
{
  "name": "MyMetaSound",
  "asset_path": "/Game/MyMetaSound.MyMetaSound",
  "type": "MetaSoundSource"
}
```

## Examples

### Create a basic MetaSound
```json
{ "tool": "create_metasound", "arguments": { "name": "MyMetaSound" } }
```

### Create in a subfolder
```json
{ "tool": "create_metasound", "arguments": { "name": "AmbientDrone", "path": "/Game/Audio/MetaSounds" } }
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `name` | `"name is required"` |
| Asset already exists | `"Asset already exists: <path>"` |
| Factory failure | `"MetaSound factory returned null — creation failed"` |
