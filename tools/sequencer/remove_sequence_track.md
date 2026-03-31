# remove_sequence_track

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Edit.cpp`

## Description

Removes a track from a Level Sequence by index. Can remove master tracks or binding tracks.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Level Sequence |
| `track_index` | integer | **Yes** | | Index of the track to remove |
| `binding_name` | string | No | | Binding name (omit for master track) |

## Response

```json
{
  "removed_type": "FadeTrack",
  "removed_name": "Fade",
  "success": true
}
```

## Examples

### Remove master track at index 0
```json
{ "tool": "remove_sequence_track", "arguments": { "asset_path": "/Game/MySeq", "track_index": 0 } }
```

### Remove binding track
```json
{
  "tool": "remove_sequence_track",
  "arguments": {
    "asset_path": "/Game/MySeq",
    "track_index": 0,
    "binding_name": "MyActor"
  }
}
```

## Error Cases

- Missing `asset_path` or `track_index` returns an error
- `track_index` out of range returns an error with valid range
- Non-existent sequence or binding returns "not found"
