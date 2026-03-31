# create_user_defined_struct

Create a new UserDefinedStruct asset with initial fields.

## Input

```json
{
  "name": "S_Weapon",
  "path": "/Game/Structs",
  "fields": [
    {"name": "DisplayName", "type": "FString"},
    {"name": "Damage", "type": "int32"},
    {"name": "Weight", "type": "float"},
    {"name": "bIsRanged", "type": "bool"}
  ]
}
```

| Parameter       | Required | Description                                                        |
| --------------- | -------- | ------------------------------------------------------------------ |
| `name`          | **yes**  | Struct asset name (e.g. `S_Weapon`)                                |
| `path`          | no       | Content folder path (default: `/Game`)                             |
| `fields`        | **yes**  | Array of field definitions (must contain at least one field)       |
| `fields[].name` | **yes**  | Field name                                                         |
| `fields[].type` | **yes**  | Field type. See supported types below                              |

### Supported Types

`bool`, `int32`, `int64`, `float`, `double`, `FString`, `FName`, `FText`, `FVector`, `FRotator`, `FTransform`, `FLinearColor`, `FColor`, `TSoftObjectPtr`

## Output

```json
{
  "name": "S_Weapon",
  "asset_path": "/Game/Structs/S_Weapon",
  "class": "UserDefinedStruct",
  "field_count": 4,
  "fields": [
    {"name": "DisplayName", "type": "FString"},
    {"name": "Damage", "type": "int32"},
    {"name": "Weight", "type": "float"},
    {"name": "bIsRanged", "type": "bool"}
  ]
}
```

| Field            | Type   | Description                                    |
| ---------------- | ------ | ---------------------------------------------- |
| `name`           | string | Created struct asset name                      |
| `asset_path`     | string | Full content path to the new struct            |
| `class`          | string | Always `UserDefinedStruct`                     |
| `field_count`    | int    | Number of fields in the struct                 |
| `fields`         | array  | Array of field definitions                     |
| `fields[].name`  | string | Field name                                     |
| `fields[].type`  | string | Field type                                     |

## Examples

### Create a simple struct with two fields

```json
{
  "name": "S_Item",
  "fields": [
    {"name": "DisplayName", "type": "FString"},
    {"name": "Value", "type": "int32"}
  ]
}
```

### Create a struct with vector and color fields

```json
{
  "name": "S_SpawnPoint",
  "path": "/Game/Structs",
  "fields": [
    {"name": "Location", "type": "FVector"},
    {"name": "Rotation", "type": "FRotator"},
    {"name": "DebugColor", "type": "FLinearColor"}
  ]
}
```

### Create a struct with a soft object reference

```json
{
  "name": "S_LootEntry",
  "path": "/Game/Structs",
  "fields": [
    {"name": "ItemName", "type": "FName"},
    {"name": "Mesh", "type": "TSoftObjectPtr"},
    {"name": "DropChance", "type": "float"},
    {"name": "bIsRare", "type": "bool"}
  ]
}
```

## Errors

| Condition              | `isError` | Message                                                  |
| ---------------------- | --------- | -------------------------------------------------------- |
| Missing `name`         | `true`    | `Missing required parameter 'name'`                      |
| Missing `fields`       | `true`    | `Missing required parameter 'fields'`                    |
| Empty `fields`         | `true`    | `'fields' array must contain at least one field`         |
| Asset already exists   | `true`    | `An asset already exists at '/Game/...'`                 |
| Invalid `path`         | `true`    | `Invalid path '...'. Must start with /Game or /Engine.`  |
| Unsupported type       | `true`    | `Unsupported field type '...'. Valid: bool, int32, ...`  |
