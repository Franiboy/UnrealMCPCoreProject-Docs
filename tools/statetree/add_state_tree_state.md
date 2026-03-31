# add_state_tree_state

**Category:** State Tree  
**Source:** `Private/StateTree/MCPStateTreeTools_Edit.cpp`

## Description

Adds a state to a State Tree. Can add as a root-level state or as a child of an existing state. Supports state types: State, Group, Linked, LinkedAsset, Subtree.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the State Tree |
| `state_name` | string | **Yes** | | Name of the state to add |
| `parent_state` | string | No | | Name of the parent state (omit for root-level) |
| `state_type` | string | No | `State` | State type: `State`, `Group`, `Linked`, `LinkedAsset`, `Subtree` |
| `selection_behavior` | string | No | | Selection behavior: `None`, `TryEnterState`, `TrySelectChildrenInOrder`, `TrySelectChildrenAtRandom`, `TryFollowTransitions` |
| `enabled` | boolean | No | `true` | Whether the state is enabled |

## Response

```json
{
  "state_name": "Patrol",
  "state_id": "...",
  "state_type": "State",
  "parent_state": "Root",
  "enabled": true,
  "total_states": 3
}
```

## Examples

### Add a root-level state
```json
{ "tool": "add_state_tree_state", "arguments": { "asset_path": "/Game/AI/ST_Enemy", "state_name": "Idle" } }
```

### Add a child state
```json
{ "tool": "add_state_tree_state", "arguments": { "asset_path": "/Game/AI/ST_Enemy", "state_name": "Patrol", "parent_state": "Idle" } }
```

### Add a Group state
```json
{ "tool": "add_state_tree_state", "arguments": { "asset_path": "/Game/AI/ST_Enemy", "state_name": "Combat", "state_type": "Group" } }
```

## Error Cases

- `asset_path` and `state_name` are required
- Returns error if the state tree or parent state is not found
- Returns error if a state with the same name already exists
- Returns error if `state_type` is invalid
