# add_sequence_binding

**Category:** Sequencer  
**Source:** `Private/Sequencer/MCPSequencerTools_Lifecycle.cpp`

## Description

Binds an actor from the current level to a Level Sequence. Creates either a **possessable** binding (references the existing actor in the level) or a **spawnable** binding (the sequence spawns its own copy of the actor at runtime).

## Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `asset_path` | string | **Yes** | | Asset path of the Level Sequence |
| `actor_name` | string | **Yes** | | Actor label or name in the current level |
| `binding_type` | string | No | `possessable` | `possessable` (references existing actor) or `spawnable` (sequence spawns a copy) |

## Response

```json
{
  "binding_name": "CineCameraActor",
  "guid": "ABCD1234-...",
  "binding_type": "Possessable",
  "class": "CineCameraActor",
  "sequence": "LS_MyCinematic"
}
```

## Examples

### Add possessable binding (default)
```json
{
  "tool": "add_sequence_binding",
  "arguments": {
    "asset_path": "/Game/LS_MyCinematic",
    "actor_name": "CineCameraActor"
  }
}
```

### Add spawnable binding
```json
{
  "tool": "add_sequence_binding",
  "arguments": {
    "asset_path": "/Game/LS_MyCinematic",
    "actor_name": "BP_Enemy",
    "binding_type": "spawnable"
  }
}
```

## Error Cases

- Missing `asset_path` or `actor_name` returns an error
- Non-existent sequence returns "not found" error
- Non-existent actor returns "not found in the current level" error
- Duplicate binding name returns "already exists" error
- Invalid `binding_type` returns error listing valid options
