# list_data_tables

List DataTable assets with their row struct.

## Input

```json
{
  "path_prefix": "/Game/Data",
  "name_filter": "weapon"
}
```

| Parameter     | Required | Description                                                        |
| ------------- | -------- | ------------------------------------------------------------------ |
| `path_prefix` | no       | Content folder prefix to search under (default: `/Game`)           |
| `name_filter` | no       | Case-insensitive substring filter applied to DataTable asset names |

## Output

```json
{
  "count": 2,
  "data_tables": [
    {
      "name": "DT_Weapons",
      "asset_path": "/Game/Data/DT_Weapons",
      "row_struct": "S_Weapon",
      "row_count": 5
    },
    {
      "name": "DT_WeaponMods",
      "asset_path": "/Game/Data/DT_WeaponMods",
      "row_struct": "S_WeaponMod",
      "row_count": 12
    }
  ]
}
```

| Field                      | Type   | Description                                   |
| -------------------------- | ------ | --------------------------------------------- |
| `count`                    | int    | Number of DataTables returned                 |
| `data_tables`              | array  | Array of DataTable info objects               |
| `data_tables[].name`       | string | DataTable asset name                          |
| `data_tables[].asset_path` | string | Full content path to the DataTable            |
| `data_tables[].row_struct` | string | Row struct type used by the DataTable         |
| `data_tables[].row_count`  | int    | Number of rows in the DataTable               |

## Examples

### List all DataTables

```json
{}
```

### List DataTables under a specific path

```json
{"path_prefix": "/Game/Data"}
```

### Filter DataTables by name

```json
{"name_filter": "weapon"}
```

### Combine path and name filter

```json
{
  "path_prefix": "/Game/Data",
  "name_filter": "enemy"
}
```

## Errors

No errors — returns an empty array if no DataTables match.
