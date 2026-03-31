# get_class_hierarchy

Get the inheritance chain for any UClass, from the given class up to UObject. Also lists implemented interfaces.

## Input

```json
{
  "class_name": "Character"
}
```

| Parameter    | Required | Default | Description                                                        |
| ------------ | -------- | ------- | ------------------------------------------------------------------ |
| `class_name` | **yes**  | —       | Class name (short like `Actor` or full path like `/Script/Engine.Actor`). |

## Output

| Field        | Type   | Description                                  |
| ------------ | ------ | -------------------------------------------- |
| `class_name` | string | Resolved class name                          |
| `class_path` | string | Full object path                             |
| `depth`      | number | Number of classes in the hierarchy chain     |
| `hierarchy`  | array  | Ordered list from class to UObject (see below) |
| `interfaces` | array  | Implemented interfaces (if any)              |

### Hierarchy entry

| Field      | Type    | Description                    |
| ---------- | ------- | ------------------------------ |
| `name`     | string  | Class name                     |
| `path`     | string  | Full object path               |
| `abstract` | boolean | Present if class is abstract   |
| `native`   | boolean | Present if class is native C++ |

## Examples

### Short name

```json
{
  "class_name": "Character"
}
```

### Full path

```json
{
  "class_name": "/Script/Engine.Actor"
}
```

## Errors

| Condition            | Error message               |
| -------------------- | --------------------------- |
| Missing `class_name` | `class_name is required`    |
| Class not found      | `Class '<name>' not found`  |
