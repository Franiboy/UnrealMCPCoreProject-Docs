# get_class_info

Get detailed info about a UClass: properties (name, type, flags, category) and functions (name, return type, parameters, flags). Works with native C++ classes and Blueprint classes.

## Input

```json
{
  "class_name": "Actor"
}
```

| Parameter            | Required | Default | Description                                                               |
| -------------------- | -------- | ------- | ------------------------------------------------------------------------- |
| `class_name`         | **yes**  | —       | Class name (short or full path).                                          |
| `include_inherited`  | no       | `false` | Include properties and functions from parent classes.                     |
| `include_properties` | no       | `true`  | Include the properties list.                                              |
| `include_functions`  | no       | `true`  | Include the functions list.                                               |
| `limit`              | no       | all     | Max number of properties and functions to return each.                    |

## Output

| Field              | Type    | Description                                    |
| ------------------ | ------- | ---------------------------------------------- |
| `class_name`       | string  | Resolved class name                            |
| `class_path`       | string  | Full object path                               |
| `parent_class`     | string  | Parent class name                              |
| `is_abstract`      | boolean | Whether the class is abstract                  |
| `is_native`        | boolean | Whether the class is native C++                |
| `is_deprecated`    | boolean | Whether the class is deprecated                |
| `include_inherited`| boolean | Whether inherited members are included         |
| `num_properties`   | number  | Count of properties returned                   |
| `properties`       | array   | Property details (see below)                   |
| `num_functions`    | number  | Count of functions returned                    |
| `functions`        | array   | Function details (see below)                   |

### Property entry

| Field        | Type   | Description                                      |
| ------------ | ------ | ------------------------------------------------ |
| `name`       | string | Property name                                    |
| `type`       | string | C++ type (e.g. `int32`, `FString`, `AActor*`)    |
| `defined_in` | string | Owner class name (if inherited)                  |
| `category`   | string | Editor category (if set)                         |
| `flags`      | array  | Property flags: Edit, BlueprintVisible, etc.     |

### Function entry

| Field        | Type   | Description                                      |
| ------------ | ------ | ------------------------------------------------ |
| `name`       | string | Function name                                    |
| `defined_in` | string | Owner class name (if inherited)                  |
| `return_type`| string | Return type or `void`                            |
| `flags`      | array  | Function flags: BlueprintCallable, Static, etc.  |
| `access`     | string | `Public`, `Protected`, or `Private`              |
| `parameters` | array  | Function parameters (name, type, out, ref)       |

## Examples

### Own properties only

```json
{
  "class_name": "Actor"
}
```

### Include inherited, limit to 10 each

```json
{
  "class_name": "Character",
  "include_inherited": true,
  "limit": 10
}
```

### Functions only

```json
{
  "class_name": "Actor",
  "include_properties": false
}
```

## Errors

| Condition            | Error message               |
| -------------------- | --------------------------- |
| Missing `class_name` | `class_name is required`    |
| Class not found      | `Class '<name>' not found`  |
