# create_blackboard

**Category:** Blackboard  
**Source:** `Private/Blackboard/MCPBlackboardTools_Edit.cpp`

## Description

Creates a new Blackboard Data asset. Optionally sets a parent blackboard for key inheritance and saves the asset to disk.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the Blackboard asset |
| `path` | string | No | `/Game` | Content path where the asset is created |
| `parent` | string | No | | Asset path of a parent Blackboard for key inheritance |

## Response

```json
{
  "name": "BB_EnemyAI",
  "asset_path": "/Game/AI/BB_EnemyAI.BB_EnemyAI",
  "key_count": 0,
  "parent": "/Game/AI/BB_BaseAI.BB_BaseAI"
}
```

## Examples

### Create a basic blackboard
```json
{ "tool": "create_blackboard", "arguments": { "name": "BB_EnemyAI" } }
```

### Create with parent
```json
{ "tool": "create_blackboard", "arguments": { "name": "BB_BossAI", "parent": "/Game/AI/BB_EnemyAI" } }
```

### Create in a subfolder
```json
{ "tool": "create_blackboard", "arguments": { "name": "BB_NPC", "path": "/Game/AI/NPC" } }
```

## Error Cases

- `name is required` — missing or empty `name`
- `Blackboard already exists: <path>` — asset with same name exists at the target path
- `Parent blackboard not found: <path>` — parent path does not point to a valid `UBlackboardData`
