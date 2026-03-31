# connect_metasound_nodes

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_MetaSoundEdit.cpp`

## Description

Wires two MetaSound nodes together (output to input). Vertices can be specified by name (`from_output_name`/`to_input_name`) or by GUID (`from_vertex_id`/`to_vertex_id`). If neither is given, the first available output/input is auto-selected.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | **Yes** | Asset path of the MetaSound Source |
| `from_node_id` | string | **Yes** | GUID of the source (output) node |
| `to_node_id` | string | **Yes** | GUID of the destination (input) node |
| `from_output_name` | string | No | Name of the output pin on source node |
| `to_input_name` | string | No | Name of the input pin on destination node |
| `from_vertex_id` | string | No | GUID of the output vertex |
| `to_vertex_id` | string | No | GUID of the input vertex |

## Response

```json
{
  "from_node_id": "CF39FC0448B54A5FBDE4BDB97F9DEC6C",
  "from_vertex_id": "DF859E00E6B223C1313F89B8EB6AC9BC",
  "to_node_id": "0A2BDDE14E0B8F5A22E88D8F036E3546",
  "to_vertex_id": "839870904FB0CB165C41AAD269031987",
  "from_output_name": "Out",
  "to_input_name": "PrimaryOperand",
  "connected": true
}
```

## Examples

### Connect by output/input name
```json
{
  "tool": "connect_metasound_nodes",
  "arguments": {
    "asset_path": "/Game/MyMetaSound",
    "from_node_id": "CF39FC04...",
    "to_node_id": "0A2BDDE1...",
    "from_output_name": "Out",
    "to_input_name": "PrimaryOperand"
  }
}
```

### Auto-select first output/input
```json
{
  "tool": "connect_metasound_nodes",
  "arguments": {
    "asset_path": "/Game/MyMetaSound",
    "from_node_id": "CF39FC04...",
    "to_node_id": "0A2BDDE1..."
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"Missing required parameter 'asset_path'"` |
| Missing `from_node_id` | `"Missing required parameter 'from_node_id'"` |
| Missing `to_node_id` | `"Missing required parameter 'to_node_id'"` |
| Invalid GUID | `"Invalid from_node_id format: '...'"` |
| Source node not found | `"Source node not found: '...'"` |
| Output not found | `"Output '...' not found on source node"` |
| Input not found | `"Input '...' not found on destination node"` |
| Incompatible types | `"Invalid connection: MismatchedDataType"` |
| MetaSound not found | `"MetaSound not found: '<path>'"` |
