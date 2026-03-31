# set_physics_body_property

Set physics body properties (physics_type, simulate_physics, enable_gravity, linear_damping, angular_damping, mass_in_kg, collision_profile_name).

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | Yes | Asset path of the Physics Asset. |
| `bone_name` | string | No | Bone name of the body. |
| `body_index` | integer | No | Body index (alternative to bone_name). |
| `properties` | object | Yes | Object with property key/value pairs. |

> **Note:** At least one of `bone_name` or `body_index` must be provided.

### Supported properties

| Property | Type | Description |
|----------|------|-------------|
| `physics_type` | string | Physics type: "Default", "Kinematic", or "Simulated" |
| `simulate_physics` | boolean | Enable or disable physics simulation |
| `enable_gravity` | boolean | Enable or disable gravity |
| `linear_damping` | number | Linear damping value |
| `angular_damping` | number | Angular damping value |
| `mass_in_kg` | number | Mass in kilograms |
| `collision_profile_name` | string | Collision profile name |

## Response

Returns a JSON object with:

| Field | Type | Description |
|-------|------|-------------|
| `asset_path` | string | Full asset path |
| `body_index` | number | Index of the modified body |
| `bone_name` | string | Bone name of the modified body |
| `properties_set` | number | Number of properties that were set |
| `values` | object | Object with the property values that were applied |

## Example

```json
{
  "method": "tools/call",
  "params": {
    "name": "set_physics_body_property",
    "arguments": {
      "asset_path": "/Game/PA_Character",
      "bone_name": "Spine",
      "properties": {
        "mass_in_kg": 10.0,
        "linear_damping": 0.5,
        "angular_damping": 0.3
      }
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
  "properties_set": 3,
  "values": {
    "mass_in_kg": 10.0,
    "linear_damping": 0.5,
    "angular_damping": 0.3
  }
}
```

## Errors

| Condition | Error |
|-----------|-------|
| Missing asset_path | "asset_path is required" |
| Missing properties | "'properties' object is required" |
| Missing body identifier | "Either 'bone_name' or 'body_index' is required" |
| Body not found | "Body not found: {bone_name or index}" |
| No recognized properties | "No recognized properties in 'properties' object" |
