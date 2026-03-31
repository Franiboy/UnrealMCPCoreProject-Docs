# spawn_actor

Spawn an actor in the current editor level from a C++ class or Blueprint asset. Optionally specify world-space location, rotation, scale, a display label, an editor folder, and tags.

## Input

```json
{
  "actor_class": "PointLight",
  "location_x": 100.0,
  "location_y": 200.0,
  "location_z": 300.0,
  "rotation_pitch": 0.0,
  "rotation_yaw": 45.0,
  "rotation_roll": 0.0,
  "scale_x": 1.0,
  "scale_y": 1.0,
  "scale_z": 1.0,
  "label": "MyLight",
  "folder": "/Gameplay/Lights",
  "tags": ["Interactive", "Gameplay"]
}
```

| Parameter        | Required | Description                                                                                              |
| ---------------- | -------- | -------------------------------------------------------------------------------------------------------- |
| `actor_class`    | **yes**  | C++ class name (e.g. `"PointLight"`, `"StaticMeshActor"`) or Blueprint asset path (e.g. `"/Game/BP_Enemy"`). |
| `location_x`     | no       | World-space X location (default: `0`).                                                                   |
| `location_y`     | no       | World-space Y location (default: `0`).                                                                   |
| `location_z`     | no       | World-space Z location (default: `0`).                                                                   |
| `rotation_pitch` | no       | Rotation pitch in degrees (default: `0`).                                                                |
| `rotation_yaw`   | no       | Rotation yaw in degrees (default: `0`).                                                                  |
| `rotation_roll`  | no       | Rotation roll in degrees (default: `0`).                                                                 |
| `scale_x`        | no       | Scale X (default: `1`).                                                                                  |
| `scale_y`        | no       | Scale Y (default: `1`).                                                                                  |
| `scale_z`        | no       | Scale Z (default: `1`).                                                                                  |
| `label`          | no       | Display label in the editor outliner. If omitted, the engine assigns a default label.                    |
| `folder`         | no       | Editor outliner folder path (e.g. `"/Gameplay/Enemies"`). Created automatically if it does not exist.    |
| `tags`           | no       | Array of string tags to assign to the spawned actor.                                                     |

### Class resolution

The `actor_class` parameter supports several formats:

1. **Short C++ class name** — e.g. `"PointLight"`, `"StaticMeshActor"`, `"CameraActor"`. The engine resolves these via `FindFirstObject<UClass>`, also trying the `A` prefix convention (e.g. `APointLight`).
2. **Blueprint asset path** — e.g. `"/Game/Blueprints/BP_Enemy"`. The Blueprint is loaded and its `GeneratedClass` is used as the spawn class.
3. **Full class path** — e.g. `"/Script/Engine.PointLight"`. Loaded directly via `LoadObject<UClass>`.

## Output

### Success

```json
{
  "name": "PointLight_0",
  "label": "MyLight",
  "class": "PointLight",
  "class_path": "/Script/Engine.PointLight",
  "is_blueprint": false,
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
  },
  "assigned_label": "MyLight",
  "folder": "/Gameplay/Lights",
  "tags": ["Interactive", "Gameplay"]
}
```

| Field            | Description                                                              |
| ---------------- | ------------------------------------------------------------------------ |
| `name`           | Internal name assigned by the engine (`AActor::GetName()`)               |
| `label`          | Editor label (`AActor::GetActorLabel()`)                                 |
| `class`          | Class short name                                                         |
| `class_path`     | Full class path (e.g. `/Script/Engine.PointLight`)                       |
| `is_blueprint`   | `true` if the actor was spawned from a Blueprint asset                   |
| `transform`      | Actual world-space transform after spawn                                 |
| `assigned_label` | Present only if a custom `label` was provided                            |
| `folder`         | Present only if a `folder` was provided                                  |
| `tags`           | Present only if `tags` were provided                                     |

### Error Cases

| Condition              | Error message contains                          |
| ---------------------- | ----------------------------------------------- |
| Missing `actor_class`  | `"Missing required parameter: actor_class"`      |
| Class not found        | `"Could not resolve actor class"`                |
| Abstract class         | `"Cannot spawn abstract class"`                  |
| Spawn failed           | `"Failed to spawn actor of class"`               |
| No editor world        | `"No editor world available"`                    |

## Notes

- The actor is spawned with `ESpawnActorCollisionHandlingMethod::AlwaysSpawn`, so it will never fail due to overlapping geometry.
- Spawned actors persist in the level until explicitly deleted or the level is reloaded without saving.
- The `folder` path is set on the actor after spawning; if the folder does not exist in the outliner, it is created automatically by the engine.
- When spawning from a Blueprint, the Blueprint is loaded via `LoadObject<UBlueprint>` and its `GeneratedClass` must inherit from `AActor`.
- The response returns the actual transform as read back from the spawned actor, which may differ slightly from the requested values due to engine rounding.
