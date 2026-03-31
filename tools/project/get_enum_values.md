# get_enum_values

Get all values and display names of a UEnum. Accepts short names or full paths.

## Input

```json
{
  "enum_name": "ECollisionChannel"
}
```

| Parameter     | Required | Default | Description                                                              |
| ------------- | -------- | ------- | ------------------------------------------------------------------------ |
| `enum_name`   | **yes**  | —       | Enum name (short like `ECollisionChannel` or full path).                 |
| `exclude_max` | no       | `true`  | Exclude the auto-generated `_MAX` sentinel entry.                        |

## Output

| Field       | Type    | Description                           |
| ----------- | ------- | ------------------------------------- |
| `enum_name` | string  | Resolved enum name                    |
| `enum_path` | string  | Full object path                      |
| `has_max`   | boolean | Whether the enum has a `_MAX` entry   |
| `count`     | number  | Number of values returned             |
| `values`    | array   | Enum value entries (see below)        |

### Value entry

| Field          | Type   | Description                          |
| -------------- | ------ | ------------------------------------ |
| `name`         | string | Short name (e.g. `WorldStatic`)      |
| `full_name`    | string | Full qualified name                  |
| `value`        | number | Numeric value                        |
| `display_name` | string | Localized display name               |

## Examples

### Get enum values (excluding _MAX)

```json
{
  "enum_name": "ECollisionChannel"
}
```

### Include _MAX entry

```json
{
  "enum_name": "ECollisionChannel",
  "exclude_max": false
}
```

### Full path

```json
{
  "enum_name": "/Script/Engine.ECollisionChannel"
}
```

## Errors

| Condition           | Error message              |
| ------------------- | -------------------------- |
| Missing `enum_name` | `enum_name is required`    |
| Enum not found      | `Enum '<name>' not found`  |
