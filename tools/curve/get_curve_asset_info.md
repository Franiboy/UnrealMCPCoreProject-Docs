# get_curve_asset_info

Inspect a Curve asset: keys (time, value, interpolation, tangent), time/value ranges, and type. Supports Float, Vector, LinearColor, and CurveTable. Optionally evaluate the curve at a given time.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `asset_path` | string | Yes | Asset path of the Curve asset |
| `eval_time` | number | No | Optional time to evaluate the curve at |

## Example

**Request:**
```json
{
  "tool": "get_curve_asset_info",
  "arguments": {
    "asset_path": "/Game/Curves/MyCurveFloat.MyCurveFloat",
    "eval_time": 0.5
  }
}
```

**Response:**
```json
{
  "name": "MyCurveFloat",
  "asset_path": "/Game/Curves/MyCurveFloat.MyCurveFloat",
  "type": "CurveFloat",
  "curve": {
    "name": "Value",
    "key_count": 3,
    "min_time": 0.0,
    "max_time": 1.0,
    "min_value": 0.0,
    "max_value": 1.0,
    "keys": [
      {
        "time": 0.0,
        "value": 0.0,
        "interp_mode": "Cubic",
        "tangent_mode": "Auto",
        "arrive_tangent": 0.0,
        "leave_tangent": 0.0
      }
    ]
  },
  "eval_time": 0.5,
  "eval_value": 0.5
}
```

## Notes

- For CurveFloat: returns a single `curve` object
- For CurveVector: returns `curves` array with 3 entries (X, Y, Z); eval_value is [x, y, z]
- For CurveLinearColor: returns `curves` array with 4 entries (R, G, B, A); eval_value is [r, g, b, a]
- For CurveTable: returns `rows` array with row_name, key_count, keys; plus row_count and curve_table_mode
- Each key includes: time, value, interp_mode (Linear/Constant/Cubic/None), tangent_mode (Auto/User/Break/None/SmartAuto), arrive_tangent, leave_tangent
