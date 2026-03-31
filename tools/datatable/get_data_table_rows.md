# get_data_table_rows

Get all rows (or filtered) from a DataTable.

## Input

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Sword",
  "limit": 10
}
```

| Parameter    | Required | Description                                                    |
| ------------ | -------- | -------------------------------------------------------------- |
| `asset_path` | **yes**  | Full content path to the DataTable (e.g. `/Game/Data/DT_Weapons`) |
| `row_name`   | no       | Filter to a single row by its row name                         |
| `limit`      | no       | Maximum number of rows to return (integer)                     |

## Output

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_struct": "S_Weapon",
  "total_rows": 5,
  "returned_rows": 1,
  "rows": [
    {
      "row_name": "Sword",
      "values": {
        "DisplayName": "Iron Sword",
        "Damage": 25,
        "Weight": 3.5
      }
    }
  ]
}
```

| Field              | Type   | Description                                      |
| ------------------ | ------ | ------------------------------------------------ |
| `asset_path`       | string | Full content path to the DataTable               |
| `row_struct`       | string | Row struct type used by the DataTable            |
| `total_rows`       | int    | Total number of rows in the DataTable            |
| `returned_rows`    | int    | Number of rows returned in the response          |
| `rows`             | array  | Array of row objects                             |
| `rows[].row_name`  | string | Row name (key) of this entry                     |
| `rows[].values`    | object | Key-value pairs of the row's field values        |

## Examples

### Get all rows from a DataTable

```json
{"asset_path": "/Game/Data/DT_Weapons"}
```

### Get a specific row by name

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Sword"
}
```

### Limit the number of returned rows

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "limit": 5
}
```

## Errors

| Condition              | `isError` | Message                                                  |
| ---------------------- | --------- | -------------------------------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter 'asset_path'`                |
| DataTable not found    | `true`    | `DataTable not found at '/Game/...'`                     |
| Row not found          | `true`    | `Row '...' not found in DataTable`                       |
