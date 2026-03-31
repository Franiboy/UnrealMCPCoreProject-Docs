# add_blueprint_function

Create a new function graph in a Blueprint. Supports input/output parameters with typed pins, a return type, pure/const flags, access specifier, category, and tooltip. The Blueprint is recompiled after creation.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "function_name": "CalculateDamage",
  "inputs": [
    {"name": "BaseDamage", "type": "float"},
    {"name": "Multiplier", "type": "float"}
  ],
  "return_type": "float",
  "pure": true,
  "category": "Combat",
  "tooltip": "Computes final damage from base and multiplier"
}
```

| Parameter       | Required | Description                                                                                          |
| --------------- | -------- | ---------------------------------------------------------------------------------------------------- |
| `asset_path`    | yes      | Asset path of the Blueprint (e.g. `/Game/Blueprints/BP_MyActor`)                                     |
| `function_name` | yes      | Name for the new function (e.g. `CalculateDamage`)                                                   |
| `inputs`        | no       | Array of input parameters — each `{"name": "...", "type": "..."}`. Types use the same syntax as `add_blueprint_variable`. |
| `outputs`       | no       | Array of output parameters (by-ref out pins) — each `{"name": "...", "type": "..."}`.                |
| `return_type`   | no       | Return type (e.g. `float`, `bool`, `FVector`). Omit for void functions.                              |
| `pure`          | no       | If `true`, creates a pure function (no execution pin). Default `false`.                              |
| `const`         | no       | If `true`, marks the function as const (does not modify state). Default `false`.                     |
| `access`        | no       | Access specifier: `Public` (default), `Protected`, or `Private`.                                     |
| `category`      | no       | Category for grouping in the function list.                                                          |
| `tooltip`       | no       | Tooltip description shown when hovering over the function.                                           |

## Supported Types (for inputs, outputs, return_type)

| Category   | Examples                                                              |
| ---------- | --------------------------------------------------------------------- |
| Primitives | `bool`, `byte`, `int`, `int64`, `float`, `double`                     |
| Strings    | `FName`, `FString`, `FText`                                           |
| Structs    | `FVector`, `FRotator`, `FTransform`, `FLinearColor`, `FVector2D`, `FVector4` |
| Objects    | `Actor`, `Pawn`, `StaticMeshComponent`, or any `UClass` name          |
| Containers | `Array<float>`, `Set<FName>`, `Map<FString, int>`                     |

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
  "input_count": 2,
  "output_count": 1,
  "undo": {
    "action": "remove_function",
    "asset_path": "/Game/BP_MyActor",
    "function_name": "CalculateDamage"
  }
}
```

## Notes

- Returns an error if a function with the same name already exists.
- The Blueprint is compiled after creating the function.
- Undo removes the function graph from the Blueprint.
- A result node is only created when `return_type` or `outputs` are provided.
- Both `inputs` and `outputs` use the same type syntax as `add_blueprint_variable` (see **Supported Types**).
