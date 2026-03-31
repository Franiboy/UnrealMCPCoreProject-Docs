# remove_state_tree_state

**Category:** State Tree  
**Source:** `Private/StateTree/MCPStateTreeTools_Edit.cpp`

## Description

Removes a state from a State Tree by name. Also removes all child states of the removed state.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the State Tree |
| `state_name` | string | **Yes** | | Name of the state to remove |

## Response

```json
{
  "success": true,
  "removed_state": "Patrol",
  "removed_type": "State",
  "remaining_states": 2
}
```

## Examples

### Remove a state
```json
{ "tool": "remove_state_tree_state", "arguments": { "asset_path": "/Game/AI/ST_Enemy", "state_name": "Patrol" } }
```

## Error Cases

- `asset_path` and `state_name` are required
- Returns error if the state tree is not found
- Returns error if the state is not found
