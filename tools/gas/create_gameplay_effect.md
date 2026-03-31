# create_gameplay_effect

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_Lifecycle.cpp`

## Description

Creates a new Gameplay Effect Blueprint asset with `UGameplayEffect` as parent. Optionally sets the duration policy on creation.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the Gameplay Effect Blueprint (e.g. `GE_Damage`) |
| `path` | string | No | `/Game` | Content path to create in |
| `duration_policy` | string | No | `Instant` | `Instant`, `Infinite`, or `HasDuration` |

## Response

```json
{
  "name": "GE_Damage",
  "asset_path": "/Game/Effects/GE_Damage.GE_Damage",
  "parent_class": "GameplayEffect",
  "duration_policy": "Instant"
}
```

## Examples

### Create an instant effect
```json
{ "tool": "create_gameplay_effect", "arguments": { "name": "GE_Damage" } }
```

### Create an infinite buff
```json
{
  "tool": "create_gameplay_effect",
  "arguments": {
    "name": "GE_SpeedBoost",
    "path": "/Game/Effects",
    "duration_policy": "Infinite"
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `name` | `"name is required"` |
| Asset already exists | `"Asset already exists: <path>"` |
| Invalid `duration_policy` | `"Invalid duration_policy: <value> (valid: Instant, Infinite, HasDuration)"` |
