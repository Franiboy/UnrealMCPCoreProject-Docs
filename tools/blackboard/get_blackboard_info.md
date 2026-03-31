# get_blackboard_info

**Category:** Blackboard  
**Source:** `Private/Blackboard/MCPBlackboardTools_Query.cpp`

## Description

Inspects a Blackboard Data asset. Returns full detail on all keys (name, type, index, instance synced, description, category), the parent chain, inherited parent keys, total key count, and synchronisation flag.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Blackboard Data asset |

## Response

```json
{
  "name": "BB_EnemyAI",
  "asset_path": "/Game/AI/BB_EnemyAI.BB_EnemyAI",
  "parent": "/Game/AI/BB_BaseAI.BB_BaseAI",
  "parent_chain": ["/Game/AI/BB_BaseAI.BB_BaseAI"],
  "keys": [
    {
      "name": "TargetActor",
      "type": "Object",
      "index": 0,
      "instance_synced": false,
      "base_class": "Actor"
    },
    {
      "name": "IsAlerted",
      "type": "Bool",
      "index": 1,
      "instance_synced": true,
      "description": "Whether the AI is alerted"
    }
  ],
  "key_count": 2,
  "parent_keys": [],
  "inherited_key_count": 0,
  "total_key_count": 3,
  "has_synchronized_keys": true
}
```

## Examples

### Inspect a blackboard
```json
{ "tool": "get_blackboard_info", "arguments": { "asset_path": "/Game/AI/BB_EnemyAI" } }
```

## Error Cases

- `asset_path is required` — missing or empty `asset_path`
- `Blackboard not found: <path>` — asset does not exist or is not a `UBlackboardData`
