# add_bt_node

**Category:** Behavior Tree  
**Source:** `Private/BehaviorTree/MCPBehaviorTreeTools_Edit.cpp`

## Description

Adds a node to a Behavior Tree. Supports Composite (Sequence, Selector, SimpleParallel), Task, Decorator, and Service node types. Nodes are addressed by path notation: `root` for the root composite, `0` for first child of root, `0.1` for second child of root's first child, etc.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Behavior Tree |
| `parent_path` | string | **Yes** | | Path to parent composite (e.g. `root`, `0`, `0.1`) |
| `node_type` | string | **Yes** | | Node type: `Composite`, `Task`, `Decorator`, `Service` |
| `node_class` | string | **Yes** | | Class name (e.g. `Sequence`, `BTTask_Wait`, `BTDecorator_Blackboard`) |
| `child_index` | integer | No | end | Insert position. For Decorators, the child index to attach to |
| `node_name` | string | No | | Custom display name for the node |

## Response

```json
{
  "node_type": "Composite",
  "node_class": "BTComposite_Sequence",
  "node_path": "0",
  "node_name": ""
}
```

## Examples

### Add a Sequence composite under root
```json
{
  "tool": "add_bt_node",
  "arguments": {
    "asset_path": "/Game/BT_EnemyAI",
    "parent_path": "root",
    "node_type": "Composite",
    "node_class": "Sequence"
  }
}
```

### Add a wait task with custom name
```json
{
  "tool": "add_bt_node",
  "arguments": {
    "asset_path": "/Game/BT_EnemyAI",
    "parent_path": "0",
    "node_type": "Task",
    "node_class": "BTTask_Wait",
    "node_name": "WaitForPlayer"
  }
}
```

## Error Cases

- Missing `asset_path`, `parent_path`, `node_type`, or `node_class` returns an error
- Non-existent BT or parent returns "not found"
- Invalid `node_class` returns an error listing the class search
