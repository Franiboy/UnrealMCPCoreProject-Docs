# set_curve_keys

Set keyframes on a Curve asset. Can replace all existing keys or add/update individual keys. Supports Float, Vector, and LinearColor curves.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `asset_path` | string | Yes | Asset path of the Curve asset |
| `keys` | array | Yes | Array of keys: {time, value, interp_mode, tangent_mode, arrive_tangent, leave_tangent} |
| `replace_all` | boolean | No | Replace all existing keys (default: true). If false, adds/updates keys at specified times |
| `curve_index` | integer | No | For vector/color curves: sub-curve index (0=X/R, 1=Y/G, 2=Z/B, 3=A). Omit to modify all sub-curves |

## Example

**Request:**
```json
{
  "tool": "set_curve_keys",
  "arguments": {
    "asset_path": "/Game/Curves/MyDamageCurve.MyDamageCurve",
    "keys": [
      { "time": 0.0, "value": 0.0 },
      { "time": 0.5, "value": 0.75 },
      { "time": 1.0, "value": 1.0 }
    ],
    "replace_all": true
  }
}
```

**Response:**
```json
{
  "asset_path": "/Game/Curves/MyDamageCurve.MyDamageCurve",
  "type": "CurveFloat",
  "key_count": 3,
  "replaced_all": true
}
```

## Notes

- When replace_all is true, all existing keys are removed before adding new ones
- When replace_all is false, keys at existing times are updated, new times are added
- For vector/color curves with scalar values and no curve_index, the value is applied to all sub-curves
- Supported interp_mode values: "linear" (default), "constant", "cubic"
- Supported tangent_mode values: "auto" (default), "user", "break"
