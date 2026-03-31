# connect_sound_cue_nodes

**Category:** Audio  
**Source:** `Private/Audio/MCPAudioTools_SoundCueEdit.cpp`

## Description

Wires two Sound Cue nodes together (parent -> child). Optionally specify which input slot on the parent to use. If no slot is specified, the first empty slot is used or a new one is created.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Sound Cue |
| `parent_index` | integer | **Yes** | | Index of the parent node |
| `child_index` | integer | **Yes** | | Index of the child node |
| `input_index` | integer | No | auto | Input slot index on the parent |

## Response

```json
{
  "parent_index": 0,
  "parent_class": "SoundNodeMixer",
  "child_index": 1,
  "child_class": "SoundNodeWavePlayer",
  "input_index": 0,
  "parent_child_count": 1
}
```

## Examples

### Auto-assign child to parent
```json
{
  "tool": "connect_sound_cue_nodes",
  "arguments": { "asset_path": "/Game/MySoundCue", "parent_index": 0, "child_index": 1 }
}
```

### Assign to a specific input slot
```json
{
  "tool": "connect_sound_cue_nodes",
  "arguments": { "asset_path": "/Game/MySoundCue", "parent_index": 0, "child_index": 2, "input_index": 1 }
}
```

## Error Cases

| Condition | Error |
|-----------|-------|
| Missing `asset_path` | `"Missing required parameter 'asset_path'"` |
| Missing `parent_index` | `"Missing required parameter 'parent_index'"` |
| Missing `child_index` | `"Missing required parameter 'child_index'"` |
| Index out of range | `"parent_index <n> out of range (0..<max>)"` |
| Same parent and child | `"parent_index and child_index must be different"` |
| Node has no child slots | `"Node '<name>' does not accept children"` |
| All slots full | `"No available child slots on '<name>'"` |
| Sound Cue not found | `"Sound Cue not found: '<path>'"` |
