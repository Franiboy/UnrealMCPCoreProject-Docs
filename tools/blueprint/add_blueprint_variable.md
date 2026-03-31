# add_blueprint_variable

Add a new member variable to a Blueprint. Supports primitive types, struct types, object types, and container wrappers (`Array<T>`, `Set<T>`, `Map<K,V>`). The Blueprint is recompiled after adding the variable.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "variable_name": "Health",
  "variable_type": "float",
  "category": "Combat",
  "default_value": "100.0",
  "instance_editable": true,
  "tooltip": "Current health points"
}
```

| Parameter          | Required | Description                                                                                                          |
| ------------------ | -------- | -------------------------------------------------------------------------------------------------------------------- |
| `asset_path`       | yes      | Asset path of the Blueprint (e.g. `/Game/Blueprints/BP_MyActor`)                                                     |
| `variable_name`    | yes      | Name for the new variable (e.g. `Health`, `MaxSpeed`)                                                                |
| `variable_type`    | yes      | Type string -- see **Supported Types** below                                                                         |
| `instance_editable`| no       | If `true` (default), the variable is editable per-instance in the Details panel                                      |
| `blueprint_read_only`| no     | If `true`, the variable is read-only in Blueprints. Default `false`                                                  |
| `expose_on_spawn`  | no       | If `true`, expose this variable as a pin on the Spawn Actor node. Default `false`                                    |
| `private`          | no       | If `true`, the variable is private (not accessible from other Blueprints). Default `false`                           |
| `category`         | no       | Category name for grouping in the Details panel (e.g. `Combat`, `Movement`)                                          |
| `default_value`    | no       | Default value as a string (e.g. `100.0`, `true`, `(X=1,Y=2,Z=3)`)                                                   |
| `tooltip`          | no       | Tooltip description shown when hovering over the variable                                                            |

## Supported Types

| Category   | Examples                                                              |
| ---------- | --------------------------------------------------------------------- |
| Primitives | `bool`, `byte`, `int`, `int64`, `float`, `double`                     |
| Strings    | `FName`, `FString`, `FText`                                           |
| Structs    | `FVector`, `FRotator`, `FTransform`, `FLinearColor`, `FVector2D`, `FVector4` |
| Objects    | `Actor`, `Pawn`, `StaticMeshComponent`, or any `UClass` name          |
| Containers | `Array<float>`, `Set<FName>`, `Map<FString, int>`                     |

Type names are case-insensitive for primitives. Struct and object names are resolved from `/Script/CoreUObject` and `/Script/Engine`.

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "variable_name": "Health",
  "variable_type": "float",
  "resolved_type": "float",
  "instance_editable": true,
  "blueprint_read_only": false,
  "expose_on_spawn": false,
  "private": false,
  "category": "Combat",
  "default_value": "100.0",
  "tooltip": "Current health points",
  "undo": {
    "action": "remove_variable",
    "asset_path": "/Game/BP_MyActor",
    "variable_name": "Health"
  }
}
```

## Notes

- Returns an error if a variable with the same name already exists.
- The Blueprint is compiled after adding the variable.
- Undo removes the variable from the Blueprint.
