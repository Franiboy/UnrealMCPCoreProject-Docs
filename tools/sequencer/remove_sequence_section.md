# remove_sequence_section

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Edit.cpp`

## Description

Removes a section from a track in a Level Sequence by its index.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Level Sequence |
| `track_index` | integer | **Yes** | | Index of the track |
| `section_index` | integer | **Yes** | | Index of the section to remove |
| `binding_name` | string | No | | Binding name (omit for master track) |

## Response

```json
{
  "removed_section_index": 0,
  "success": true
}
```

## Examples

### Remove section from master track
```json
{
  "tool": "remove_sequence_section",
  "arguments": {
    "asset_path": "/Game/MySeq",
    "track_index": 0,
    "section_index": 0
  }
}
```

## Error Cases

- Missing `asset_path`, `track_index`, or `section_index` returns an error
- `section_index` out of range returns an error with valid range
- Non-existent sequence or binding returns "not found"
