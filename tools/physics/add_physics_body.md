# add_physics_body

Add a physics body to a Physics Asset with a collision shape (sphere, box, or capsule).

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | Yes | Asset path of the Physics Asset. |
| `bone_name` | string | Yes | Bone name for the body. |
| `shape_type` | string | Yes | Shape type: `sphere`, `box`, or `capsule`. |
| `radius` | number | No | Radius for sphere (default: 10) or capsule (default: 5). |
| `length` | number | No | Length for capsule (default: 20). |
| `extent_x` | number | No | Box half-extent X (default: 10). |
| `extent_y` | number | No | Box half-extent Y (default: 10). |
| `extent_z` | number | No | Box half-extent Z (default: 10). |
| `physics_type` | string | No | Physics type: `Default`, `Kinematic`, or `Simulated`. |
| `mass_in_kg` | number | No | Mass override in kg. |
| `linear_damping` | number | No | Linear damping value. |
| `angular_damping` | number | No | Angular damping value. |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `asset_path` | string | Full asset path |
| `body_index` | number | Index of the newly added body |
| `bone_name` | string | Bone name the body is attached to |
| `shape_type` | string | Shape type that was created |
| `physics_type` | string | Physics type of the body |
| `body_count` | number | Total body count after addition |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "add_physics_body",
    "arguments": {
      "asset_path": "/Game/PA_Character",
      "bone_name": "Spine",
      "shape_type": "sphere",
      "radius": 12.0,
      "physics_type": "Simulated",
      "mass_in_kg": 5.0
    }
  }
}
```

Response:

```json
{
  "asset_path": "/Game/PA_Character.PA_Character",
  "body_index": 0,
  "bone_name": "Spine",
  "shape_type": "sphere",
  "physics_type": "Simulated",
  "body_count": 1
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing asset_path | "asset_path is required" |
| Missing bone_name | "bone_name is required" |
| Missing shape_type | "shape_type is required" |
| Asset not found | "Physics Asset not found: {path}" |
| Body already exists | "Body already exists for bone: {bone_name}" |
| Unknown shape_type | "Unknown shape_type: {shape_type}. Must be sphere, box, or capsule" |
