# set_anim_bp_variable

Modify properties of an existing variable in an Animation Blueprint. Can set default value, category, instance editability, and read-only flag. The AnimBP is compiled after changes.

## Input

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "variable_name": "Speed",
  "default_value": "100.0",
  "category": "Movement",
  "instance_editable": true,
  "blueprint_read_only": false
}
```

| Parameter           | Required | Default | Description                                          |
| ------------------- | -------- | ------- | ---------------------------------------------------- |
| `asset_path`        | **yes**  | ---     | Content path to the Animation Blueprint.             |
| `variable_name`     | **yes**  | ---     | Name of the variable to modify.                      |
| `default_value`     | no       | ---     | New default value as a string.                       |
| `category`          | no       | ---     | New category for the variable.                       |
| `instance_editable` | no       | ---     | Whether the variable is editable on instances.       |
| `blueprint_read_only`| no      | ---     | Whether the variable is read-only in Blueprints.     |

At least one of `default_value`, `category`, `instance_editable`, or `blueprint_read_only` must be provided.

## Output

| Field               | Type    | Description                                 |
| ------------------- | ------- | ------------------------------------------- |
| `asset_path`        | string  | Content path to the Animation Blueprint     |
| `variable_name`     | string  | Name of the modified variable               |
| `variable_type`     | string  | Variable type category                      |
| `default_value`     | string  | Current default value                       |
| `category`          | string  | Current category                            |
| `instance_editable` | boolean | Whether instance-editable                   |
| `blueprint_read_only`| boolean| Whether Blueprint read-only                 |

## Examples

### Set default value

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "variable_name": "Speed",
  "default_value": "300.0"
}
```

### Make variable instance-editable with category

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "variable_name": "bIsInAir",
  "instance_editable": true,
  "category": "State"
}
```

### Set multiple properties at once

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "variable_name": "MaxSpeed",
  "default_value": "600.0",
  "category": "Movement",
  "instance_editable": true,
  "blueprint_read_only": false
}
```

## Errors

| Condition               | `isError` | Message                                          |
| ----------------------- | --------- | ------------------------------------------------ |
| Missing `asset_path`    | `true`    | `Missing required parameter: asset_path...`      |
| Missing `variable_name` | `true`    | `Missing required parameter: ...variable_name`   |
| AnimBP not found        | `true`    | `Animation Blueprint not found at '...'`         |
| Variable not found      | `true`    | `Variable '...' not found in '...'`              |
| No properties to change | `true`    | `No properties to change. Provide at least one...`|
