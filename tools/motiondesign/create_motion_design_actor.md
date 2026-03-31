# create_motion_design_actor

Create a Motion Design actor (Cloner or Effector) in the current level. Optionally set initial layout, seed, color, location, rotation, and scale.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `type` | string | Yes | Actor type: "cloner" or "effector" |
| `name` | string | No | Label for the actor |
| `layout` | string | No | Initial layout for cloners (Grid, Line, Circle, Cylinder, Sphere, Honeycomb, etc.) |
| `seed` | integer | No | Random seed for cloners |
| `location` | object | No | Spawn location `{x, y, z}` |
| `rotation` | object | No | Spawn rotation `{pitch, yaw, roll}` |
| `scale` | object | No | Spawn scale `{x, y, z}` |

## Example

**Request:**
```json
{
  "tool": "create_motion_design_actor",
  "arguments": {
    "type": "cloner",
    "name": "MyCloner",
    "layout": "Line",
    "seed": 42,
    "location": { "x": 100, "y": 0, "z": 0 }
  }
}
```

**Response:**
```json
{
  "name": "MyCloner",
  "actor_name": "MyCloner",
  "type": "Cloner",
  "layout": "Line",
  "seed": 42
}
```

## Notes

- Requires the ClonerEffector plugin (UE 5.5 experimental)
- For cloners, default layout is "Grid" if not specified
- For effectors, default shape is "Sphere" and mode is "Offset"
- Returns error if type is not "cloner" or "effector"
