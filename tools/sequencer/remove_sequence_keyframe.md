# remove_sequence_keyframe

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Edit.cpp`

## Description

Removes a keyframe at a specific time from a channel in a Level Sequence track section. Supports Float, Double, Bool, and Integer channel types.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Level Sequence |
| `track_index` | integer | **Yes** | | Index of the track |
| `time` | number | **Yes** | | Time of the keyframe to remove (in seconds) |
| `binding_name` | string | No | | Binding name (omit for master track) |
| `section_index` | integer | No | `0` | Section index within the track |
| `channel_index` | integer | No | `0` | Channel index within section |

## Response

```json
{
  "removed_frame": 48000,
  "removed_time": 2.0,
  "channel_type": "Float",
  "channel_index": 0,
  "success": true
}
```

## Examples

### Remove keyframe at 2 seconds
```json
{
  "tool": "remove_sequence_keyframe",
  "arguments": {
    "asset_path": "/Game/MySeq",
    "track_index": 0,
    "time": 2.0
  }
}
```

## Error Cases

- Missing `asset_path`, `track_index`, or `time` returns an error
- No keyframe at the specified time returns an error
- No supported channel at the given index returns an error
- Non-existent sequence or binding returns "not found"
