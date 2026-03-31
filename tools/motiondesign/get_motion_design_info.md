# get_motion_design_info

Inspect Motion Design actors (Cloners and Effectors) in the current level. Returns layout, extensions, linked effectors, seed, color, enabled state, and other properties. If `actor_name` is omitted, lists all Cloner and Effector actors.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `actor_name` | string | No | Name or label of the actor to inspect. If omitted, lists all Cloner and Effector actors. |

## Example

**Request (list all):**
```json
{
  "tool": "get_motion_design_info",
  "arguments": {}
}
```

**Response:**
```json
{
  "cloners": [
    {
      "name": "MyCloner",
      "actor_name": "MyCloner",
      "type": "Cloner",
      "enabled": true,
      "seed": 0,
      "color": { "r": 1, "g": 1, "b": 1, "a": 1 },
      "layout": "Grid",
      "mesh_count": 0,
      "extensions": [],
      "location": { "x": 0, "y": 0, "z": 0 }
    }
  ],
  "effectors": [
    {
      "name": "MyEffector",
      "actor_name": "MyEffector",
      "type": "Effector",
      "enabled": true,
      "magnitude": 1,
      "color": { "r": 1, "g": 0, "b": 0, "a": 1 },
      "shape": "Sphere",
      "mode": "Offset",
      "location": { "x": 0, "y": 0, "z": 0 }
    }
  ],
  "cloner_count": 1,
  "effector_count": 1
}
```

**Request (single actor):**
```json
{
  "tool": "get_motion_design_info",
  "arguments": { "actor_name": "MyCloner" }
}
```

## Notes

- Requires the ClonerEffector plugin (UE 5.5 experimental)
- Looks up actors by label or internal name (case-insensitive)
- Cloner info includes layout, seed, color, extensions, and mesh count
- Effector info includes magnitude, shape (type), mode, and color
- Returns error if the named actor is not a Cloner or Effector
