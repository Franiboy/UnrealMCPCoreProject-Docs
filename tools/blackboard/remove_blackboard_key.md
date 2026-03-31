# remove_blackboard_key

**Category:** Blackboard  
**Source:** `Private/Blackboard/MCPBlackboardTools_Edit.cpp`

## Description

Removes a key from a Blackboard Data asset by name. Only keys owned by this blackboard (not inherited parent keys) can be removed.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Blackboard |
| `key_name` | string | **Yes** | | Name of the key to remove |

## Response

```json
{
  "success": true,
  "removed_key": "Health",
  "removed_type": "Float",
  "remaining_keys": 2
}
```

## Examples

### Remove a key
```json
{ "tool": "remove_blackboard_key", "arguments": { "asset_path": "/Game/BB_EnemyAI", "key_name": "Health" } }
```

## Error Cases

- `asset_path is required` — missing or empty `asset_path`
- `key_name is required` — missing or empty `key_name`
- `Key not found: <name>` — no key with that name exists in the blackboard's own keys
- `Blackboard not found: <path>` — asset does not exist or is not a `UBlackboardData`
