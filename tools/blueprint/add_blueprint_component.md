# add_blueprint_component

Add a component to a Blueprint's component tree (SimpleConstructionScript). Supports scene components with transform (location, rotation, scale) and non-scene components. Optionally attach to a parent component by name. The Blueprint is recompiled after adding.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "component_class": "StaticMeshComponent",
  "component_name": "MeshComp",
  "parent_component": "DefaultSceneRoot",
  "location": {"x": 0, "y": 0, "z": 100},
  "rotation": {"pitch": 0, "yaw": 90, "roll": 0},
  "scale": {"x": 1, "y": 1, "z": 1}
}
```

| Parameter          | Required | Description                                                                                          |
| ------------------ | -------- | ---------------------------------------------------------------------------------------------------- |
| `asset_path`       | yes      | Asset path of the Blueprint (e.g. `/Game/Blueprints/BP_MyActor`)                                     |
| `component_class`  | yes      | Component class name (e.g. `StaticMeshComponent`, `PointLightComponent`, `BoxComponent`)             |
| `component_name`   | no       | Variable name for the component. Auto-generated from class name if omitted.                          |
| `parent_component` | no       | Name of existing component to attach to. If omitted, scene components attach to default scene root.  |
| `location`         | no       | Relative location `{"x": 0, "y": 0, "z": 100}`. Scene components only.                             |
| `rotation`         | no       | Relative rotation `{"pitch": 0, "yaw": 90, "roll": 0}`. Scene components only.                     |
| `scale`            | no       | Relative scale `{"x": 1, "y": 1, "z": 1}`. Scene components only.                                  |

## Component Class Resolution

The class name is resolved in this order:

1. Direct lookup by name (e.g. `StaticMeshComponent`)
2. With `U` prefix (e.g. `UStaticMeshComponent`)
3. From `/Script/Engine` package

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "component_name": "MeshComp",
  "component_class": "StaticMeshComponent",
  "is_scene_component": true,
  "parent_component": "DefaultSceneRoot",
  "relative_transform": {
    "location": {"x": 0, "y": 0, "z": 100},
    "rotation": {"pitch": 0, "yaw": 90, "roll": 0},
    "scale": {"x": 1, "y": 1, "z": 1}
  },
  "undo": {
    "action": "remove_component",
    "asset_path": "/Game/BP_MyActor",
    "component_name": "MeshComp"
  }
}
```

## Notes

- Only works with Actor-based Blueprints (must have a SimpleConstructionScript).
- Returns an error if a component with the same name already exists.
- Returns an error if the class is not a `UActorComponent` subclass.
- The Blueprint is compiled after adding the component.
- Undo removes the component from the Blueprint.
- Transform fields (`location`, `rotation`, `scale`) are ignored for non-scene components.
