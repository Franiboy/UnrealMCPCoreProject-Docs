# list_gameplay_effects

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_Query.cpp`

## Description

Lists Gameplay Effect Blueprint assets in the project. Scans for Blueprints whose generated class derives from `UGameplayEffect`. Returns summary info including duration policy, modifier count, stacking type, and tags.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `path_prefix` | string | No | `/Game` | Content path prefix to filter |
| `name_filter` | string | No | | Case-insensitive substring match on asset name |
| `include_engine` | boolean | No | `false` | Include engine/plugin content |

## Response

```json
{
  "count": 2,
  "effects": [
    {
      "name": "GE_Damage",
      "asset_path": "/Game/Effects/GE_Damage.GE_Damage",
      "duration_policy": "Instant",
      "modifier_count": 1,
      "stacking_type": "None",
      "asset_tags": ["Effect.Damage"],
      "granted_tags": []
    }
  ]
}
```

## Examples

### List all effects
```json
{ "tool": "list_gameplay_effects", "arguments": {} }
```

### Filter by name
```json
{ "tool": "list_gameplay_effects", "arguments": { "name_filter": "Damage" } }
```

## Error Cases

No errors — returns `count: 0` with empty array if no effects found.
