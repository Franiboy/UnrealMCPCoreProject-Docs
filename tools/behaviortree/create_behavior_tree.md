# create_behavior_tree

**Category:** Behavior Tree  
**Source:** `Private/BehaviorTree/MCPBehaviorTreeTools_Edit.cpp`

## Description

Creates a new Behavior Tree asset with a root composite node. Supports choosing the root composite type and optionally associating a Blackboard asset.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the Behavior Tree asset |
| `path` | string | No | `/Game` | Content path to create the asset in |
| `root_type` | string | No | `Selector` | Root composite type: `Selector`, `Sequence`, `SimpleParallel` |
| `blackboard_asset` | string | No | | Blackboard asset path to associate |

## Response

```json
{
  "name": "BT_EnemyAI",
  "asset_path": "/Game/AI/BT_EnemyAI",
  "root_type": "Selector"
}
```

## Examples

### Create with default root (Selector)
```json
{ "tool": "create_behavior_tree", "arguments": { "name": "BT_EnemyAI" } }
```

### Create with Sequence root in subfolder
```json
{
  "tool": "create_behavior_tree",
  "arguments": {
    "name": "BT_PatrolAI",
    "path": "/Game/AI",
    "root_type": "Sequence"
  }
}
```

## Error Cases

- Missing `name` returns an error
- Duplicate name at the same path returns "already exists"
- Invalid `root_type` returns "Invalid root_type"
