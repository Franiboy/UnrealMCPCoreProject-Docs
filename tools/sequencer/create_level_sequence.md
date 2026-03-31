# create_level_sequence

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Lifecycle.cpp`

## Description

Creates a new Level Sequence asset. Initializes the inner MovieScene with default tick resolution and display rate. Optionally set a custom frame rate and initial playback duration.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `name` | string | **Yes** | | Name of the Level Sequence asset |
| `path` | string | No | `/Game` | Content path prefix |
| `frame_rate` | integer | No | `30` | Display frame rate in fps |
| `duration` | number | No | | Initial playback duration in seconds |

## Response

```json
{
  "name": "LS_MyCinematic",
  "asset_path": "/Game/LS_MyCinematic.LS_MyCinematic",
  "display_rate": 30.0,
  "duration_seconds": 5.0
}
```

## Examples

### Create with defaults
```json
{ "tool": "create_level_sequence", "arguments": { "name": "LS_Intro" } }
```

### Create with custom frame rate and duration
```json
{
  "tool": "create_level_sequence",
  "arguments": {
    "name": "LS_Cinematic",
    "path": "/Game/Cinematics",
    "frame_rate": 60,
    "duration": 10.0
  }
}
```

## Error Cases

- Missing or empty `name` returns an error
- Duplicate asset name returns "already exists" error
