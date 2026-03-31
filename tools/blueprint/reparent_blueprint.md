# reparent_blueprint

Change the parent class of a Blueprint. The new parent can be a C++ class name, a full class path, or another Blueprint's generated class. The Blueprint is recompiled after reparenting.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "new_parent_class": "Character"
}
```

| Parameter          | Required | Description                                                                                     |
| ------------------ | -------- | ----------------------------------------------------------------------------------------------- |
| `asset_path`       | yes      | Asset path of the Blueprint to reparent                                                         |
| `new_parent_class` | yes      | New parent class -- C++ name (e.g. `Character`), full path (e.g. `/Script/Engine.Character`), or BP generated class (e.g. `/Game/BP_Base.BP_Base_C`) |

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "name": "BP_MyActor",
  "old_parent_class": "Actor",
  "old_parent_class_path": "/Script/Engine.Actor",
  "new_parent_class": "Character",
  "new_parent_class_path": "/Script/Engine.Character",
  "compile_status": "UpToDate",
  "generated_class": "/Game/BP_MyActor.BP_MyActor_C"
}
```

## Notes

- If the Blueprint already has the requested parent class, returns an error.
- All nodes are refreshed and the Blueprint is recompiled after reparenting.
- Undo reparents back to the original parent class.
