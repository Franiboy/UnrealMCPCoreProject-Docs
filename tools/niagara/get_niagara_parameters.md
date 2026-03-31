# get_niagara_parameters

List all user-exposed parameters of a Niagara System with their types, current values, and metadata.

## Input

```json
{
  "system_path": "/Game/FX/NS_Fire"
}
```

| Parameter     | Required | Description                        |
| ------------- | -------- | ---------------------------------- |
| `system_path` | **yes**  | Asset path of the Niagara System   |

## Output

```json
{
  "system": "NS_Fire",
  "count": 2,
  "parameters": [
    {
      "name": "User.SpawnRate",
      "type": "NiagaraFloat",
      "is_data_interface": false,
      "is_uobject": false,
      "value": 100.0
    },
    {
      "name": "User.Color",
      "type": "NiagaraLinearColor",
      "is_data_interface": false,
      "is_uobject": false,
      "value": {"r": 1.0, "g": 0.5, "b": 0.0, "a": 1.0}
    }
  ]
}
```

| Field                       | Type    | Description                                   |
| --------------------------- | ------- | --------------------------------------------- |
| `system`                    | string  | System asset name                             |
| `count`                     | number  | Total number of exposed parameters            |
| `parameters[].name`         | string  | Parameter name (may include "User." prefix)   |
| `parameters[].type`         | string  | Niagara type name                             |
| `parameters[].is_data_interface` | boolean | Whether the parameter is a data interface |
| `parameters[].is_uobject`   | boolean | Whether the parameter is a UObject reference  |
| `parameters[].value`        | varies  | Current value (number, bool, object, or null) |

### Supported value formats

| Niagara type        | JSON value format                              |
| ------------------- | ---------------------------------------------- |
| `NiagaraFloat`      | number (e.g. `42.0`)                           |
| `NiagaraInt32`      | number (e.g. `10`)                             |
| `NiagaraBool`       | boolean (e.g. `true`)                          |
| Vec2                | `{"x": 1.0, "y": 2.0}`                        |
| Vec3 / Position     | `{"x": 1.0, "y": 2.0, "z": 3.0}`             |
| Vec4                | `{"x": 1.0, "y": 2.0, "z": 3.0, "w": 4.0}`  |
| Color               | `{"r": 1.0, "g": 0.0, "b": 0.0, "a": 1.0}`  |
| Data interface / UObject | `null` (value not serialized)             |

## Errors

| Condition          | Message                      |
| ------------------ | ---------------------------- |
| Missing param      | system_path is required      |
| System not found   | System not found: ...        |
