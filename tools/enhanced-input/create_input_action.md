# create_input_action

**Category:** Enhanced Input  
**Source:** `Private/EnhancedInput/MCPEnhancedInputTools_Lifecycle.cpp`

## Description

Creates a new Input Action asset (`UInputAction`). Supports value types: Boolean, Axis1D, Axis2D, Axis3D.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the Input Action asset (e.g. `IA_Jump`) |
| `path` | string | No | `/Game` | Content path to create in |
| `value_type` | string | No | `Boolean` | `Boolean`, `Axis1D`, `Axis2D`, or `Axis3D` |
| `description` | string | No | | Human-readable action description |
| `consume_input` | boolean | No | `true` | Whether the action consumes input |

## Response

```json
{
  "name": "IA_Jump",
  "path": "/Game/IA_Jump.IA_Jump",
  "value_type": "Boolean",
  "consume_input": true
}
```

## Examples

### Create a simple boolean action
```json
{ "tool": "create_input_action", "arguments": { "name": "IA_Jump" } }
```

### Create an Axis2D action for look input
```json
{
  "tool": "create_input_action",
  "arguments": {
    "name": "IA_Look",
    "value_type": "Axis2D",
    "description": "Mouse/stick look input"
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `name` | `"name is required"` |
| Asset already exists | `"Asset already exists: <path>"` |
| Invalid `value_type` | `"Invalid value_type: <type>. Expected Boolean, Axis1D, Axis2D, or Axis3D"` |
