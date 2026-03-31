# remove_physics_constraint

Remove a constraint from a Physics Asset by index.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | Yes | Asset path of the Physics Asset. |
| `constraint_index` | integer | Yes | Index of the constraint to remove. |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `asset_path` | string | Full asset path |
| `removed_index` | number | Index of the removed constraint |
| `bone1` | string | First body bone name of the removed constraint |
| `bone2` | string | Second body bone name of the removed constraint |
| `constraint_count` | number | Total constraint count after removal |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "remove_physics_constraint",
    "arguments": {
      "asset_path": "/Game/PA_Character",
      "constraint_index": 0
    }
  }
}
```

Response:

```json
{
  "asset_path": "/Game/PA_Character.PA_Character",
  "removed_index": 0,
  "bone1": "Spine",
  "bone2": "Head",
  "constraint_count": 0
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing asset_path | "asset_path is required" |
| Missing constraint_index | "'constraint_index' is required" |
| Asset not found | "Physics Asset not found: {path}" |
| Index out of range | "Constraint index out of range: {index}" |
