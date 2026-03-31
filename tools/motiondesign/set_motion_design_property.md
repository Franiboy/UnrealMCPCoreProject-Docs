# set_motion_design_property

Set properties on a Motion Design actor (Cloner or Effector). Supports enabled, seed, color, layout, magnitude, shape, and mode.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `actor_name` | string | Yes | Name or label of the Motion Design actor |
| `enabled` | boolean | No | Enable or disable the cloner/effector |
| `seed` | integer | No | Random seed (cloners only) |
| `color` | object | No | Color `{r, g, b, a}` (0-1 range) |
| `layout` | string | No | Layout name for cloners (Grid, Line, Circle, etc.) |
| `magnitude` | number | No | Magnitude for effectors (0-1) |
| `shape` | string | No | Effector shape/type (Sphere, Plane, Box, Unbound, Radial, Torus) |
| `mode` | string | No | Effector mode (Default, Target, NoiseField, Push, Step) |

## Example

**Request:**
```json
{
  "tool": "set_motion_design_property",
  "arguments": {
    "actor_name": "MyCloner",
    "seed": 99,
    "enabled": false
  }
}
```

**Response:**
```json
{
  "type": "Cloner",
  "layout": "Grid",
  "seed": 99,
  "enabled": false,
  "actor_name": "MyCloner",
  "changed": ["seed", "enabled"]
}
```

## Notes

- Requires the ClonerEffector plugin (UE 5.5 experimental)
- Cloner-specific properties: seed, layout, color, enabled
- Effector-specific properties: magnitude, shape, mode, color, enabled
- Returns the updated state and a list of changed properties
- Returns error if actor is not a Cloner or Effector
