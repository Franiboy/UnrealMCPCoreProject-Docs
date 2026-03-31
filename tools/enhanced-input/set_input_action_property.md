# set_input_action_property

**Category:** Enhanced Input  
**Source:** `Private/EnhancedInput/MCPEnhancedInputTools_Edit.cpp`

## Description

Sets properties on an Input Action asset: value type, description, consume input, triggers, and modifiers. At least one property must be specified.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Input Action |
| `value_type` | string | No | | `Boolean`, `Axis1D`, `Axis2D`, or `Axis3D` |
| `description` | string | No | | Human-readable action description |
| `consume_input` | boolean | No | | Whether the action consumes input |
| `triggers` | string[] | No | | Replace action-level triggers (empty array clears) |
| `modifiers` | string[] | No | | Replace action-level modifiers (empty array clears) |

## Response

```json
{
  "name": "IA_Jump",
  "path": "/Game/IA_Jump.IA_Jump",
  "value_type": "Axis2D",
  "consume_input": false,
  "description": "Updated description",
  "triggers": ["InputTriggerDown"],
  "modifiers": [],
  "changed": ["value_type", "consume_input", "description", "triggers"]
}
```

## Examples

### Change value type
```json
{
  "tool": "set_input_action_property",
  "arguments": {
    "asset_path": "/Game/IA_Move",
    "value_type": "Axis2D"
  }
}
```

### Set triggers and description
```json
{
  "tool": "set_input_action_property",
  "arguments": {
    "asset_path": "/Game/IA_Jump",
    "description": "Player jump",
    "triggers": ["InputTriggerPressed"]
  }
}
```

### Clear triggers
```json
{
  "tool": "set_input_action_property",
  "arguments": {
    "asset_path": "/Game/IA_Jump",
    "triggers": []
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"asset_path is required"` |
| Action not found | `"Input Action not found: <path>"` |
| Invalid `value_type` | `"Invalid value_type: <type>"` |
| No properties specified | `"No properties specified to change"` |
| Unknown trigger class | `"Unknown trigger class: <name>"` |
| Unknown modifier class | `"Unknown modifier class: <name>"` |
