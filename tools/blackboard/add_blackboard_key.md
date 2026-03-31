# add_blackboard_key

**Category:** Blackboard  
**Source:** `Private/Blackboard/MCPBlackboardTools_Edit.cpp`

## Description

Adds a new key to a Blackboard Data asset. Supports 10 key types (Bool, Int, Float, String, Name, Vector, Rotator, Enum, Object, Class) with optional type-specific configuration such as base class for Object/Class keys and enum type for Enum keys.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Blackboard |
| `key_name` | string | **Yes** | | Name of the key to add |
| `key_type` | string | **Yes** | | Key type: `Bool`, `Int`, `Float`, `String`, `Name`, `Vector`, `Rotator`, `Enum`, `Object`, `Class` |
| `instance_synced` | boolean | No | `false` | Whether the key is instance synced |
| `description` | string | No | | Description/tooltip for the key |
| `category` | string | No | | Category for editor organization |
| `base_class` | string | No | | Base class for Object/Class keys (e.g. `Actor`, `Pawn`) |
| `enum_type` | string | No | | Enum type name for Enum keys (e.g. `EAIState`) |

## Response

```json
{
  "key_name": "TargetActor",
  "key_type": "Object",
  "instance_synced": false,
  "key_index": 1,
  "total_keys": 2
}
```

## Examples

### Add a Bool key
```json
{ "tool": "add_blackboard_key", "arguments": { "asset_path": "/Game/BB_EnemyAI", "key_name": "IsAlerted", "key_type": "Bool" } }
```

### Add an Object key with base class
```json
{ "tool": "add_blackboard_key", "arguments": { "asset_path": "/Game/BB_EnemyAI", "key_name": "TargetActor", "key_type": "Object", "base_class": "Actor" } }
```

### Add a synced key with description
```json
{ "tool": "add_blackboard_key", "arguments": { "asset_path": "/Game/BB_EnemyAI", "key_name": "Health", "key_type": "Float", "instance_synced": true, "description": "Current health" } }
```

## Error Cases

- `asset_path is required` — missing or empty `asset_path`
- `key_name is required` — missing or empty `key_name`
- `key_type is required` — missing or empty `key_type`
- `Key already exists: <name>` — duplicate key name in the blackboard
- `Invalid key_type: <type>` — unrecognised type string
- `Blackboard not found: <path>` — asset does not exist or is not a `UBlackboardData`
