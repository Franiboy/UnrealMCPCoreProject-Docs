# remove_sound_cue_node

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_SoundCueEdit.cpp`

## Description

Removes a node from a Sound Cue graph by index. Disconnects the node from all parents (sets their child references to null) and clears the root reference if the removed node was the first node.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Sound Cue |
| `node_index` | integer | **Yes** | | Index of the node to remove (from `get_sound_cue_info`) |

## Response

```json
{
  "removed_class": "SoundNodeDelay",
  "removed_name": "SoundNodeDelay_0",
  "removed_index": 2,
  "remaining_nodes": 2
}
```

## Examples

### Remove a node by index
```json
{ "tool": "remove_sound_cue_node", "arguments": { "asset_path": "/Game/MySoundCue", "node_index": 2 } }
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"Missing required parameter 'asset_path'"` |
| Missing `node_index` | `"Missing required parameter 'node_index'"` |
| Index out of range | `"node_index <n> out of range (0..<max>)"` |
| Sound Cue not found | `"Sound Cue not found: '<path>'"` |
