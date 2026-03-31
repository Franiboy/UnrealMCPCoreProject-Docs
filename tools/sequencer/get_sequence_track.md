# get_sequence_track

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Query.cpp`

## Description

Detailed inspection of a track in a Level Sequence — type, sections, channels, and keyframes. Use `binding_name` for object binding tracks; omit it to inspect master (top-level) tracks. Supports float, double, bool, and integer channel types.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Level Sequence |
| `binding_name` | string | No | | Binding name (for object binding tracks). Omit for master tracks. |
| `track_index` | integer | **Yes** | | Track index within the master or binding track list (0-based) |
| `include_keyframes` | boolean | No | `true` | Include keyframe data |
| `max_keyframes` | integer | No | `100` | Maximum keyframes to return per channel |

## Response

```json
{
  "type": "3DTransformTrack",
  "display_name": "Transform",
  "source": "binding",
  "track_index": 0,
  "binding_name": "CineCameraActor",
  "section_count": 1,
  "sections": [
    {
      "index": 0,
      "start_frame": 0,
      "start_time": 0.0,
      "end_frame": 72000,
      "end_time": 3.0,
      "channel_count": 9,
      "channels": [
        {
          "index": 0,
          "type": "Double",
          "keyframe_count": 2,
          "keyframes": [
            {
              "frame": 0,
              "time": 0.0,
              "value": 100.0,
              "interp": "Cubic"
            },
            {
              "frame": 72000,
              "time": 3.0,
              "value": 200.0,
              "interp": "Cubic"
            }
          ]
        }
      ]
    }
  ]
}
```

## Channel Types

| Type | Description |
|------|-------------|
| `Float` | Single-precision float values (e.g. FOV, opacity) |
| `Double` | Double-precision values (e.g. transform XYZ in UE5) |
| `Bool` | Boolean on/off keyframes (e.g. visibility) |
| `Integer` | Integer values (e.g. enum indices) |

## Examples

### Inspect a master track
```json
{
  "tool": "get_sequence_track",
  "arguments": {
    "asset_path": "/Game/Cinematics/LS_Intro",
    "track_index": 0
  }
}
```

### Inspect a binding track
```json
{
  "tool": "get_sequence_track",
  "arguments": {
    "asset_path": "/Game/Cinematics/LS_Intro",
    "binding_name": "CineCameraActor",
    "track_index": 0
  }
}
```

### Without keyframes (summary only)
```json
{
  "tool": "get_sequence_track",
  "arguments": {
    "asset_path": "/Game/Cinematics/LS_Intro",
    "track_index": 0,
    "include_keyframes": false
  }
}
```

## Error Cases

- Missing `asset_path` or `track_index` returns an error
- Non-existent sequence returns "not found" error
- Invalid `binding_name` returns error with list of available binding names
- `track_index` out of range returns error with valid range
