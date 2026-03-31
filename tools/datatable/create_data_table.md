# create_data_table

Create a new DataTable asset with a specified row struct.

## Input

```json
{
  "name": "DT_Weapons",
  "path": "/Game/Data",
  "row_struct": "TableRowBase"
}
```

| Parameter    | Required | Description                                                                                                              |
| ------------ | -------- | ------------------------------------------------------------------------------------------------------------------------ |
| `name`       | **yes**  | DataTable asset name (e.g. `DT_Weapons`)                                                                                |
| `path`       | no       | Content folder path (default: `/Game`)                                                                                   |
| `row_struct` | no       | Row struct type (default: `TableRowBase`). Can be a UserDefinedStruct asset path (e.g. `/Game/Structs/S_Weapon`) or a C++ struct name (e.g. `TableRowBase`) |

## Output

```json
{
  "name": "DT_Weapons",
  "asset_path": "/Game/Data/DT_Weapons",
  "class": "DataTable",
  "row_struct": "TableRowBase",
  "row_count": 0
}
```

| Field        | Type   | Description                              |
| ------------ | ------ | ---------------------------------------- |
| `name`       | string | Created DataTable asset name             |
| `asset_path` | string | Full content path to the new DataTable   |
| `class`      | string | Always `DataTable`                       |
| `row_struct` | string | Row struct type assigned to the table    |
| `row_count`  | int    | Number of rows (always `0` on creation)  |

## Examples

### Create a DataTable with default row struct

```json
{"name": "DT_Items"}
```

### Create a DataTable with a C++ struct

```json
{
  "name": "DT_Weapons",
  "path": "/Game/Data",
  "row_struct": "TableRowBase"
}
```

### Create a DataTable using a UserDefinedStruct

```json
{
  "name": "DT_Enemies",
  "path": "/Game/Data",
  "row_struct": "/Game/Structs/S_Enemy"
}
```

## Errors

| Condition              | `isError` | Message                                                  |
| ---------------------- | --------- | -------------------------------------------------------- |
| Missing `name`         | `true`    | `Missing required parameter 'name'`                      |
| Asset already exists   | `true`    | `An asset already exists at '/Game/...'`                 |
| Invalid `path`         | `true`    | `Invalid path '...'. Must start with /Game or /Engine.`  |
| Row struct not found   | `true`    | `Row struct '...' not found`                             |
