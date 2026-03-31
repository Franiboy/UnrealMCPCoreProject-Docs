# create_curve_table

Create a new empty Curve Table asset. Rows can be added afterward using add_curve_table_row.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Name of the new Curve Table |
| `path` | string | No | Content path (default: "/Game") |

## Example

**Request:**
```json
{
  "tool": "create_curve_table",
  "arguments": {
    "name": "DamageCurveTable",
    "path": "/Game/Data"
  }
}
```

**Response:**
```json
{
  "name": "DamageCurveTable",
  "asset_path": "/Game/Data/DamageCurveTable.DamageCurveTable",
  "type": "CurveTable",
  "row_count": 0
}
```

## Notes

- Creates a UCurveTable asset using UCurveTableFactory
- The table is created empty; use add_curve_table_row to add rows
- Each row in a Curve Table is a named FRichCurve
- Returns error if an asset with the same name already exists at the path
