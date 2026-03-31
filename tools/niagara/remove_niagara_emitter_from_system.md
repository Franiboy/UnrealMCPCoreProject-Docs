# remove_niagara_emitter_from_system

Remove an emitter from a Niagara System by name.

## Input

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "emitter_name": "Sparks"
}
```

| Parameter      | Required | Description                          |
| -------------- | -------- | ------------------------------------ |
| `system_path`  | **yes**  | Asset path of the Niagara System     |
| `emitter_name` | **yes**  | Name of the emitter to remove        |

## Output

```json
{
  "system": "NS_Fire",
  "removed_emitter": "Sparks",
  "remaining_emitter_count": 1
}
```

| Field                     | Type   | Description                               |
| ------------------------- | ------ | ----------------------------------------- |
| `system`                  | string | System asset name                         |
| `removed_emitter`         | string | Name of the removed emitter               |
| `remaining_emitter_count` | number | Emitter count after removal               |

## Examples

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "emitter_name": "OmnidirectionalBurst"
}
```

## Errors

| Condition              | Message                                              |
| ---------------------- | ---------------------------------------------------- |
| Missing required param | system_path and emitter_name are required            |
| System not found       | System not found: ...                                |
| Emitter not found      | Emitter '...' not found. Available: ...              |
