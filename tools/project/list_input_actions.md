# list_input_actions

List Enhanced Input Actions and Input Mapping Contexts defined in the project. Returns action names, value types, triggers, modifiers, and mapping context details with key bindings.

## Input

```json
{}
```

| Parameter          | Required | Default | Description                                                              |
| ------------------ | -------- | ------- | ------------------------------------------------------------------------ |
| `include_contexts` | no       | `true`  | Include Input Mapping Context details with key bindings.                 |
| `name_filter`      | no       | —       | Case-insensitive substring filter on action/context name.                |

## Output

| Field          | Type   | Description                         |
| -------------- | ------ | ----------------------------------- |
| `num_actions`  | number | Number of Input Actions found       |
| `actions`      | array  | Array of action objects             |
| `num_contexts` | number | Number of Mapping Contexts found    |
| `contexts`     | array  | Array of mapping context objects    |
| `name_filter`  | string | Name filter applied (if any)        |

### Action object

| Field          | Type    | Description                                            |
| -------------- | ------- | ------------------------------------------------------ |
| `name`         | string  | Asset name                                             |
| `path`         | string  | Full asset path                                        |
| `value_type`   | string  | `Boolean`, `Axis1D`, `Axis2D`, or `Axis3D`            |
| `description`  | string  | Action description (if set)                            |
| `consume_input`| boolean | Whether the action consumes input                      |
| `triggers`     | array   | Trigger class names (if any)                           |
| `modifiers`    | array   | Modifier class names (if any)                          |

### Mapping context object

| Field          | Type   | Description                           |
| -------------- | ------ | ------------------------------------- |
| `name`         | string | Asset name                            |
| `path`         | string | Full asset path                       |
| `description`  | string | Context description (if set)          |
| `num_mappings` | number | Number of key mappings                |
| `mappings`     | array  | Array of mapping objects              |

### Mapping object

| Field       | Type   | Description                         |
| ----------- | ------ | ----------------------------------- |
| `action`    | string | Name of the mapped Input Action     |
| `key`       | string | Display name of the bound key       |
| `triggers`  | array  | Mapping-level trigger class names   |
| `modifiers` | array  | Mapping-level modifier class names  |

## Examples

### List all actions and contexts

```json
{}
```

### Actions only (no contexts)

```json
{
  "include_contexts": false
}
```

### Filter by name

```json
{
  "name_filter": "Jump"
}
```

## Errors

| Condition                 | Error message                    |
| ------------------------- | -------------------------------- |
| Asset Registry unavailable | `Asset Registry not available`  |
