# add_sequence_keyframe

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Edit.cpp`

## Description

Adds or sets a keyframe on a channel in a Level Sequence track section. Supports Float, Double, Bool, and Integer channel types. For Transform tracks, use `channel_index` to target specific components (0-2 = Location XYZ, 3-5 = Rotation XYZ, 6-8 = Scale XYZ).

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Level Sequence |
| `track_index` | integer | **Yes** | | Index of the track |
| `time` | number | **Yes** | | Keyframe time in seconds |
| `value` | number | **Yes** | | Keyframe value |
| `binding_name` | string | No | | Binding name (omit for master track) |
| `section_index` | integer | No | `0` | Section index within the track |
| `channel_index` | integer | No | `0` | Channel index within section |
| `interpolation` | string | No | `cubic` | Interpolation mode: `constant`, `linear`, `cubic` |

## Response

```json
{
  "frame": 48000,
  "time": 2.0,
  "value": 1.0,
  "channel_type": "Float",
  "channel_index": 0,
  "interpolation": "cubic"
}
```

## Examples

### Add cubic keyframe on Fade track
```json
{
  "tool": "add_sequence_keyframe",
  "arguments": {
    "asset_path": "/Game/MySeq",
    "track_index": 0,
    "time": 2.0,
    "value": 1.0
  }
}
```

### Add linear keyframe on Transform track (Location X)
```json
{
  "tool": "add_sequence_keyframe",
  "arguments": {
    "asset_path": "/Game/MySeq",
    "track_index": 0,
    "binding_name": "CameraActor",
    "time": 0.0,
    "value": 100.0,
    "channel_index": 0,
    "interpolation": "linear"
  }
}
```

## Error Cases

- Missing `asset_path`, `track_index`, `time`, or `value` returns an error
- No supported channel at the given index returns an error
- Non-existent sequence or binding returns "not found"
