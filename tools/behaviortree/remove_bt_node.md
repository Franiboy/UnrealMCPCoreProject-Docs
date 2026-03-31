# remove_bt_node

**Category:** Behavior Tree  
**Source:** `Private/BehaviorTree/MCPBehaviorTreeTools_Edit.cpp`

## Description

Removes a node from a Behavior Tree by its path. The root node cannot be removed. Child nodes of the removed node are also removed.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Behavior Tree |
| `node_path` | string | **Yes** | | Path to the node to remove (e.g. `0`, `0.1`). Cannot be `root`. |

## Response

```json
{
  "success": true,
  "removed_path": "1",
  "removed_type": "Task"
}
```

## Examples

### Remove second child of root
```json
{ "tool": "remove_bt_node", "arguments": { "asset_path": "/Game/BT_EnemyAI", "node_path": "1" } }
```

### Remove nested child
```json
{ "tool": "remove_bt_node", "arguments": { "asset_path": "/Game/BT_EnemyAI", "node_path": "0.2" } }
```

## Error Cases

- Missing `asset_path` or `node_path` returns an error
- Attempting to remove root returns "Cannot remove the root"
- Non-existent BT or node returns "not found"
