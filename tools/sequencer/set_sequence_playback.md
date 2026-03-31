# set_sequence_playback

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Edit.cpp`

## Description

Sets playback settings for a Level Sequence, including playback range, display frame rate, view range, and working range. You can set the playback range by time (seconds) or frame numbers.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Level Sequence |
| `start_time` | number | No | | Playback start time in seconds |
| `end_time` | number | No | | Playback end time in seconds |
| `start_frame` | integer | No | | Playback start frame (alternative to `start_time`) |
| `end_frame` | integer | No | | Playback end frame (alternative to `end_time`) |
| `frame_rate` | integer | No | | Display frame rate (e.g. 24, 30, 60) |
| `view_start` | number | No | | View range start in seconds |
| `view_end` | number | No | | View range end in seconds |
| `work_start` | number | No | | Working range start in seconds |
| `work_end` | number | No | | Working range end in seconds |

## Response

```json
{
  "sequence": "LS_MyCinematic",
  "display_rate": 60.0,
  "start_frame": 0,
  "end_frame": 240000,
  "start_seconds": 0.0,
  "end_seconds": 10.0,
  "duration_seconds": 10.0,
  "changed": ["start_time", "end_time", "frame_rate"]
}
```

## Examples

### Set 10-second playback range at 60fps
```json
{
  "tool": "set_sequence_playback",
  "arguments": {
    "asset_path": "/Game/MySeq",
    "start_time": 0.0,
    "end_time": 10.0,
    "frame_rate": 60
  }
}
```

### Set playback by frame numbers
```json
{
  "tool": "set_sequence_playback",
  "arguments": {
    "asset_path": "/Game/MySeq",
    "start_frame": 0,
    "end_frame": 300
  }
}
```

## Error Cases

- Missing `asset_path` returns an error
- No settings provided returns an error listing available options
- Non-existent sequence returns "not found"
