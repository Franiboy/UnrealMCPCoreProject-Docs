# create_gameplay_ability

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_Lifecycle.cpp`

## Description

Creates a new Gameplay Ability Blueprint asset. The Blueprint's parent class defaults to `UGameplayAbility` but can be set to any subclass.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the Gameplay Ability Blueprint (e.g. `GA_FireBall`) |
| `path` | string | No | `/Game` | Content path to create in |
| `parent_class` | string | No | `GameplayAbility` | Parent class path. Accepts native class paths or Blueprint asset paths. |

## Response

```json
{
  "name": "GA_FireBall",
  "asset_path": "/Game/Abilities/GA_FireBall.GA_FireBall",
  "parent_class": "GameplayAbility"
}
```

## Examples

### Create a basic ability
```json
{ "tool": "create_gameplay_ability", "arguments": { "name": "GA_FireBall" } }
```

### Create in subfolder
```json
{
  "tool": "create_gameplay_ability",
  "arguments": {
    "name": "GA_Dash",
    "path": "/Game/Abilities"
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `name` | `"name is required"` |
| Asset already exists | `"Asset already exists: <path>"` |
| Invalid parent class | `"parent_class not found or not a valid GameplayAbility subclass: <class>"` |
