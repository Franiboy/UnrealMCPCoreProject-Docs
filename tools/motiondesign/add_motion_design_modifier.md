# add_motion_design_modifier

Set the effector shape (type) and/or mode on an Effector actor. Also supports setting magnitude.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `actor_name` | string | Yes | Name or label of the Effector actor |
| `shape` | string | No | Effector shape/type: Sphere, Plane, Box, Unbound, Radial, Torus |
| `mode` | string | No | Effector mode: Default, Target, NoiseField, Push, Step |
| `magnitude` | number | No | Effector magnitude (0-1) |

At least one of `shape`, `mode`, or `magnitude` must be provided.

## Example

**Request:**
```json
{
  "tool": "add_motion_design_modifier",
  "arguments": {
    "actor_name": "MyEffector",
    "shape": "Box",
    "mode": "Target",
    "magnitude": 0.75
  }
}
```

**Response:**
```json
{
  "actor_name": "MyEffector",
  "type": "Effector",
  "shape": "Box",
  "mode": "Target",
  "magnitude": 0.75,
  "changed": ["shape", "mode", "magnitude"]
}
```

## Notes

- Requires the ClonerEffector plugin (UE 5.5 experimental)
- Available shapes: Sphere, Plane, Box, Unbound, Radial, Torus
- Available modes: Default, Target, NoiseField, Push, Step, Offset
- Returns the updated state and a list of changed properties
- Returns error if the actor is not an Effector
