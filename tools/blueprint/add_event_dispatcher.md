# add_event_dispatcher

Add a new event dispatcher (multicast delegate) to a Blueprint. Optionally specify parameters for the delegate signature.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "dispatcher_name": "OnHealthChanged",
  "parameters": [
    {"name": "NewHealth", "type": "float"},
    {"name": "DamageCauser", "type": "Name"}
  ]
}
```

| Parameter         | Required | Description                                                                     |
| ----------------- | -------- | ------------------------------------------------------------------------------- |
| `asset_path`      | yes      | Blueprint asset path                                                            |
| `dispatcher_name` | yes      | Name for the event dispatcher (e.g. `OnHealthChanged`)                          |
| `parameters`      | no       | Array of `{"name": "...", "type": "..."}` objects. Types use the same format as `add_blueprint_function` (see type reference below). |

### Supported Parameter Types

Same as `add_blueprint_function` / `add_blueprint_variable`:

- **Primitives:** `bool`, `byte`, `int` / `int32`, `int64`, `float`, `double`, `Name` / `FName`, `String` / `FString`, `Text` / `FText`
- **Structs:** `FVector`, `FRotator`, `FTransform`, `FLinearColor`, `FVector2D`, `FVector4`, or any `UScriptStruct` name
- **Objects:** `Actor`, `Pawn`, or any `UClass` name
- **Containers:** `Array<T>`, `Set<T>`, `Map<K, V>`

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "dispatcher_name": "OnHealthChanged",
  "parameters": [
    {"name": "NewHealth", "type": "real (float)"},
    {"name": "DamageCauser", "type": "name"}
  ]
}
```

| Field              | Description                                              |
| ------------------ | -------------------------------------------------------- |
| `asset_path`       | The Blueprint that was modified                          |
| `dispatcher_name`  | Name of the created event dispatcher                     |
| `parameters`       | Array of parameters with resolved type names             |

## Errors

| Scenario                    | Error message contains          |
| --------------------------- | ------------------------------- |
| Blueprint not found         | `"not found at path"`           |
| Duplicate name              | `"already exists"`              |
| Variable creation failed    | `"Failed to add"`               |
| Invalid parameter type      | `"Unrecognised type"`           |
| Missing `dispatcher_name`   | `"dispatcher_name"`             |

## Notes

- The Blueprint is compiled automatically after creation.
- Creates both the MC delegate member variable and the delegate signature graph (matching the Blueprint Editor workflow).
- If parameter type validation fails, the dispatcher is rolled back (not created).
- Undo removes the dispatcher via `FBlueprintEditorUtils::RemoveMemberVariable`.
