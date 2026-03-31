# modify_user_defined_struct

Add, remove, rename, or change field types in a UserDefinedStruct.

## Input

```json
{
  "asset_path": "/Game/Structs/S_Weapon",
  "operations": [
    {"action": "add", "field_name": "CritChance", "type": "float"},
    {"action": "remove", "field_name": "Weight"},
    {"action": "rename", "field_name": "Damage", "new_name": "BaseDamage"},
    {"action": "change_type", "field_name": "bIsRanged", "type": "int32"}
  ]
}
```

| Parameter                   | Required | Description                                                     |
| --------------------------- | -------- | --------------------------------------------------------------- |
| `asset_path`                | **yes**  | Full content path to the UserDefinedStruct                      |
| `operations`                | **yes**  | Array of operations to apply                                    |
| `operations[].action`       | **yes**  | Operation type: `add`, `remove`, `rename`, or `change_type`     |
| `operations[].field_name`   | **yes**  | Target field name                                               |
| `operations[].type`         | cond.    | Field type — required for `add` and `change_type` actions       |
| `operations[].new_name`     | cond.    | New field name — required for `rename` action                   |

### Actions

| Action        | Required Params            | Description                        |
| ------------- | -------------------------- | ---------------------------------- |
| `add`         | `field_name`, `type`       | Add a new field to the struct      |
| `remove`      | `field_name`               | Remove an existing field           |
| `rename`      | `field_name`, `new_name`   | Rename an existing field           |
| `change_type` | `field_name`, `type`       | Change the type of an existing field |

### Supported Types

`bool`, `int32`, `int64`, `float`, `double`, `FString`, `FName`, `FText`, `FVector`, `FRotator`, `FTransform`, `FLinearColor`, `FColor`, `TSoftObjectPtr`

## Output

```json
{
  "asset_path": "/Game/Structs/S_Weapon",
  "operations_applied": 4,
  "applied": [
    {"action": "add", "field_name": "CritChance", "type": "float"},
    {"action": "remove", "field_name": "Weight"},
    {"action": "rename", "field_name": "Damage", "new_name": "BaseDamage"},
    {"action": "change_type", "field_name": "bIsRanged", "type": "int32"}
  ],
  "field_count": 4
}
```

| Field                | Type   | Description                                          |
| -------------------- | ------ | ---------------------------------------------------- |
| `asset_path`         | string | Full content path to the struct                      |
| `operations_applied` | int    | Number of operations successfully applied            |
| `applied`            | array  | List of successfully applied operations              |
| `errors`             | array  | *(optional)* List of operation-level error messages  |
| `field_count`        | int    | Total number of fields after all operations          |

## Examples

### Add a single field

```json
{
  "asset_path": "/Game/Structs/S_Weapon",
  "operations": [
    {"action": "add", "field_name": "CritChance", "type": "float"}
  ]
}
```

### Remove a field

```json
{
  "asset_path": "/Game/Structs/S_Weapon",
  "operations": [
    {"action": "remove", "field_name": "Weight"}
  ]
}
```

### Rename a field

```json
{
  "asset_path": "/Game/Structs/S_Weapon",
  "operations": [
    {"action": "rename", "field_name": "Damage", "new_name": "BaseDamage"}
  ]
}
```

### Partial success with errors

If some operations succeed and others fail, the successful ones are applied and the failures are reported in the `errors` array:

```json
{
  "asset_path": "/Game/Structs/S_Weapon",
  "operations_applied": 1,
  "applied": [
    {"action": "add", "field_name": "CritChance", "type": "float"}
  ],
  "errors": ["Field 'UnknownField' not found in struct"],
  "field_count": 5
}
```

## Errors

| Condition              | `isError` | Message                                                  |
| ---------------------- | --------- | -------------------------------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter 'asset_path'`                |
| Missing `operations`   | `true`    | `Missing required parameter 'operations'`                |
| Struct not found       | `true`    | `UserDefinedStruct not found at '/Game/...'`             |
| Field not found        | `true`    | `Field '...' not found in struct`                        |
| Unknown action         | `true`    | `Unknown action '...'. Valid: add, remove, rename, change_type` |
| Unsupported type       | `true`    | `Unsupported field type '...'. Valid: bool, int32, ...`  |
