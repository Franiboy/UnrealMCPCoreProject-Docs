# set_metasound_input

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_MetaSoundEdit.cpp`

## Description

Sets the default value of a MetaSound node input. Identify the input by name (`input_name`) or by vertex GUID (`vertex_id`). Supports value types: float, int, bool, string.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | **Yes** | Asset path of the MetaSound Source |
| `node_id` | string | **Yes** | GUID of the node |
| `input_name` | string | * | Name of the input pin (one of `input_name`/`vertex_id` required) |
| `vertex_id` | string | * | GUID of the input vertex (one of `input_name`/`vertex_id` required) |
| `value_type` | string | **Yes** | Type of the value: `float`, `int`, `bool`, `string` |
| `value` | any | **Yes** | The value to set (type must match `value_type`) |

## Response

```json
{
  "node_id": "CF39FC0448B54A5FBDE4BDB97F9DEC6C",
  "input_name": "PrimaryOperand",
  "vertex_id": "839870904FB0CB165C41AAD269031987",
  "value_type": "float",
  "set": true
}
```

## Examples

### Set a float input by name
```json
{
  "tool": "set_metasound_input",
  "arguments": {
    "asset_path": "/Game/MyMetaSound",
    "node_id": "CF39FC04...",
    "input_name": "PrimaryOperand",
    "value_type": "float",
    "value": 42.5
  }
}
```

### Set a float input by vertex ID
```json
{
  "tool": "set_metasound_input",
  "arguments": {
    "asset_path": "/Game/MyMetaSound",
    "node_id": "CF39FC04...",
    "vertex_id": "839870904FB0CB165C41AAD269031987",
    "value_type": "float",
    "value": 3.14
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"Missing required parameter 'asset_path'"` |
| Missing `node_id` | `"Missing required parameter 'node_id'"` |
| Missing input identifier | `"Missing required parameter: either 'input_name' or 'vertex_id'"` |
| Missing `value_type` | `"Missing required parameter 'value_type'"` |
| Missing `value` | `"Missing required parameter 'value'"` |
| Unsupported type | `"Unsupported value_type: '...'. Supported: float, int, bool, string"` |
| Node not found | `"Node not found: '...'"` |
| Input not found | `"Input '...' not found on node"` |
| MetaSound not found | `"MetaSound not found: '<path>'"` |
