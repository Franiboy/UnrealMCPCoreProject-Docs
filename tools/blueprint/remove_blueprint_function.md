# remove_blueprint_function

Remove a function graph from a Blueprint by name. Returns the removed function's metadata (flags, parameters, return type) for undo. The Blueprint is recompiled after removal.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "function_name": "CalculateDamage"
}
```

| Parameter       | Required | Description                                                          |
| --------------- | -------- | -------------------------------------------------------------------- |
| `asset_path`    | yes      | Asset path of the Blueprint (e.g. `/Game/Blueprints/BP_MyActor`)     |
| `function_name` | yes      | Name of the function to remove                                       |

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "function_name": "CalculateDamage",
  "pure": true,
  "const": false,
  "access": "Public",
  "return_type": "float",
  "category": "Combat",
  "undo": {
    "action": "add_function",
    "asset_path": "/Game/BP_MyActor",
    "function_name": "CalculateDamage",
    "pure": true,
    "const": false,
    "access": "Public",
    "return_type": "float",
    "category": "Combat",
    "tooltip": "Computes final damage",
    "inputs": [
      {"name": "BaseDamage", "type": "float"}
    ]
  }
}
```

## Notes

- Returns an error if the function does not exist.
- The undo payload captures the full function signature (flags, inputs, outputs, return type, category, tooltip) so the function can be re-created.
- The Blueprint is compiled after removal.
- Only user-created function graphs can be removed (not the EventGraph).
