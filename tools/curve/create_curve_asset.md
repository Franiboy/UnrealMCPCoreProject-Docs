# create_curve_asset

Create a new Curve asset: Float, Vector, or LinearColor. Optionally provide initial keyframes.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Name of the new Curve asset |
| `path` | string | No | Content path (default: "/Game") |
| `curve_type` | string | No | Curve type: "float", "vector", "linear_color" (default: "float") |
| `keys` | array | No | Optional initial keys. Each: {time, value, interp_mode, tangent_mode, arrive_tangent, leave_tangent}. For vector/color, value can be [x,y,z] or [r,g,b,a] |

## Example

**Request:**
```json
{
  "tool": "create_curve_asset",
  "arguments": {
    "name": "MyDamageCurve",
    "path": "/Game/Curves",
    "curve_type": "float",
    "keys": [
      { "time": 0.0, "value": 0.0 },
      { "time": 1.0, "value": 1.0 }
    ]
  }
}
```

**Response:**
```json
{
  "name": "MyDamageCurve",
  "asset_path": "/Game/Curves/MyDamageCurve.MyDamageCurve",
  "type": "CurveFloat"
}
```

## Notes

- Uses UCurveFloatFactory, UCurveVectorFactory, or UCurveLinearColorFactory
- If keys are provided, they are applied immediately after creation
- For vector curves, each key's value can be a scalar (applied to all axes) or [x, y, z] array
- For linear_color curves, value can be [r, g, b, a] array
- Returns error if an asset with the same name already exists at the path
