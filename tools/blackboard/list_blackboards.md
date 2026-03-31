# list_blackboards

**Category:** Blackboard  
**Source:** `Private/Blackboard/MCPBlackboardTools_Query.cpp`

## Description

Lists Blackboard Data assets in the project. Returns name, asset path, key count, and parent info for each blackboard. Supports filtering by path prefix, name substring, and whether to include engine/plugin content.

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
  "blackboards": [
    {
      "name": "BB_EnemyAI",
      "asset_path": "/Game/AI/BB_EnemyAI.BB_EnemyAI",
      "key_count": 5,
      "parent": "/Game/AI/BB_BaseAI.BB_BaseAI"
    }
  ]
}
```

## Examples

### List all game blackboards
```json
{ "tool": "list_blackboards", "arguments": {} }
```

### Filter by name
```json
{ "tool": "list_blackboards", "arguments": { "name_filter": "Enemy" } }
```

## Error Cases

- No parameters are required; an empty call lists all `/Game` blackboards
- Returns `count: 0` with empty array when no matches are found
