# list_state_trees

**Category:** State Tree  
**Source:** `Private/StateTree/MCPStateTreeTools_Query.cpp`

## Description

Lists State Tree assets in the project. Returns name, asset path, schema, and state count for each tree. Supports filtering by path prefix, name substring, and whether to include engine/plugin content.

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
  "state_trees": [
    {
      "name": "ST_EnemyAI",
      "asset_path": "/Game/AI/ST_EnemyAI.ST_EnemyAI",
      "schema": "StateTreeComponentSchema",
      "state_count": 5
    }
  ]
}
```

## Examples

### List all game state trees
```json
{ "tool": "list_state_trees", "arguments": {} }
```

### Filter by name
```json
{ "tool": "list_state_trees", "arguments": { "name_filter": "Enemy" } }
```

## Error Cases

- No parameters are required; an empty call lists all `/Game` state trees
- Returns `count: 0` with empty array when no matches are found
