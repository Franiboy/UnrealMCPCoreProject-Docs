# delete_data_table_row

Remove a row from a DataTable.

## Input

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Sword"
}
```

| Parameter    | Required | Description                                                        |
| ------------ | -------- | ------------------------------------------------------------------ |
| `asset_path` | **yes**  | Full content path to the DataTable (e.g. `/Game/Data/DT_Weapons`)  |
| `row_name`   | **yes**  | Row name (key) of the row to remove                                |

## Output

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "removed_row": "Sword",
  "remaining_rows": 4
}
```

| Field            | Type   | Description                                    |
| ---------------- | ------ | ---------------------------------------------- |
| `asset_path`     | string | Full content path to the DataTable             |
| `removed_row`    | string | Row name of the removed entry                  |
| `remaining_rows` | int    | Number of rows remaining after deletion        |

## Examples

### Remove a row by name

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Sword"
}
```

### Remove a row from a different DataTable

```json
{
  "asset_path": "/Game/Data/DT_Items",
  "row_name": "HealthPotion"
}
```

## Errors

| Condition              | `isError` | Message                                                  |
| ---------------------- | --------- | -------------------------------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter 'asset_path'`                |
| Missing `row_name`     | `true`    | `Missing required parameter 'row_name'`                  |
| DataTable not found    | `true`    | `DataTable not found at '/Game/...'`                     |
| Row not found          | `true`    | `Row '...' not found in DataTable`                       |
