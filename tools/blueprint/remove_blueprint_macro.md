# remove_blueprint_macro

Remove a macro graph from a Blueprint by name. Returns the removed macro's tunnel pin metadata (input/output pins) for undo. The Blueprint is recompiled after removal.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "macro_name": "ForEachEnemy"
}
```

| Parameter    | Required | Description                                                          |
| ------------ | -------- | -------------------------------------------------------------------- |
| `asset_path` | yes      | Asset path of the Blueprint (e.g. `/Game/Blueprints/BP_MyActor`)     |
| `macro_name` | yes      | Name of the macro to remove                                          |

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "macro_name": "ForEachEnemy",
  "node_count": 5,
  "input_pins": [
    {"name": "Execute", "type": "exec"},
    {"name": "TargetArray", "type": "Array<Actor>"}
  ],
  "output_pins": [
    {"name": "Completed", "type": "exec"},
    {"name": "Element", "type": "Actor"}
  ],
  "undo": {
    "action": "add_macro",
    "asset_path": "/Game/BP_MyActor",
    "macro_name": "ForEachEnemy",
    "input_pins": [
      {"name": "Execute", "type": "exec"},
      {"name": "TargetArray", "type": "Array<Actor>"}
    ],
    "output_pins": [
      {"name": "Completed", "type": "exec"},
      {"name": "Element", "type": "Actor"}
    ]
  }
}
```

## Notes

- Returns an error if the macro does not exist.
- The undo payload captures the macro's tunnel pin metadata (input and output pins) so the macro can be re-created.
- The Blueprint is compiled after removal.
- `node_count` indicates how many nodes were in the macro graph before removal.
- Only user-created macro graphs can be removed.
