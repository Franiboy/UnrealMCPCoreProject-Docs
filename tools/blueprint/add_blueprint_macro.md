# add_blueprint_macro

Create a new macro graph in a Blueprint with optional input/output tunnel pins. Macros support multiple exec output pins (unlike functions, which have a single execution flow). The Blueprint is recompiled after creation.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "macro_name": "BranchOnHealth",
  "inputs": [
    {"name": "Health", "type": "float"},
    {"name": "Threshold", "type": "float"}
  ],
  "outputs": [
    {"name": "Above", "type": "exec"},
    {"name": "Below", "type": "exec"},
    {"name": "CurrentHealth", "type": "float"}
  ]
}
```

| Parameter    | Required | Description                                                                                          |
| ------------ | -------- | ---------------------------------------------------------------------------------------------------- |
| `asset_path` | yes      | Asset path of the Blueprint (e.g. `/Game/Blueprints/BP_MyActor`)                                     |
| `macro_name` | yes      | Name for the new macro (e.g. `BranchOnHealth`)                                                       |
| `inputs`     | no       | Array of input tunnel pins — each `{"name": "...", "type": "..."}`. Types use the same syntax as `add_blueprint_variable`. |
| `outputs`    | no       | Array of output tunnel pins — each `{"name": "...", "type": "..."}`. Supports `exec` type for flow-control pins. |

## Supported Types (for inputs, outputs)

| Category   | Examples                                                              |
| ---------- | --------------------------------------------------------------------- |
| Exec       | `exec` — execution flow pin (multiple exec outputs allowed)           |
| Primitives | `bool`, `byte`, `int`, `int64`, `float`, `double`                     |
| Strings    | `FName`, `FString`, `FText`                                           |
| Structs    | `FVector`, `FRotator`, `FTransform`, `FLinearColor`, `FVector2D`, `FVector4` |
| Objects    | `Actor`, `Pawn`, `StaticMeshComponent`, or any `UClass` name          |
| Containers | `Array<float>`, `Set<FName>`, `Map<FString, int>`                     |

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "macro_name": "BranchOnHealth",
  "graph_name": "BranchOnHealth",
  "input_count": 2,
  "output_count": 3,
  "undo": {
    "action": "remove_macro",
    "asset_path": "/Game/BP_MyActor",
    "macro_name": "BranchOnHealth"
  }
}
```

## Notes

- Returns an error if a macro with the same name already exists.
- The Blueprint is compiled after creating the macro.
- Undo removes the macro graph from the Blueprint.
- **Macros vs Functions:** Macros are expanded inline at compile time and support multiple exec output pins (e.g. branch-style flow control). Functions are called as subroutines and have a single execution flow.
- Use `exec` type on output pins to create flow-control macros with multiple execution paths (similar to the built-in Branch or ForEachLoop macros).
- Both `inputs` and `outputs` use the same type syntax as `add_blueprint_variable` (see **Supported Types**), plus the `exec` type for execution flow pins.
