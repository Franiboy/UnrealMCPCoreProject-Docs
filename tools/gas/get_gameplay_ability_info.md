# get_gameplay_ability_info

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_Query.cpp`

## Description

Inspects a Gameplay Ability Blueprint in detail. Returns instancing/network policies, all tag containers (activation, blocking, cancellation, source, target), and cost/cooldown effect references.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Gameplay Ability Blueprint |

## Response

```json
{
  "name": "GA_FireBall",
  "asset_path": "/Game/Abilities/GA_FireBall.GA_FireBall",
  "parent_class": "GameplayAbility",
  "instancing_policy": "InstancedPerActor",
  "replication_policy": "ReplicateNo",
  "net_execution_policy": "LocalPredicted",
  "net_security_policy": "ClientOrServer",
  "ability_tags": ["Ability.Attack.Fire"],
  "cancel_abilities_with_tag": [],
  "block_abilities_with_tag": [],
  "activation_owned_tags": ["State.Attacking"],
  "activation_required_tags": [],
  "activation_blocked_tags": ["State.Dead"],
  "source_required_tags": [],
  "source_blocked_tags": [],
  "target_required_tags": [],
  "target_blocked_tags": [],
  "cooldown_effect": "/Script/MyGame.GE_Cooldown",
  "cost_effect": "/Script/MyGame.GE_Cost"
}
```

Tag arrays are only included when non-empty.

## Examples

### Inspect an ability
```json
{ "tool": "get_gameplay_ability_info", "arguments": { "asset_path": "/Game/Abilities/GA_FireBall" } }
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"asset_path is required"` |
| Asset not found | `"Gameplay Ability Blueprint not found: <path>"` |
