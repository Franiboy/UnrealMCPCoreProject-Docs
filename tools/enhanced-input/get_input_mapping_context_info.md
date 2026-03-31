# get_input_mapping_context_info

**Category:** Enhanced Input  
**Source:** `Private/EnhancedInput/MCPEnhancedInputTools_Query.cpp`

## Description

Inspects an Input Mapping Context asset. Returns the context description and each action mapping with key, triggers, and modifiers.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Input Mapping Context |

## Response

```json
{
  "name": "IMC_Default",
  "path": "/Game/IMC_Default.IMC_Default",
  "description": "Default player context",
  "num_mappings": 2,
  "mappings": [
    {
      "index": 0,
      "action": "IA_Jump",
      "action_path": "/Game/IA_Jump.IA_Jump",
      "key": "SpaceBar",
      "key_display": "Space Bar",
      "triggers": ["InputTriggerPressed"],
      "modifiers": []
    }
  ]
}
```

## Examples

### Inspect a context
```json
{ "tool": "get_input_mapping_context_info", "arguments": { "asset_path": "/Game/IMC_Default" } }
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"asset_path is required"` |
| Context not found | `"Input Mapping Context not found: <path>"` |
