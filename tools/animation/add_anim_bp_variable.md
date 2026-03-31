# add_anim_bp_variable

Add a variable to an Animation Blueprint. Supports all Blueprint variable types. The AnimBP is compiled after adding the variable.

## Input

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "variable_name": "Speed",
  "variable_type": "float",
  "default_value": "0.0",
  "category": "Movement"
}
```

| Parameter       | Required | Default | Description                                                           |
| --------------- | -------- | ------- | --------------------------------------------------------------------- |
| `asset_path`    | **yes**  | ---     | Content path to the Animation Blueprint.                              |
| `variable_name` | **yes**  | ---     | Name for the new variable.                                            |
| `variable_type` | **yes**  | ---     | Type string (`"bool"`, `"int"`, `"float"`, `"FVector"`, etc.).        |
| `default_value` | no       | ---     | Default value as a string.                                            |
| `category`      | no       | ---     | Category for organizing the variable in the details panel.            |

## Output

| Field             | Type   | Description                              |
| ----------------- | ------ | ---------------------------------------- |
| `asset_path`      | string | Content path to the Animation Blueprint  |
| `variable_name`   | string | Name of the created variable             |
| `variable_type`   | string | Type string as provided                  |
| `resolved_type`   | string | Resolved UE pin type string              |
| `default_value`   | string | Default value (if set)                   |
| `category`        | string | Category (if set)                        |
| `total_variables` | number | Total variable count after addition      |

## Examples

### Add a float variable

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "variable_name": "Speed",
  "variable_type": "float"
}
```

### Add a bool variable with default and category

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "variable_name": "bIsInAir",
  "variable_type": "bool",
  "default_value": "false",
  "category": "State"
}
```

### Add a vector variable

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "variable_name": "Velocity",
  "variable_type": "FVector"
}
```

## Errors

| Condition              | `isError` | Message                                           |
| ---------------------- | --------- | ------------------------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter: asset_path...`       |
| Missing `variable_name`| `true`    | `Missing required parameter: ...variable_name`    |
| Missing `variable_type`| `true`    | `Missing required parameter: ...variable_type`    |
| AnimBP not found       | `true`    | `Animation Blueprint not found at '...'`          |
| Duplicate variable     | `true`    | `Variable '...' already exists in '...'`          |
| Invalid type           | `true`    | Type parsing error message                        |
