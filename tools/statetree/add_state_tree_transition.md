# add_state_tree_transition

**Category:** State Tree  
**Source:** `Private/StateTree/MCPStateTreeTools_Edit.cpp`

## Description

Adds a transition to a state in a State Tree. Supports trigger types: OnStateCompleted, OnStateSucceeded, OnStateFailed, OnTick, OnEvent. Target can be a specific state name or a built-in target (Succeeded, Failed, Next, NextSelectable).

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the State Tree |
| `source_state` | string | **Yes** | | Name of the source state |
| `trigger` | string | **Yes** | | Trigger type: `OnStateCompleted`, `OnStateSucceeded`, `OnStateFailed`, `OnTick`, `OnEvent` |
| `target_state` | string | **Yes** | | Target: state name, or `Succeeded`, `Failed`, `Next`, `NextSelectable` |
| `priority` | string | No | `Normal` | Priority: `Normal`, `Medium`, `High`, `Critical` |
| `delay` | number | No | `0` | Delay in seconds before the transition fires |
| `enabled` | boolean | No | `true` | Whether the transition is enabled |

## Response

```json
{
  "source_state": "Idle",
  "trigger": "OnStateCompleted",
  "target_state": "Combat",
  "transition_id": "...",
  "enabled": true,
  "transition_count": 1
}
```

## Examples

### Transition to another state on completion
```json
{
  "tool": "add_state_tree_transition",
  "arguments": {
    "asset_path": "/Game/AI/ST_Enemy",
    "source_state": "Idle",
    "trigger": "OnStateCompleted",
    "target_state": "Patrol"
  }
}
```

### Mark tree as succeeded on state success
```json
{
  "tool": "add_state_tree_transition",
  "arguments": {
    "asset_path": "/Game/AI/ST_Enemy",
    "source_state": "Combat",
    "trigger": "OnStateSucceeded",
    "target_state": "Succeeded"
  }
}
```

### High-priority tick transition with delay
```json
{
  "tool": "add_state_tree_transition",
  "arguments": {
    "asset_path": "/Game/AI/ST_Enemy",
    "source_state": "Idle",
    "trigger": "OnTick",
    "target_state": "Combat",
    "priority": "High",
    "delay": 2.5
  }
}
```

## Error Cases

- `asset_path`, `source_state`, `trigger`, and `target_state` are required
- Returns error if the state tree, source state, or target state is not found
- Returns error if the trigger type is invalid
