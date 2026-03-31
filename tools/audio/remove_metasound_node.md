# remove_metasound_node

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_MetaSoundEdit.cpp`

## Description

Removes a node from a MetaSound graph by its GUID. Also removes all edges connected to the node.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | **Yes** | Asset path of the MetaSound Source |
| `node_id` | string | **Yes** | GUID of the node to remove |

## Response

```json
{
  "node_id": "CF39FC0448B54A5FBDE4BDB97F9DEC6C",
  "class_id": "A1B2C3D4...",
  "name": "None",
  "removed": true,
  "remaining_nodes": 4
}
```

## Examples

### Remove a node by ID
```json
{
  "tool": "remove_metasound_node",
  "arguments": { "asset_path": "/Game/MyMetaSound", "node_id": "CF39FC0448B54A5FBDE4BDB97F9DEC6C" }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"Missing required parameter 'asset_path'"` |
| Missing `node_id` | `"Missing required parameter 'node_id'"` |
| Invalid GUID | `"Invalid node_id format: '...'"` |
| Node not found | `"Node not found: '...'"` |
| MetaSound not found | `"MetaSound not found: '<path>'"` |
