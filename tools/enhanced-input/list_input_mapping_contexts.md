# list_input_mapping_contexts

**Category:** Enhanced Input  
**Source:** `Private/EnhancedInput/MCPEnhancedInputTools_Query.cpp`

## Description

Lists Input Mapping Context assets (`UInputMappingContext`). Returns name, path, description, and mapping count for each context.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `path_prefix` | string | No | `/Game` | Content path prefix to filter |
| `name_filter` | string | No | | Filter by name (case-insensitive substring match) |
| `include_engine` | boolean | No | `false` | Include engine/plugin content |

## Response

```json
{
  "num_contexts": 2,
  "contexts": [
    {
      "name": "IMC_Default",
      "path": "/Game/IMC_Default.IMC_Default",
      "description": "Default player context",
      "num_mappings": 3
    }
  ]
}
```

## Examples

### List all game contexts
```json
{ "tool": "list_input_mapping_contexts", "arguments": {} }
```

### Filter by name
```json
{ "tool": "list_input_mapping_contexts", "arguments": { "name_filter": "Vehicle" } }
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Asset Registry unavailable | `"Asset Registry not available"` |
