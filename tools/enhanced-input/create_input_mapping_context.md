# create_input_mapping_context

**Category:** Enhanced Input  
**Source:** `Private/EnhancedInput/MCPEnhancedInputTools_Lifecycle.cpp`

## Description

Creates a new Input Mapping Context asset (`UInputMappingContext`). The new context starts with zero mappings.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the Input Mapping Context asset (e.g. `IMC_Default`) |
| `path` | string | No | `/Game` | Content path to create in |
| `description` | string | No | | Human-readable context description |

## Response

```json
{
  "name": "IMC_Default",
  "path": "/Game/IMC_Default.IMC_Default",
  "description": "Default player context",
  "num_mappings": 0
}
```

## Examples

### Create a basic context
```json
{ "tool": "create_input_mapping_context", "arguments": { "name": "IMC_Default" } }
```

### Create with description
```json
{
  "tool": "create_input_mapping_context",
  "arguments": {
    "name": "IMC_Vehicle",
    "path": "/Game/Input",
    "description": "Vehicle driving controls"
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `name` | `"name is required"` |
| Asset already exists | `"Asset already exists: <path>"` |
