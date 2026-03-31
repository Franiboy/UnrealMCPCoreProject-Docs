# add_curve_table_row

Add a named row (curve) to a Curve Table. Each row is a FRichCurve with optional initial keyframes.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `asset_path` | string | Yes | Asset path of the Curve Table |
| `row_name` | string | Yes | Name of the new row |
| `keys` | array | No | Optional initial keys: [{time, value, interp_mode, tangent_mode, arrive_tangent, leave_tangent}] |

## Example

**Request:**
```json
{
  "tool": "add_curve_table_row",
  "arguments": {
    "asset_path": "/Game/Data/DamageCurveTable.DamageCurveTable",
    "row_name": "DamageMultiplier",
    "keys": [
      { "time": 0.0, "value": 1.0 },
      { "time": 10.0, "value": 2.5 }
    ]
  }
}
```

**Response:**
```json
{
  "asset_path": "/Game/Data/DamageCurveTable.DamageCurveTable",
  "row_name": "DamageMultiplier",
  "key_count": 2,
  "total_rows": 1
}
```

## Notes

- Each row name must be unique within the Curve Table
- If keys are provided, they are applied immediately after row creation
- Returns error if row_name already exists in the table
- Returns error if the asset_path does not point to a valid UCurveTable
