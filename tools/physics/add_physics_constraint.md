# add_physics_constraint

Add a constraint between two bodies in a Physics Asset. Defaults to limited swing (45 deg) and twist (45 deg) with locked linear.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | Yes | Asset path of the Physics Asset. |
| `bone1` | string | Yes | First body bone name. |
| `bone2` | string | Yes | Second body bone name. |
| `swing1_limit_angle` | number | No | Swing1 limit angle in degrees (default: 45). |
| `swing2_limit_angle` | number | No | Swing2 limit angle in degrees (default: 45). |
| `twist_limit_angle` | number | No | Twist limit angle in degrees (default: 45). |
| `disable_collision` | boolean | No | Disable collision between constrained bodies. |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `asset_path` | string | Full asset path |
| `constraint_index` | number | Index of the newly added constraint |
| `bone1` | string | First body bone name |
| `bone2` | string | Second body bone name |
| `swing1_limit_angle` | number | Swing1 limit angle in degrees |
| `swing2_limit_angle` | number | Swing2 limit angle in degrees |
| `twist_limit_angle` | number | Twist limit angle in degrees |
| `constraint_count` | number | Total constraint count after addition |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "add_physics_constraint",
    "arguments": {
      "asset_path": "/Game/PA_Character",
      "bone1": "Spine",
      "bone2": "Head",
      "swing1_limit_angle": 30,
      "swing2_limit_angle": 30,
      "twist_limit_angle": 15,
      "disable_collision": true
    }
  }
}
```

Response:

```json
{
  "asset_path": "/Game/PA_Character.PA_Character",
  "constraint_index": 0,
  "bone1": "Spine",
  "bone2": "Head",
  "swing1_limit_angle": 30.0,
  "swing2_limit_angle": 30.0,
  "twist_limit_angle": 15.0,
  "constraint_count": 1
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing asset_path | "asset_path is required" |
| Missing bone1 | "'bone1' is required" |
| Missing bone2 | "'bone2' is required" |
| Asset not found | "Physics Asset not found: {path}" |
