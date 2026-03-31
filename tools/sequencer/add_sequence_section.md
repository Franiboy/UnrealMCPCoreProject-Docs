# add_sequence_section

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Edit.cpp`

## Description

Adds a section (time range) to a track in a Level Sequence. Sections define the active range of a track and contain the actual keyframe data. You can specify the range by time (seconds) or frame numbers.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Level Sequence |
| `track_index` | integer | **Yes** | | Index of the track |
| `binding_name` | string | No | | Binding name (omit for master track) |
| `start_time` | number | No | | Section start time in seconds |
| `end_time` | number | No | | Section end time in seconds |
| `start_frame` | integer | No | | Section start frame (alternative to `start_time`) |
| `end_frame` | integer | No | | Section end frame (alternative to `end_time`) |
| `row_index` | integer | No | | Row index for the section |

## Response

```json
{
  "section_index": 0,
  "section_class": "MovieSceneFloatSection",
  "start_frame": 0,
  "start_time": 0.0,
  "end_frame": 48000,
  "end_time": 2.0
}
```

## Examples

### Add section with time range
```json
{
  "tool": "add_sequence_section",
  "arguments": {
    "asset_path": "/Game/MySeq",
    "track_index": 0,
    "start_time": 0.0,
    "end_time": 5.0
  }
}
```

### Add section to a binding track
```json
{
  "tool": "add_sequence_section",
  "arguments": {
    "asset_path": "/Game/MySeq",
    "track_index": 0,
    "binding_name": "MyActor",
    "start_time": 1.0,
    "end_time": 3.0
  }
}
```

## Error Cases

- Missing `asset_path` or `track_index` returns an error
- `track_index` out of range returns an error with valid range
- Non-existent sequence or binding returns "not found"
