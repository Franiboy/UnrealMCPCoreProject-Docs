# set_actor_transform

Move, rotate, and/or scale an actor in the current editor level. Only the provided transform fields are changed — unprovided fields retain their previous values.

## Input

```json
{
  "actor_name": "MyLight",
  "location_x": 200.0,
  "rotation_yaw": 45.0,
  "scale_x": 2.0,
  "scale_y": 2.0,
  "scale_z": 2.0
}
```

| Parameter        | Required | Description                                                                     |
| ---------------- | -------- | ------------------------------------------------------------------------------- |
| `actor_name`     | **yes**  | Internal name or editor label of the actor (case-insensitive).                  |
| `location_x`     | no       | World-space X location.                                                         |
| `location_y`     | no       | World-space Y location.                                                         |
| `location_z`     | no       | World-space Z location.                                                         |
| `rotation_pitch` | no       | Rotation pitch in degrees.                                                      |
| `rotation_yaw`   | no       | Rotation yaw in degrees.                                                        |
| `rotation_roll`  | no       | Rotation roll in degrees.                                                       |
| `scale_x`        | no       | Scale X.                                                                        |
| `scale_y`        | no       | Scale Y.                                                                        |
| `scale_z`        | no       | Scale Z.                                                                        |

At least one transform field must be provided.

### Partial updates

Fields are merged individually. For example, providing only `location_x` changes the X position while leaving Y, Z, rotation, and scale unchanged. Within a group (location, rotation, scale), only provided axes are updated — the rest keep their current values.

## Output

### Success

```json
{
  "name": "PointLight_0",
  "label": "MyLight",
  "class": "PointLight",
  "previous_transform": {
    "location_x": 100.0,
    "location_y": 0.0,
    "location_z": 0.0,
    "rotation_pitch": 0.0,
    "rotation_yaw": 0.0,
    "rotation_roll": 0.0,
    "scale_x": 1.0,
    "scale_y": 1.0,
    "scale_z": 1.0
  },
  "transform": {
    "location_x": 200.0,
    "location_y": 0.0,
    "location_z": 0.0,
    "rotation_pitch": 0.0,
    "rotation_yaw": 45.0,
    "rotation_roll": 0.0,
    "scale_x": 2.0,
    "scale_y": 2.0,
    "scale_z": 2.0
  }
}
```

| Field                | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| `name`               | Internal name assigned by the engine (`AActor::GetName()`)   |
| `label`              | Editor label (`AActor::GetActorLabel()`)                     |
| `class`              | Class short name                                             |
| `previous_transform` | Full 9-field transform before the change                     |
| `transform`          | Full 9-field transform after the change (read back from actor) |

### Error Cases

| Condition                    | Error message contains                                                                           |
| ---------------------------- | ------------------------------------------------------------------------------------------------ |
| Missing `actor_name`         | `"Missing required parameter: actor_name"`                                                       |
| Actor not found              | `"Actor '...' not found in the current level"`                                                   |
| No transform fields provided | `"No transform fields provided. Supply at least one of: location_x/y/z, rotation_pitch/yaw/roll, scale_x/y/z"` |
| No editor world              | `"No editor world available"`                                                                    |

## Notes

- Actor lookup searches by both internal name (`AActor::GetName()`) and editor label (`AActor::GetActorLabel()`), case-insensitive.
- The response returns the actual transform as read back from the actor after the change, which may differ slightly from the requested values due to engine rounding.
- The `previous_transform` can be used to undo the change by calling `set_actor_transform` again with the old values.
