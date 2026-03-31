# set_state_tree_task

**Category:** State Tree  
**Source:** `Private/StateTree/MCPStateTreeTools_Edit.cpp`

## Description

Sets or configures a task on a state in a State Tree. Can add a new task by struct class name, or modify an existing task by index. Supports setting properties via reflection and enabling/disabling tasks.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the State Tree |
| `state_name` | string | **Yes** | | Name of the state to set the task on |
| `task_class` | string | No | | Task struct class name to add (e.g. `StateTreeDelayTask`). Prefix `FStateTree` is optional. |
| `task_index` | integer | No | | Index of existing task to modify (if omitted and `task_class` given, adds new task) |
| `properties` | object | No | | Key-value pairs to set on the task via reflection |
| `enabled` | boolean | No | `true` | Whether the task is enabled |

## Response

```json
{
  "state_name": "Idle",
  "task_type": "FStateTreeDelayTask",
  "task_index": 0,
  "total_tasks": 1,
  "enabled": true
}
```

## Examples

### Add a delay task
```json
{
  "tool": "set_state_tree_task",
  "arguments": {
    "asset_path": "/Game/AI/ST_Enemy",
    "state_name": "Idle",
    "task_class": "StateTreeDelayTask"
  }
}
```

### Modify an existing task's enabled state
```json
{
  "tool": "set_state_tree_task",
  "arguments": {
    "asset_path": "/Game/AI/ST_Enemy",
    "state_name": "Idle",
    "task_index": 0,
    "enabled": false
  }
}
```

## Error Cases

- `asset_path` and `state_name` are required
- Either `task_class` (to add new) or `task_index` (to modify existing) must be provided
- Returns error if the state tree, state, or task class is not found
- Returns error if `task_index` is out of range
