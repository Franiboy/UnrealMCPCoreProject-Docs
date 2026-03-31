# list_smart_object_definitions

**Category:** Smart Object  
**Source:** `Private/SmartObject/MCPSmartObjectTools_Query.cpp`

## Description

Lists Smart Object Definition assets in the project. Returns name, asset path, slot count, and activity tags for each definition. Supports filtering by path prefix, name substring, and whether to include engine/plugin content.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `path_prefix` | string | No | `/Game` | Content path prefix to filter |
| `name_filter` | string | No | | Filter by name (case-insensitive substring match) |
| `include_engine` | boolean | No | `false` | Include engine/plugin content |

## Response

```json
{
  "count": 2,
  "definitions": [
    {
      "name": "SO_Bench",
      "asset_path": "/Game/SmartObjects/SO_Bench.SO_Bench",
      "slot_count": 3,
      "activity_tags": ["SmartObject.Activity.Sit"]
    }
  ]
}
```

## Examples

### List all game Smart Object Definitions
```json
{ "tool": "list_smart_object_definitions", "arguments": {} }
```

### Filter by name
```json
{ "tool": "list_smart_object_definitions", "arguments": { "name_filter": "Bench" } }
```

### Filter by path
```json
{ "tool": "list_smart_object_definitions", "arguments": { "path_prefix": "/Game/SmartObjects" } }
```

## Error Cases

- No parameters are required; an empty call lists all `/Game` Smart Object Definitions
- Returns `count: 0` with empty array when no matches are found
