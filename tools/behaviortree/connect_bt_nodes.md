# connect_bt_nodes

**Category:** Behavior Tree  
**Source:** `Private/BehaviorTree/MCPBehaviorTreeTools_Edit.cpp`

## Description

Moves a Behavior Tree node to a new parent composite (re-parent). The node is removed from its current parent and added to the target composite at the specified index. The root node cannot be moved.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Behavior Tree |
| `node_path` | string | **Yes** | | Path to the node to move (e.g. `0`, `0.1`) |
| `new_parent_path` | string | **Yes** | | Path to the new parent composite (e.g. `root`, `1`) |
| `child_index` | integer | No | end | Insert position in the new parent |

## Response

```json
{
  "success": true,
  "old_path": "1",
  "new_parent_path": "0",
  "new_index": 0
}
```

## Examples

### Move task from root to a child composite
```json
{
  "tool": "connect_bt_nodes",
  "arguments": {
    "asset_path": "/Game/BT_EnemyAI",
    "node_path": "1",
    "new_parent_path": "0"
  }
}
```

### Move with explicit position
```json
{
  "tool": "connect_bt_nodes",
  "arguments": {
    "asset_path": "/Game/BT_EnemyAI",
    "node_path": "0.2",
    "new_parent_path": "root",
    "child_index": 0
  }
}
```

## Error Cases

- Missing `asset_path`, `node_path`, or `new_parent_path` returns an error
- Attempting to move root returns "Cannot move the root"
- Non-existent BT or node returns "not found"
- Target parent must be a composite node
