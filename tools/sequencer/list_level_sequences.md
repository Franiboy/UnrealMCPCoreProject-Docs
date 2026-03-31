# list_level_sequences

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Query.cpp`

## Description

Lists Level Sequence assets in the project. Returns name, asset path, and package path for each sequence. Supports filtering by path prefix, name substring, and engine/plugin inclusion.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `path_prefix` | string | No | `/Game` | Content path prefix to filter |
| `name_filter` | string | No | | Case-insensitive substring match on asset name |
| `include_engine` | boolean | No | `false` | Include engine/plugin assets |

## Response

```json
{
  "count": 3,
  "sequences": [
    {
      "name": "LS_Intro",
      "asset_path": "/Game/Cinematics/LS_Intro.LS_Intro",
      "package_path": "/Game/Cinematics"
    }
  ]
}
```

## Examples

### List all game sequences
```json
{ "tool": "list_level_sequences", "arguments": {} }
```

### Filter by name
```json
{ "tool": "list_level_sequences", "arguments": { "name_filter": "Intro" } }
```

### Include engine sequences
```json
{ "tool": "list_level_sequences", "arguments": { "include_engine": true } }
```
