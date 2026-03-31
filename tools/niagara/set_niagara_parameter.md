# set_niagara_parameter

Set a user-exposed parameter on a Niagara System.

## Input

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "parameter_name": "User.SpawnRate",
  "value": 200.0
}
```

| Parameter        | Required | Description                                                |
| ---------------- | -------- | ---------------------------------------------------------- |
| `system_path`    | **yes**  | Asset path of the Niagara System                           |
| `parameter_name` | **yes**  | Name of the parameter to set (with or without "User." prefix) |
| `value`          | **yes**  | Value to set (format depends on parameter type)            |

### Value formats by type

| Niagara type        | JSON value format                              |
| ------------------- | ---------------------------------------------- |
| `NiagaraFloat`      | number (e.g. `42.0`)                           |
| `NiagaraInt32`      | number (e.g. `10`)                             |
| `NiagaraBool`       | boolean (e.g. `true`)                          |
| Vec2                | `{"x": 1.0, "y": 2.0}`                        |
| Vec3 / Position     | `{"x": 1.0, "y": 2.0, "z": 3.0}`             |
| Vec4                | `{"x": 1.0, "y": 2.0, "z": 3.0, "w": 4.0}`  |
| Color               | `{"r": 1.0, "g": 0.0, "b": 0.0, "a": 1.0}`  |

## Output

```json
{
  "system": "NS_Fire",
  "parameter_name": "User.SpawnRate",
  "type": "NiagaraFloat",
  "value": 200.0
}
```

| Field            | Type   | Description                   |
| ---------------- | ------ | ----------------------------- |
| `system`         | string | System asset name             |
| `parameter_name` | string | Full parameter name           |
| `type`           | string | Niagara type name             |
| `value`          | varies | New value after setting       |

## Examples

### Set a float parameter

```json
{"system_path": "/Game/FX/NS_Fire", "parameter_name": "SpawnRate", "value": 500.0}
```

### Set a color parameter

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "parameter_name": "User.Color",
  "value": {"r": 1.0, "g": 0.0, "b": 0.0, "a": 1.0}
}
```

### Set a vector parameter

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "parameter_name": "Velocity",
  "value": {"x": 0.0, "y": 0.0, "z": 500.0}
}
```

## Errors

| Condition             | Message                                     |
| --------------------- | ------------------------------------------- |
| Missing required      | system_path and parameter_name are required |
| Missing value         | value is required                           |
| System not found      | System not found: ...                       |
| Parameter not found   | Parameter '...' not found. Available: ...   |
| Wrong value format    | Failed to set parameter '...' of type '...' |
