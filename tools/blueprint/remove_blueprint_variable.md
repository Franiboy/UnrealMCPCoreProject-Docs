# remove_blueprint_variable

Remove a member variable from a Blueprint by name. The Blueprint is recompiled after removal. Returns the removed variable's type and metadata.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "variable_name": "Health"
}
```

| Parameter       | Required | Description                                                        |
| --------------- | -------- | ------------------------------------------------------------------ |
| `asset_path`    | yes      | Asset path of the Blueprint (e.g. `/Game/Blueprints/BP_MyActor`)   |
| `variable_name` | yes      | Name of the variable to remove                                     |

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "variable_name": "Health",
  "variable_type": "float",
  "undo": {
    "action": "add_variable",
    "asset_path": "/Game/BP_MyActor",
    "variable_name": "Health",
    "variable_type": "float",
    "instance_editable": true,
    "blueprint_read_only": false,
    "expose_on_spawn": false,
    "private": false,
    "category": "Combat",
    "default_value": "100.0"
  }
}
```

## Notes

- Returns an error if the variable does not exist.
- The Blueprint is compiled after removal.
- Undo re-adds the variable with its original type, default value, category, tooltip, and visibility flags.
