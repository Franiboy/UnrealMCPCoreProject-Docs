# get_state_tree_info

**Category:** State Tree  
**Source:** `Private/StateTree/MCPStateTreeTools_Query.cpp`

## Description

Inspects a State Tree asset in detail. Returns the full state hierarchy including each state's name, type, tasks, transitions, enter conditions, and children. Also includes evaluators, global tasks, and schema information.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the State Tree |

## Response

```json
{
  "name": "ST_EnemyAI",
  "asset_path": "/Game/AI/ST_EnemyAI.ST_EnemyAI",
  "schema": "StateTreeComponentSchema",
  "state_count": 3,
  "states": [
    {
      "name": "Root",
      "id": "...",
      "type": "State",
      "selection_behavior": "TrySelectChildrenInOrder",
      "enabled": true,
      "tasks": [],
      "enter_conditions": [],
      "transitions": [],
      "children": [
        {
          "name": "Patrol",
          "id": "...",
          "type": "State",
          "tasks": [{ "index": 0, "id": "...", "type": "StateTreeDelayTask", "enabled": true }],
          "transitions": [{ "index": 0, "id": "...", "trigger": "OnStateCompleted", "enabled": true }],
          "children": []
        }
      ]
    }
  ],
  "evaluators": [],
  "global_tasks": []
}
```

## Examples

### Inspect a state tree
```json
{ "tool": "get_state_tree_info", "arguments": { "asset_path": "/Game/AI/ST_EnemyAI" } }
```

## Error Cases

- `asset_path` is required
- Returns error if the asset is not found or has no editor data
