# remove_sequence_binding

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Lifecycle.cpp`

## Description

Removes an actor/component binding from a Level Sequence. Also removes all tracks associated with the binding. Handles both possessable and spawnable bindings automatically.

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Level Sequence |
| `binding_name` | string | **Yes** | | Name of the binding to remove |

## Response

```json
{
  "removed_binding": "CineCameraActor",
  "guid": "ABCD1234-...",
  "binding_type": "Possessable",
  "sequence": "LS_MyCinematic"
}
```

## Examples

### Remove a binding
```json
{
  "tool": "remove_sequence_binding",
  "arguments": {
    "asset_path": "/Game/LS_MyCinematic",
    "binding_name": "CineCameraActor"
  }
}
```

## Error Cases

- Missing `asset_path` or `binding_name` returns an error
- Non-existent sequence returns "not found" error
- Non-existent binding returns error with list of available binding names
