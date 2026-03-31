# set_bt_node_property

**Category:** Behavior Tree  
**Source:** `Private/BehaviorTree/MCPBehaviorTreeTools_Edit.cpp`

## Description

Sets a property on a Behavior Tree node. The value is provided as a string and parsed to match the property type. Works on any node type (composite, task, decorator, service).

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Behavior Tree |
| `node_path` | string | **Yes** | | Path to the node (e.g. `root`, `0`, `0.1`) |
| `property_name` | string | **Yes** | | Property name (e.g. `NodeName`, `WaitTime`, `FlowAbortMode`) |
| `value` | string | **Yes** | | Value to set (as string, parsed to match property type) |

## Response

```json
{
  "property_name": "NodeName",
  "new_value": "MainSequence",
  "node_path": "root"
}
```

## Examples

### Set node name on root
```json
{
  "tool": "set_bt_node_property",
  "arguments": {
    "asset_path": "/Game/BT_EnemyAI",
    "node_path": "root",
    "property_name": "NodeName",
    "value": "MainSelector"
  }
}
```

### Set wait time on a task
```json
{
  "tool": "set_bt_node_property",
  "arguments": {
    "asset_path": "/Game/BT_EnemyAI",
    "node_path": "0.1",
    "property_name": "WaitTime",
    "value": "5.0"
  }
}
```

## Error Cases

- Missing `asset_path`, `node_path`, `property_name`, or `value` returns an error
- Non-existent property returns "not found"
- Non-existent BT or node returns "not found"
