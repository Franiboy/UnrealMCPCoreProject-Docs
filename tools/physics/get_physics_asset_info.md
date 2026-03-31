# get_physics_asset_info

Inspect a Physics Asset: bodies (bone, shape, mass, damping, physics type), constraints (bones, linear/angular limits), and overall counts.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | Yes | Asset path of the Physics Asset. |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Asset name |
| `asset_path` | string | Full asset path |
| `body_count` | number | Total number of physics bodies |
| `constraint_count` | number | Total number of constraints |
| `bodies` | array | Array of body objects (see below) |
| `constraints` | array | Array of constraint objects (see below) |

### body object

| Field | Type | Description |
|-------|------|-------------|
| `index` | number | Body index in the Physics Asset |
| `bone_name` | string | Skeleton bone this body is attached to |
| `physics_type` | string | Physics type: "Default", "Kinematic", or "Simulated" |
| `simulate_physics` | boolean | Whether physics simulation is enabled |
| `enable_gravity` | boolean | Whether gravity is enabled |
| `linear_damping` | number | Linear damping value |
| `angular_damping` | number | Angular damping value |
| `mass_in_kg` | number | Mass in kilograms |
| `override_mass` | boolean | Whether mass is manually overridden |
| `collision_profile_name` | string | Collision profile name |
| `shapes` | array | Array of shape descriptors |
| `shape_count` | number | Number of shapes on this body |

### constraint object

| Field | Type | Description |
|-------|------|-------------|
| `index` | number | Constraint index in the Physics Asset |
| `joint_name` | string | Joint name |
| `bone1` | string | First body bone name |
| `bone2` | string | Second body bone name |
| `linear_limit` | object | Linear limit settings (motion types, limit distance) |
| `cone_limit` | object | Cone/swing limit settings (swing1/swing2 angles) |
| `twist_limit` | object | Twist limit settings (twist angle) |
| `disable_collision` | boolean | Whether collision between constrained bodies is disabled |
| `parent_dominates` | boolean | Whether the parent body dominates |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "get_physics_asset_info",
    "arguments": {
      "asset_path": "/Game/PA_Character"
    }
  }
}
```

Response:

```json
{
  "name": "PA_Character",
  "asset_path": "/Game/PA_Character.PA_Character",
  "body_count": 2,
  "constraint_count": 1,
  "bodies": [
    {
      "index": 0,
      "bone_name": "Spine",
      "physics_type": "Simulated",
      "simulate_physics": true,
      "enable_gravity": true,
      "linear_damping": 0.01,
      "angular_damping": 0.0,
      "mass_in_kg": 1.0,
      "override_mass": false,
      "collision_profile_name": "PhysicsActor",
      "shapes": [
        { "type": "Sphere", "radius": 10.0 }
      ],
      "shape_count": 1
    },
    {
      "index": 1,
      "bone_name": "Head",
      "physics_type": "Simulated",
      "simulate_physics": true,
      "enable_gravity": true,
      "linear_damping": 0.01,
      "angular_damping": 0.0,
      "mass_in_kg": 1.0,
      "override_mass": false,
      "collision_profile_name": "PhysicsActor",
      "shapes": [
        { "type": "Sphere", "radius": 8.0 }
      ],
      "shape_count": 1
    }
  ],
  "constraints": [
    {
      "index": 0,
      "joint_name": "Head",
      "bone1": "Spine",
      "bone2": "Head",
      "linear_limit": {
        "x_motion": "Locked",
        "y_motion": "Locked",
        "z_motion": "Locked",
        "limit": 0.0
      },
      "cone_limit": {
        "swing1_motion": "Limited",
        "swing2_motion": "Limited",
        "swing1_limit_angle": 45.0,
        "swing2_limit_angle": 45.0
      },
      "twist_limit": {
        "twist_motion": "Limited",
        "twist_limit_angle": 45.0
      },
      "disable_collision": true,
      "parent_dominates": false
    }
  ]
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing asset_path | "asset_path is required" |
| Asset not found | "Physics Asset not found: {path}" |
