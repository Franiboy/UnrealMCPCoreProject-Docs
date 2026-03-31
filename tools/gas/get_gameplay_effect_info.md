# get_gameplay_effect_info

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_Query.cpp`

## Description

Inspects a Gameplay Effect Blueprint in detail. Returns duration/stacking config, full modifier list with attributes and magnitudes, tag containers, execution and cue counts, and UE5.3+ component list.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Gameplay Effect Blueprint |

## Response

```json
{
  "name": "GE_Damage",
  "asset_path": "/Game/Effects/GE_Damage.GE_Damage",
  "parent_class": "GameplayEffect",
  "duration_policy": "Instant",
  "period": 0.0,
  "execute_periodic_on_application": false,
  "stacking_type": "None",
  "asset_tags": [],
  "granted_tags": [],
  "blocked_ability_tags": [],
  "modifiers": [
    {
      "index": 0,
      "attribute": "MyAttributeSet.Health",
      "modifier_op": "Additive",
      "magnitude_type": "ScalableFloat",
      "magnitude_value": -25.0
    }
  ],
  "modifier_count": 1,
  "execution_count": 0,
  "gameplay_cue_count": 0,
  "components": ["AssetTagsGameplayEffectComponent"],
  "component_count": 1
}
```

When `stacking_type` is not `None`, `stack_limit_count` is also included.

## Examples

### Inspect an effect
```json
{ "tool": "get_gameplay_effect_info", "arguments": { "asset_path": "/Game/Effects/GE_Damage" } }
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"asset_path is required"` |
| Asset not found | `"Gameplay Effect Blueprint not found: <path>"` |
