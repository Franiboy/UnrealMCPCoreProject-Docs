# set_physics_constraint_property

Set constraint properties: linear/angular motion types, limits, stiffness, damping, disable_collision, parent_dominates.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | Yes | Asset path of the Physics Asset. |
| `constraint_index` | integer | Yes | Index of the constraint. |
| `properties` | object | Yes | Object with property key/value pairs. |

### Supported properties

| Property | Type | Description |
|----------|------|-------------|
| `linear_x_motion` | string | Linear X motion: "Free", "Limited", or "Locked" |
| `linear_y_motion` | string | Linear Y motion: "Free", "Limited", or "Locked" |
| `linear_z_motion` | string | Linear Z motion: "Free", "Limited", or "Locked" |
| `linear_limit` | number | Linear limit distance |
| `swing1_motion` | string | Swing1 motion: "Free", "Limited", or "Locked" |
| `swing2_motion` | string | Swing2 motion: "Free", "Limited", or "Locked" |
| `swing1_limit_angle` | number | Swing1 limit angle in degrees |
| `swing2_limit_angle` | number | Swing2 limit angle in degrees |
| `twist_motion` | string | Twist motion: "Free", "Limited", or "Locked" |
| `twist_limit_angle` | number | Twist limit angle in degrees |
| `disable_collision` | boolean | Disable collision between constrained bodies |
| `parent_dominates` | boolean | Whether the parent body dominates |
| `linear_stiffness` | number | Linear drive stiffness |
| `linear_damping` | number | Linear drive damping |
| `angular_stiffness` | number | Angular drive stiffness |
| `angular_damping` | number | Angular drive damping |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `asset_path` | string | Full asset path |
| `constraint_index` | number | Index of the modified constraint |
| `properties_set` | number | Number of properties that were set |
| `values` | object | Object with the property values that were applied |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "set_physics_constraint_property",
    "arguments": {
      "asset_path": "/Game/PA_Character",
      "constraint_index": 0,
      "properties": {
        "swing1_limit_angle": 60,
        "swing2_limit_angle": 45,
        "twist_limit_angle": 30,
        "disable_collision": true
      }
    }
  }
}
```

Response:

```json
{
  "asset_path": "/Game/PA_Character.PA_Character",
  "constraint_index": 0,
  "properties_set": 4,
  "values": {
    "swing1_limit_angle": 60.0,
    "swing2_limit_angle": 45.0,
    "twist_limit_angle": 30.0,
    "disable_collision": true
  }
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing asset_path | "asset_path is required" |
| Missing constraint_index | "'constraint_index' is required" |
| Missing properties | "'properties' object is required" |
| Asset not found | "Physics Asset not found: {path}" |
| Index out of range | "Constraint index out of range: {index}" |
| No recognized properties | "No recognized properties in 'properties' object" |
