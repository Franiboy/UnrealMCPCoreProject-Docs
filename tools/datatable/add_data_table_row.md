# add_data_table_row

Add a new row to a DataTable with field values.

## Input

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Axe",
  "values": {
    "DisplayName": "Battle Axe",
    "Damage": 40,
    "Weight": 6.0
  }
}
```

| Parameter    | Required | Description                                                        |
| ------------ | -------- | ------------------------------------------------------------------ |
| `asset_path` | **yes**  | Full content path to the DataTable (e.g. `/Game/Data/DT_Weapons`)  |
| `row_name`   | **yes**  | Unique row name (key) for the new entry                            |
| `values`     | no       | Object of field key-value pairs to set on the new row              |

## Output

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Axe",
  "fields_set": 3,
  "total_rows": 6,
  "set_fields": ["DisplayName", "Damage", "Weight"]
}
```

| Field         | Type   | Description                                    |
| ------------- | ------ | ---------------------------------------------- |
| `asset_path`  | string | Full content path to the DataTable             |
| `row_name`    | string | Row name of the newly added entry              |
| `fields_set`  | int    | Number of fields that were set                 |
| `total_rows`  | int    | Total number of rows after adding              |
| `set_fields`  | array  | List of field names that were set              |

## Examples

### Add a row with values

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Axe",
  "values": {
    "DisplayName": "Battle Axe",
    "Damage": 40,
    "Weight": 6.0
  }
}
```

### Add an empty row (defaults only)

```json
{
  "asset_path": "/Game/Data/DT_Weapons",
  "row_name": "Placeholder"
}
```

### Add a row with a single field

```json
{
  "asset_path": "/Game/Data/DT_Items",
  "row_name": "HealthPotion",
  "values": {
    "DisplayName": "Health Potion"
  }
}
```

## Errors

| Condition              | `isError` | Message                                                  |
| ---------------------- | --------- | -------------------------------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter 'asset_path'`                |
| Missing `row_name`     | `true`    | `Missing required parameter 'row_name'`                  |
| DataTable not found    | `true`    | `DataTable not found at '/Game/...'`                     |
| Row already exists     | `true`    | `Row '...' already exists in DataTable`                  |
| No row struct          | `true`    | `DataTable has no row struct assigned`                   |
