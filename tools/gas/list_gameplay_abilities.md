# list_gameplay_abilities

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_Query.cpp`

## Description

Lists Gameplay Ability Blueprint assets in the project. Scans for Blueprints whose generated class derives from `UGameplayAbility`. Returns summary info including tags, instancing policy, and net execution policy for each ability.

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
  "abilities": [
    {
      "name": "GA_FireBall",
      "asset_path": "/Game/Abilities/GA_FireBall.GA_FireBall",
      "ability_tags": ["Ability.Attack.Fire"],
      "instancing_policy": "InstancedPerActor",
      "net_execution_policy": "LocalPredicted",
      "cooldown_effect": "/Script/MyGame.GE_FireBallCooldown",
      "cost_effect": "/Script/MyGame.GE_FireBallCost"
    }
  ]
}
```

## Examples

### List all abilities
```json
{ "tool": "list_gameplay_abilities", "arguments": {} }
```

### Filter by name
```json
{ "tool": "list_gameplay_abilities", "arguments": { "name_filter": "Fire" } }
```

## Error Cases

No errors — returns `count: 0` with empty array if no abilities found.
