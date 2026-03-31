# set_blackboard_key_properties

**Category:** Blackboard  
**Source:** `Private/Blackboard/MCPBlackboardTools_Edit.cpp`

## Description

Modifies properties of an existing key in a Blackboard Data asset. Can change the instance synced flag, description, and editor category. At least one property must be specified.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Blackboard |
| `key_name` | string | **Yes** | | Name of the key to modify |
| `instance_synced` | boolean | No | | Set the instance synced flag |
| `description` | string | No | | Set the description/tooltip |
| `category` | string | No | | Set the editor category |

## Response

```json
{
  "key_name": "Health",
  "changed_properties": ["instance_synced", "description"],
  "instance_synced": true,
  "description": "Current health points",
  "category": "Combat"
}
```

## Examples

### Set instance synced
```json
{ "tool": "set_blackboard_key_properties", "arguments": { "asset_path": "/Game/BB_EnemyAI", "key_name": "Health", "instance_synced": true } }
```

### Set description and category
```json
{ "tool": "set_blackboard_key_properties", "arguments": { "asset_path": "/Game/BB_EnemyAI", "key_name": "Health", "description": "Current HP", "category": "Combat" } }
```

## Error Cases

- `asset_path is required` — missing or empty `asset_path`
- `key_name is required` — missing or empty `key_name`
- `No properties specified to change` — none of `instance_synced`, `description`, `category` provided
- `Key not found: <name>` — no key with that name in the blackboard's own keys
- `Blackboard not found: <path>` — asset does not exist or is not a `UBlackboardData`
