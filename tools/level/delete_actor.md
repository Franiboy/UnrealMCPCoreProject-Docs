# delete_actor

Remove an actor from the current editor level. Finds the actor by internal name or editor label (case-insensitive). Returns the deleted actor's name, class, and last known transform for undo/logging purposes.

## Input

```json
{
  "actor_name": "PointLight_0"
}
```

| Parameter    | Required | Description                                                                                      |
| ------------ | -------- | ------------------------------------------------------------------------------------------------ |
| `actor_name` | **yes**  | Internal name (`GetName()`) or editor label (`GetActorLabel()`), case-insensitive. First match wins. |

## Output

### Success

```json
{
  "name": "PointLight_0",
  "label": "MyLight",
  "class": "PointLight",
  "transform": {
    "location_x": 100.0,
    "location_y": 200.0,
    "location_z": 300.0,
    "rotation_pitch": 0.0,
    "rotation_yaw": 45.0,
    "rotation_roll": 0.0,
    "scale_x": 1.0,
    "scale_y": 1.0,
    "scale_z": 1.0
  }
}
```

| Field       | Description                                                    |
| ----------- | -------------------------------------------------------------- |
| `name`      | Internal name of the deleted actor                             |
| `label`     | Editor label of the deleted actor                              |
| `class`     | Class short name of the deleted actor                          |
| `transform` | Last known world-space transform (location, rotation, scale)   |

### Error Cases

| Condition             | Error message contains                         |
| --------------------- | ---------------------------------------------- |
| Missing `actor_name`  | `"Missing required parameter: actor_name"`      |
| Actor not found       | `"not found in the current level"`              |
| Destroy failed        | `"Failed to delete actor"`                      |
| No editor world       | `"No editor world available"`                   |

## Notes

- Uses `UWorld::EditorDestroyActor()` with `bShouldModifyLevel=true`, which marks the level as dirty.
- Actor lookup matches against both `GetName()` and `GetActorLabel()`, case-insensitive. First match wins.
- The response captures the actor's name, label, class, and transform before destruction so the caller has a record of what was removed.
- Deleting an actor that is already destroyed or pending kill returns "not found".
- Some actors (e.g. `WorldSettings`) may be protected by the engine and cannot be destroyed; in that case a "Failed to delete" error is returned.
