# remove_input_mapping

**Category:** Enhanced Input  
**Source:** `Private/EnhancedInput/MCPEnhancedInputTools_Edit.cpp`

## Description

Removes an action mapping from an Input Mapping Context by index. Use `get_input_mapping_context_info` to discover mapping indices.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `context_path` | string | **Yes** | | Asset path of the Input Mapping Context |
| `mapping_index` | integer | **Yes** | | 0-based index of the mapping to remove |

## Response

```json
{
  "context": "IMC_Default",
  "context_path": "/Game/IMC_Default.IMC_Default",
  "removed_action": "IA_Jump",
  "removed_key": "SpaceBar",
  "removed_index": 0,
  "num_mappings": 2
}
```

## Examples

### Remove the first mapping
```json
{
  "tool": "remove_input_mapping",
  "arguments": {
    "context_path": "/Game/IMC_Default",
    "mapping_index": 0
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `context_path` | `"context_path is required"` |
| Missing `mapping_index` | `"mapping_index is required"` |
| Context not found | `"Input Mapping Context not found: <path>"` |
| Index out of range | `"mapping_index <n> out of range (0..<max>)"` |
