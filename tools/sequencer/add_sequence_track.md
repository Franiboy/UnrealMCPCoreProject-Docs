# add_sequence_track

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Edit.cpp`

## Description

Adds a track to a Level Sequence — either as a master (top-level) track or attached to a named binding. Supports common track types: Transform, Float, Bool, Event, Audio, Animation, Fade, CameraCut, Sub, and Visibility.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Level Sequence |
| `track_type` | string | **Yes** | | Track type: `Transform`, `Float`, `Bool`, `Event`, `Audio`, `Animation`, `Fade`, `CameraCut`, `Sub`, `Visibility` |
| `binding_name` | string | No | | Binding name to add the track to (omit for master track) |

## Response

```json
{
  "type": "3DTransformTrack",
  "display_name": "Transform",
  "source": "binding",
  "track_index": 0,
  "binding_name": "MyActor"
}
```

## Examples

### Add master Fade track
```json
{ "tool": "add_sequence_track", "arguments": { "asset_path": "/Game/MySeq", "track_type": "Fade" } }
```

### Add Transform track to a binding
```json
{
  "tool": "add_sequence_track",
  "arguments": {
    "asset_path": "/Game/MySeq",
    "track_type": "Transform",
    "binding_name": "CameraActor"
  }
}
```

## Error Cases

- Missing `asset_path` or `track_type` returns an error
- Unknown `track_type` lists available types
- Non-existent sequence or binding returns "not found"
