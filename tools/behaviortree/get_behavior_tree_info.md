# get_behavior_tree_info

**Category:** Behavior Tree  
**Source:** `Private/BehaviorTree/MCPBehaviorTreeTools_Query.cpp`

## Description

Inspects a Behavior Tree asset: returns its root node hierarchy, tasks, decorators, services, blackboard association, and node counts. Supports limiting traversal depth.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Behavior Tree |
| `max_depth` | integer | No | unlimited | Max depth to traverse |

## Response

```json
{
  "name": "BT_EnemyAI",
  "asset_path": "/Game/AI/BT_EnemyAI",
  "blackboard": "BB_EnemyAI",
  "root_node": {
    "type": "Composite",
    "class": "Selector",
    "path": "root",
    "children": [ ... ]
  },
  "node_counts": {
    "composites": 3,
    "tasks": 5,
    "decorators": 2,
    "services": 1
  }
}
```

## Examples

### Inspect a BT
```json
{ "tool": "get_behavior_tree_info", "arguments": { "asset_path": "/Game/AI/BT_EnemyAI" } }
```

### Shallow inspection (root only)
```json
{ "tool": "get_behavior_tree_info", "arguments": { "asset_path": "/Game/AI/BT_EnemyAI", "max_depth": 0 } }
```

## Error Cases

- Missing `asset_path` returns an error
- Non-existent asset returns "not found"
