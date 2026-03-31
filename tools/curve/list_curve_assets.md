# list_curve_assets

List Curve Float, Curve Vector, Curve Linear Color, and Curve Table assets. Returns name, path, type, and key count for each.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `path_prefix` | string | No | Content path prefix to filter (default: "/Game") |
| `name_filter` | string | No | Filter by name substring, case-insensitive (default: "") |
| `type_filter` | string | No | Filter by type: "all", "float", "vector", "linear_color", "curve_table" (default: "all") |
| `include_engine` | boolean | No | Include engine/plugin content (default: false) |

## Example

**Request:**
```json
{
  "tool": "list_curve_assets",
  "arguments": {
    "type_filter": "float"
  }
}
```

**Response:**
```json
{
  "curve_assets": [
    {
      "name": "MyCurveFloat",
      "asset_path": "/Game/Curves/MyCurveFloat.MyCurveFloat",
      "type": "CurveFloat",
      "key_count": 4
    }
  ],
  "count": 1
}
```

## Notes

- Scans the Asset Registry for UCurveFloat, UCurveVector, UCurveLinearColor, and UCurveTable assets
- Results include the key_count for each curve (total keys across all sub-curves for vector/color types)
- Use type_filter to narrow results to a specific curve type
