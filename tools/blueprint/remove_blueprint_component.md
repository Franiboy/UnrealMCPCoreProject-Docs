# remove_blueprint_component

Remove a component from a Blueprint's component tree (SimpleConstructionScript) by name. Returns the removed component's class, parent, and transform for undo. The Blueprint is recompiled after removal.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "component_name": "MeshComp"
}
```

| Parameter        | Required | Description                                                          |
| ---------------- | -------- | -------------------------------------------------------------------- |
| `asset_path`     | yes      | Asset path of the Blueprint (e.g. `/Game/Blueprints/BP_MyActor`)     |
| `component_name` | yes      | Variable name of the component to remove                             |

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "component_name": "MeshComp",
  "component_class": "StaticMeshComponent",
  "is_scene_component": true,
  "undo": {
    "action": "add_component",
    "asset_path": "/Game/BP_MyActor",
    "component_name": "MeshComp",
    "component_class": "StaticMeshComponent",
    "parent_component": "DefaultSceneRoot",
    "location": {"x": 0, "y": 0, "z": 100},
    "rotation": {"pitch": 0, "yaw": 0, "roll": 0},
    "scale": {"x": 1, "y": 1, "z": 1}
  }
}
```

## Notes

- Returns an error if the component does not exist.
- The undo payload captures the full component metadata (class, parent, transform) so the component can be re-created.
- The Blueprint is compiled after removal.
- Only works with Actor-based Blueprints (must have a SimpleConstructionScript).
