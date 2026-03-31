# get_metasound_info

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_Query.cpp`

## Description

Inspects a MetaSound Source asset and returns its document-level structure: metadata (class name, version, description), input/output interfaces, graph pages, nodes, edges, and external dependencies. Uses the MetaSound Frontend API to read the `FMetasoundFrontendDocument` without modifying the asset.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Full asset path (e.g. `/Game/Audio/MyMetaSound`) |

## Response

```json
{
  "name": "MyMetaSound",
  "asset_path": "/Game/Audio/MyMetaSound.MyMetaSound",
  "metadata": {
    "class_name": "MyMetaSound",
    "version": { "major": 1, "minor": 0 },
    "description": ""
  },
  "inputs": [
    {
      "name": "On Play",
      "type_name": "Trigger",
      "access_type": "Value"
    }
  ],
  "outputs": [
    {
      "name": "On Finished",
      "type_name": "Trigger",
      "access_type": "Value"
    }
  ],
  "pages": [
    {
      "name": "Default",
      "node_count": 5,
      "nodes": [
        {
          "id": "abc-123",
          "class_name": "Trigger Delay",
          "name": "Trigger Delay"
        }
      ],
      "edge_count": 3,
      "edges": [
        {
          "from_node": "abc-123",
          "from_vertex": "Out",
          "to_node": "def-456",
          "to_vertex": "In"
        }
      ]
    }
  ],
  "dependency_count": 2,
  "dependencies": [
    { "name": "/Script/MetasoundStandardNodes" }
  ]
}
```

## Examples

### Inspect a MetaSound
```json
{
  "tool": "get_metasound_info",
  "arguments": {
    "asset_path": "/Game/Audio/MyMetaSound"
  }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"Missing required parameter 'asset_path'"` |
| Empty `asset_path` | `"Parameter 'asset_path' must not be empty"` |
| Asset not found or wrong type | `"MetaSound Source not found: '<path>'"` |
| Cannot read document | `"Failed to read MetaSound document for '<name>'"` |
