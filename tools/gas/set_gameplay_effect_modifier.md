# set_gameplay_effect_modifier

**Category:** Gameplay Ability System (GAS)  
**Source:** `Private/GAS/MCPGASTools_Edit.cpp`

## Description

Adds or edits a modifier on a Gameplay Effect Blueprint. Each modifier targets an attribute, applies an operation (add, multiply, override, etc.), and has a magnitude value.

- **Add new modifier:** Omit `modifier_index` (or set to `-1`). `attribute` is required.
- **Edit existing modifier:** Set `modifier_index` to the 0-based index. Only provided fields are updated.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Gameplay Effect Blueprint |
| `modifier_index` | integer | No | `-1` (add new) | 0-based index of existing modifier to edit |
| `attribute` | string | Conditional | | Attribute in `ClassName.PropertyName` format (e.g. `MyAttributeSet.Health`). Required when adding a new modifier. |
| `modifier_op` | string | No | `Additive` | `Additive`, `Multiplicitive`, `Division`, `Override`, `MultiplyCompound`, or `AddFinal` |
| `magnitude` | number | No | `0` | Scalable float magnitude value |

## Response

```json
{
  "success": true,
  "added": true,
  "modifier_index": 0,
  "attribute": "MyAttributeSet.Health",
  "modifier_op": "Additive",
  "magnitude_value": -25.0,
  "total_modifiers": 1
}
```

## Examples

### Add a damage modifier
```json
{
  "tool": "set_gameplay_effect_modifier",
  "arguments": {
    "asset_path": "/Game/Effects/GE_Damage",
    "attribute": "MyAttributeSet.Health",
    "modifier_op": "Additive",
    "magnitude": -25
  }
}
```

### Edit an existing modifier's magnitude
```json
{
  "tool": "set_gameplay_effect_modifier",
  "arguments": {
    "asset_path": "/Game/Effects/GE_Damage",
    "modifier_index": 0,
    "magnitude": -50
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"asset_path is required"` |
| Asset not found | `"Gameplay Effect Blueprint not found: <path>"` |
| Missing `attribute` (new modifier) | `"attribute is required when adding a new modifier"` |
| Invalid `modifier_index` | `"Invalid modifier_index: N (effect has M modifiers)"` |
| Invalid `modifier_op` | `"Invalid modifier_op: <op>"` |
| Unknown AttributeSet class | `"AttributeSet class not found: <class>"` |
| Unknown attribute property | `"Attribute property '<prop>' not found on <class>"` |
