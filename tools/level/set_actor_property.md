# set_actor_property

Set one or more property values on an actor instance in the current editor level. Supports any property accessible via Unreal reflection, including nested struct properties with dot-notation and array indexing.

## Input

```json
{
  "actor_name": "MyLight",
  "properties": [
    {"property_path": "bCanBeDamaged", "value": "false"},
    {"property_path": "PivotOffset.X", "value": "42.5"}
  ]
}
```

| Parameter    | Required | Description                                                                        |
| ------------ | -------- | ---------------------------------------------------------------------------------- |
| `actor_name` | **yes**  | Internal name or editor label of the actor (case-insensitive).                     |
| `properties` | **yes**  | Array of property changes. Each entry has `property_path` (string) and `value` (string). |

### Property paths

| Format             | Example                 | Description                                      |
| ------------------ | ----------------------- | ------------------------------------------------ |
| Simple property    | `bCanBeDamaged`         | Direct property access                           |
| Nested struct      | `PivotOffset.X`        | Drills into struct, accesses a field              |
| Array element      | `Tags[0]`              | Accesses element at index                        |
| Deep nesting       | `MyArray[0].MyStruct.Y` | Multi-level drilling                             |

### Value format

Values are parsed by Unreal's `ImportText` system. Use the same format as Unreal's text export:

| Type     | Example values                            |
| -------- | ----------------------------------------- |
| Bool     | `"true"`, `"false"`, `"True"`, `"False"`  |
| Integer  | `"42"`, `"-1"`                            |
| Float    | `"3.14"`, `"0.0"`                         |
| String   | `"Hello World"`                           |
| Name     | `"MyName"`                                |
| Enum     | `"EnumValueName"` (short name)            |
| Vector   | `"(X=1.0,Y=2.0,Z=3.0)"` (full struct)    |
| Rotator  | `"(Pitch=0.0,Yaw=90.0,Roll=0.0)"`        |
| Color    | `"(R=255,G=0,B=0,A=255)"`                |

## Output

### Success

```json
{
  "name": "PointLight_0",
  "label": "MyLight",
  "class": "PointLight",
  "success_count": 2,
  "fail_count": 0,
  "results": [
    {
      "property_path": "bCanBeDamaged",
      "success": true,
      "type": "bool",
      "old_value": "True",
      "new_value": "False"
    },
    {
      "property_path": "PivotOffset.X",
      "success": true,
      "type": "float",
      "old_value": "0.000000",
      "new_value": "42.500000"
    }
  ]
}
```

| Field           | Description                                                          |
| --------------- | -------------------------------------------------------------------- |
| `name`          | Internal name assigned by the engine (`AActor::GetName()`)           |
| `label`         | Editor label (`AActor::GetActorLabel()`)                             |
| `class`         | Class short name                                                     |
| `success_count` | Number of properties successfully changed                            |
| `fail_count`    | Number of properties that failed                                     |
| `results`       | Per-property result array with path, success, type, old/new values   |

### Partial Success

When some properties succeed and others fail, `isError` is `false` (partial success). Check `fail_count` and individual `results[].success` to identify failures.

```json
{
  "success_count": 1,
  "fail_count": 1,
  "results": [
    {"property_path": "bCanBeDamaged", "success": true, "type": "bool", "old_value": "True", "new_value": "False"},
    {"property_path": "NonExistent", "success": false, "error": "Property 'NonExistent' not found on PointLight"}
  ]
}
```

### Error Cases

| Condition                  | Error message contains                          |
| -------------------------- | ----------------------------------------------- |
| Missing `actor_name`       | `"Missing required parameter: actor_name"`       |
| Missing `properties`       | `"Missing or empty required parameter: properties"` |
| Actor not found            | `"Actor '...' not found in the current level"`   |
| Property not found         | `"Property '...' not found on ..."`              |
| Value parse failure        | `"Failed to parse value '...' for type ..."`     |
| Not a struct (dot-notation)| `"Property '...' is not a struct or object"`     |
| Array index out of range   | `"Array index ... out of range"`                 |
| No editor world            | `"No editor world available"`                    |

## Notes

- Actor lookup searches by both internal name (`AActor::GetName()`) and editor label (`AActor::GetActorLabel()`), case-insensitive.
- Uses Unreal's reflection system (`ImportText_Direct` / `ExportTextItem_Direct`), so any property type supported by Unreal's text serialization works.
- Numeric types (float, int) are lenient — non-numeric strings like `"hello"` are parsed as `0` rather than producing an error.
- The `old_value` and `new_value` fields use Unreal's text export format, which can be fed back to `set_actor_property` for undo.
- The actor is marked as modified after successful changes (`Modify()` + `MarkPackageDirty()`).
- For transform changes, prefer `set_actor_transform` which provides a more ergonomic interface.
