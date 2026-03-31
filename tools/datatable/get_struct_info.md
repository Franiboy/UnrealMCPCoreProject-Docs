# get_struct_info

Get fields, types, and defaults of a UStruct (UserDefinedStruct or C++ struct).

## Input

```json
{
  "struct_id": "/Game/Structs/S_Weapon"
}
```

| Parameter   | Required | Description                                                                                     |
| ----------- | -------- | ----------------------------------------------------------------------------------------------- |
| `struct_id` | **yes**  | Struct identifier — asset path for a UserDefinedStruct (e.g. `/Game/Structs/S_Weapon`) or C++ struct name (e.g. `TableRowBase`) |

## Output

```json
{
  "name": "S_Weapon",
  "is_user_defined": true,
  "asset_path": "/Game/Structs/S_Weapon",
  "field_count": 4,
  "struct_size": 56,
  "fields": [
    {"name": "DisplayName", "type": "FString", "default_value": ""},
    {"name": "Damage", "type": "int32", "category": "Combat", "default_value": "0"},
    {"name": "Weight", "type": "float", "default_value": "0.0"},
    {"name": "bIsRanged", "type": "bool", "default_value": "false"}
  ]
}
```

| Field                   | Type   | Description                                                |
| ----------------------- | ------ | ---------------------------------------------------------- |
| `name`                  | string | Struct name                                                |
| `is_user_defined`       | bool   | `true` for UserDefinedStruct, `false` for C++ structs     |
| `asset_path`            | string | Full content path *(only present for UserDefinedStructs)* |
| `field_count`           | int    | Number of fields in the struct                             |
| `struct_size`           | int    | Size of the struct in bytes                                |
| `fields`                | array  | Array of field info objects                                |
| `fields[].name`         | string | Field name                                                 |
| `fields[].type`         | string | Field type                                                 |
| `fields[].category`     | string | *(optional)* Field category if defined                     |
| `fields[].default_value`| string | Default value as a string representation                   |

## Examples

### Get info for a UserDefinedStruct

```json
{"struct_id": "/Game/Structs/S_Weapon"}
```

### Get info for a C++ struct

```json
{"struct_id": "TableRowBase"}
```

### Output for a C++ struct (no asset_path)

```json
{
  "name": "TableRowBase",
  "is_user_defined": false,
  "field_count": 0,
  "struct_size": 8,
  "fields": []
}
```

## Errors

| Condition              | `isError` | Message                                                  |
| ---------------------- | --------- | -------------------------------------------------------- |
| Missing `struct_id`    | `true`    | `Missing required parameter 'struct_id'`                 |
| Struct not found       | `true`    | `Struct '...' not found`                                 |
