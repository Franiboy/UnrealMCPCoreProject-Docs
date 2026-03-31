# list_asset_types

List all registered UClass types that can exist as assets. Excludes abstract, deprecated, hidden, and transient classes by default.

## Input

```json
{}
```

| Parameter          | Required | Default | Description                                                          |
| ------------------ | -------- | ------- | -------------------------------------------------------------------- |
| `base_class`       | no       | —       | Only include classes derived from this base (e.g. `Texture`).        |
| `name_filter`      | no       | —       | Case-insensitive substring filter on class name.                     |
| `include_abstract` | no       | `false` | Include abstract classes.                                            |
| `limit`            | no       | all     | Maximum number of types to return.                                   |

## Output

| Field           | Type   | Description                              |
| --------------- | ------ | ---------------------------------------- |
| `count`         | number | Number of types returned                 |
| `total_scanned` | number | Total UClasses scanned                   |
| `types`         | array  | Asset type entries (see below)           |
| `base_class`    | string | Base class filter applied (if any)       |
| `name_filter`   | string | Name filter applied (if any)             |

### Type entry

| Field      | Type    | Description                    |
| ---------- | ------- | ------------------------------ |
| `name`     | string  | Class name                     |
| `path`     | string  | Full object path               |
| `parent`   | string  | Parent class name              |
| `abstract` | boolean | Present if class is abstract   |

## Examples

### List all asset types

```json
{}
```

### Filter by base class

```json
{
  "base_class": "Texture"
}
```

### Filter by name

```json
{
  "name_filter": "Material"
}
```

### Limit results

```json
{
  "limit": 20
}
```

## Errors

| Condition               | Error message                    |
| ----------------------- | -------------------------------- |
| Base class not found    | `Base class '<name>' not found`  |
