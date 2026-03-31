# list_behavior_trees

**Category:** Behavior Tree  
**Source:** `Private/BehaviorTree/MCPBehaviorTreeTools_Query.cpp`

## Description

Lists Behavior Tree assets in the project. Returns name, asset path, and blackboard info for each tree. Supports filtering by path prefix, name substring, and whether to include engine/plugin content.

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
  "behavior_trees": [
    {
      "name": "BT_EnemyAI",
      "asset_path": "/Game/AI/BT_EnemyAI",
      "blackboard": "BB_EnemyAI"
    }
  ]
}
```

## Examples

### List all game BTs
```json
{ "tool": "list_behavior_trees", "arguments": {} }
```

### Filter by name
```json
{ "tool": "list_behavior_trees", "arguments": { "name_filter": "Enemy" } }
```

## Error Cases

- No parameters are required; an empty call lists all `/Game` BTs
- Returns `count: 0` with empty array when no matches are found
