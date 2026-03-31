# modify_data_table_row

Modify field values of an existing DataTable row.

## Input

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Sword",
  "values": {
    "Damage": 30,
    "Weight": 4.0
  }
}
```

| Parameter    | Required | Description                                                        |
| ------------ | -------- | ------------------------------------------------------------------ |
| `asset_path` | **yes**  | Full content path to the DataTable (e.g. `/Game/Data/DT_Weapons`)  |
| `row_name`   | **yes**  | Row name (key) of the row to modify                                |
| `values`     | **yes**  | Object of field key-value pairs to update                          |

## Output

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Sword",
  "fields_modified": 2,
  "modified_fields": ["Damage", "Weight"]
}
```

| Field              | Type   | Description                                          |
| ------------------ | ------ | ---------------------------------------------------- |
| `asset_path`       | string | Full content path to the DataTable                   |
| `row_name`         | string | Row name of the modified entry                       |
| `fields_modified`  | int    | Number of fields that were modified                  |
| `modified_fields`  | array  | List of field names that were modified               |
| `errors`           | array  | *(optional)* List of field-level error messages      |

## Examples

### Modify a single field

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Sword",
  "values": {
    "Damage": 50
  }
}
```

### Modify multiple fields

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Sword",
  "values": {
    "DisplayName": "Steel Sword",
    "Damage": 30,
    "Weight": 4.0
  }
}
```

### Partial success with errors

If some fields are valid and others are not, the valid fields are modified and the invalid ones are reported in the `errors` array:

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Sword",
  "fields_modified": 1,
  "modified_fields": ["Damage"],
  "errors": ["Field 'UnknownField' not found in row struct"]
}
```

## Errors

| Condition              | `isError` | Message                                                  |
| ---------------------- | --------- | -------------------------------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter 'asset_path'`                |
| Missing `row_name`     | `true`    | `Missing required parameter 'row_name'`                  |
| Missing `values`       | `true`    | `Missing required parameter 'values'`                    |
| DataTable not found    | `true`    | `DataTable not found at '/Game/...'`                     |
| Row not found          | `true`    | `Row '...' not found in DataTable`                       |
| Field not found        | `true`    | `Field '...' not found in row struct`                    |
