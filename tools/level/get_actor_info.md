# get_actor_info

Get detailed information about a specific actor in the current editor level. Returns the actor's identity, class, transform, tags, layers, visibility state, component hierarchy (with per-component properties), and non-default actor properties via reflection.

## Input

```json
{
  "actor_name": "PointLight_0",
  "include_properties": true,
  "include_components": true
}
```

| Parameter            | Required | Description                                                              |
| -------------------- | -------- | ------------------------------------------------------------------------ |
| `actor_name`         | **yes**  | Internal name (`GetName()`) or editor label (`GetActorLabel()`), case-insensitive. |
| `include_properties` | no       | Include non-default editable properties via reflection (default: `true`). |
| `include_components` | no       | Include component hierarchy with transforms and properties (default: `true`). |

## Output

### Success

```json
{
  "name": "PointLight_0",
  "label": "PointLight",
  "class": "PointLight",
  "class_path": "/Script/Engine.PointLight",
  "is_hidden": false,
  "is_temporarily_hidden_in_editor": false,
  "folder": "Lighting",
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
  "tags": ["Gameplay", "Interactive"],
  "layers": ["Lighting"],
  "component_count": 3,
  "components": [
    {
      "name": "PointLightComponent0",
      "class": "PointLightComponent",
      "is_scene_component": true,
      "is_root": true,
      "relative_transform": {
        "location_x": 0.0,
        "location_y": 0.0,
        "location_z": 0.0,
        "rotation_pitch": 0.0,
        "rotation_yaw": 0.0,
        "rotation_roll": 0.0,
        "scale_x": 1.0,
        "scale_y": 1.0,
        "scale_z": 1.0
      },
      "properties": {
        "Intensity": 5000.0,
        "LightColor": "(R=255,G=200,B=150,A=255)"
      }
    },
    {
      "name": "SpriteComponent",
      "class": "BillboardComponent",
      "is_scene_component": true,
      "attach_parent": "PointLightComponent0",
      "relative_transform": { "..." : "..." }
    }
  ],
  "properties": {
    "bCanBeDamaged": false
  }
}
```

| Field                                | Description                                                                     |
| ------------------------------------ | ------------------------------------------------------------------------------- |
| `name`                               | Internal name (`AActor::GetName()`)                                             |
| `label`                              | Editor label (`AActor::GetActorLabel()`)                                        |
| `class`                              | Class short name                                                                |
| `class_path`                         | Full class path (e.g. `/Script/Engine.PointLight`)                              |
| `is_hidden`                          | Whether the actor is hidden in-game                                             |
| `is_temporarily_hidden_in_editor`    | Whether the actor is temporarily hidden in the editor viewport                  |
| `folder`                             | Outliner folder path (present only when assigned)                               |
| `transform`                          | World-space location, rotation, and scale                                       |
| `tags`                               | Actor tags (present only when the actor has tags)                               |
| `layers`                             | Layer membership (present only when the actor belongs to layers)                |
| `component_count`                    | Number of components (when `include_components=true`)                           |
| `components`                         | Array of component objects (when `include_components=true`)                     |
| `components[].name`                  | Component name                                                                  |
| `components[].class`                 | Component class name                                                            |
| `components[].is_scene_component`    | Whether this component inherits from `USceneComponent`                          |
| `components[].is_root`               | Present and `true` only for the root component                                  |
| `components[].attach_parent`         | Name of the parent scene component (present when attached to a parent)          |
| `components[].relative_transform`    | Relative location/rotation/scale (scene components only)                        |
| `components[].properties`            | Non-default editable properties (when `include_properties=true` and any differ) |
| `properties`                         | Non-default editable actor properties (when `include_properties=true` and any differ) |

### Error Cases

| Condition                  | Error message contains            |
| -------------------------- | --------------------------------- |
| Missing `actor_name`       | `"Missing required parameter: actor_name"` |
| Actor not found            | `"not found in the current level"` |
| No editor world            | `"No editor world available"`     |

## Notes

- Actor lookup matches against both `GetName()` and `GetActorLabel()`, case-insensitive. First match wins.
- Properties are extracted via `TFieldIterator<FProperty>` reflection. Only editable (`CPF_Edit`), non-deprecated, non-delegate properties that differ from the Class Default Object are included.
- Property values use native JSON types where possible: `bool`, `int`/`float`/`double` as numbers, enums as strings (stripped of scope prefix). All other types use `ExportTextItem` string representation.
- Components are returned in the order provided by `AActor::GetComponents()`.
- Scene components include `relative_transform` and attachment hierarchy. Non-scene components (e.g. `UMovementComponent`) include `is_scene_component: false` only.
