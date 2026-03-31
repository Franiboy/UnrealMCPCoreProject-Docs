# add_niagara_emitter_to_system

Add an emitter reference to a Niagara System.

## Input

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "emitter_path": "/Game/FX/NE_Sparks",
  "emitter_name": "Sparks"
}
```

| Parameter      | Required | Description                                                     |
| -------------- | -------- | --------------------------------------------------------------- |
| `system_path`  | **yes**  | Asset path of the Niagara System                                |
| `emitter_path` | **yes**  | Asset path of the Niagara Emitter to add                        |
| `emitter_name` | no       | Name for the emitter handle (defaults to the emitter asset name)|

## Output

```json
{
  "system": "NS_Fire",
  "emitter_name": "Sparks",
  "handle_id": "A1B2C3D4-...",
  "emitter_count": 2
}
```

| Field           | Type   | Description                                |
| --------------- | ------ | ------------------------------------------ |
| `system`        | string | System asset name                          |
| `emitter_name`  | string | Name of the newly added emitter handle     |
| `handle_id`     | string | GUID of the new emitter handle             |
| `emitter_count` | number | Total emitter count in the system after add|

## Examples

### Add an emitter with default name

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "emitter_path": "/Game/FX/NE_Sparks"
}
```

### Add an emitter with a custom name

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "emitter_path": "/Game/FX/NE_Sparks",
  "emitter_name": "TrailSparks"
}
```

## Errors

| Condition                        | Message                        |
| -------------------------------- | ------------------------------ |
| Missing `system_path`            | system_path and emitter_path are required |
| Missing `emitter_path`           | system_path and emitter_path are required |
| System not found                 | System not found: ...          |
| Emitter not found                | Emitter not found: ...         |
