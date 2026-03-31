# add_metasound_node

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_MetaSoundEdit.cpp`

## Description

Adds a node to a MetaSound graph by class name. The node class is specified in `Namespace.Name.Variant` format (e.g. `UE.Add.Float`). Returns the new node's GUID, class, inputs, and outputs.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the MetaSound Source |
| `node_class` | string | **Yes** | | Node class in `Namespace.Name.Variant` format |
| `major_version` | integer | No | `1` | Major version of the node class |

### Common Node Classes

| Class String | Description |
|-------------|-------------|
| `UE.Add.Float` | Adds float values |
| `UE.Add.Int32` | Adds integer values |
| `UE.Subtract.Float` | Subtracts float values |
| `UE.Multiply.Float` | Multiplies float values |
| `UE.Divide.Float` | Divides float values |

## Response

```json
{
  "node_id": "CF39FC0448B54A5FBDE4BDB97F9DEC6C",
  "class_id": "A1B2C3D4...",
  "inputs": [
    { "name": "PrimaryOperand", "type": "Float", "vertex_id": "839870904FB0CB165C41AAD269031987" }
  ],
  "outputs": [
    { "name": "Out", "type": "Float", "vertex_id": "DF859E00E6B223C1313F89B8EB6AC9BC" }
  ],
  "asset_path": "/Game/MyMetaSound.MyMetaSound",
  "node_class": "UE.Add.Float",
  "major_version": 1,
  "total_nodes": 5
}
```

## Examples

### Add an Add-Float node
```json
{ "tool": "add_metasound_node", "arguments": { "asset_path": "/Game/MyMetaSound", "node_class": "UE.Add.Float" } }
```

### Add a Multiply-Int32 node with explicit version
```json
{
  "tool": "add_metasound_node",
  "arguments": { "asset_path": "/Game/MyMetaSound", "node_class": "UE.Multiply.Int32", "major_version": 1 }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"Missing required parameter 'asset_path'"` |
| Missing `node_class` | `"Missing required parameter 'node_class'"` |
| Invalid class format | `"Invalid node_class format: '...'. Expected 'Namespace.Name.Variant'"` |
| Class not registered | `"Failed to add node '...' (version ...). Class may not be registered"` |
| MetaSound not found | `"MetaSound not found: '<path>'"` |
