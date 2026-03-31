# get_level_sequence_info

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Query.cpp`

## Description

Inspects a Level Sequence asset: duration, frame rate, playback range, master tracks, object bindings (possessable/spawnable), and track summaries. Provides a complete overview of the sequence structure without deep keyframe data.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Level Sequence |
| `include_bindings` | boolean | No | `true` | Include object binding details |
| `include_tracks` | boolean | No | `true` | Include track listings |

## Response

```json
{
  "name": "LS_Intro",
  "asset_path": "/Game/Cinematics/LS_Intro.LS_Intro",
  "display_rate": "30/1 fps",
  "display_rate_numerator": 30,
  "display_rate_denominator": 1,
  "tick_resolution": "24000/1",
  "start_frame": 0,
  "end_frame": 72000,
  "start_seconds": 0.0,
  "end_seconds": 3.0,
  "duration_seconds": 3.0,
  "duration_frames": 90,
  "master_track_count": 1,
  "master_tracks": [
    {
      "index": 0,
      "type": "CameraCutTrack",
      "display_name": "Camera Cuts",
      "section_count": 1
    }
  ],
  "binding_count": 2,
  "bindings": [
    {
      "name": "CineCameraActor",
      "guid": "ABCD-1234...",
      "binding_type": "Possessable",
      "class": "CineCameraActor",
      "track_count": 1,
      "tracks": [
        {
          "index": 0,
          "type": "3DTransformTrack",
          "display_name": "Transform",
          "section_count": 1
        }
      ]
    }
  ],
  "total_track_count": 3
}
```

## Examples

### Full inspection
```json
{ "tool": "get_level_sequence_info", "arguments": { "asset_path": "/Game/Cinematics/LS_Intro" } }
```

### Structure only (no bindings)
```json
{
  "tool": "get_level_sequence_info",
  "arguments": {
    "asset_path": "/Game/Cinematics/LS_Intro",
    "include_bindings": false
  }
}
```

## Error Cases

- Missing or empty `asset_path` returns an error
- Non-existent asset path returns "not found" error
