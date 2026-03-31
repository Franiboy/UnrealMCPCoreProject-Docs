# set_blueprint_defaults

Set class default (CDO) property values with support for nested property paths (e.g. `MyVector.X`).

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "compile": true,
  "properties": [
    {"property_path": "MaxHealth", "value": "200.0"},
    {"property_path": "SpawnOffset.Z", "value": "100.0"},
    {"property_path": "bCanBeDamaged", "value": "false"}
  ]
}
```

| Parameter    | Required | Description                                        |
| ------------ | -------- | -------------------------------------------------- |
| `asset_path` | yes      | Blueprint asset path                               |
| `properties` | yes      | Array of `{property_path, value}` pairs            |
| `compile`    | no       | Recompile after changes (default: `true`)          |

### Property Path Syntax

- Simple: `MaxHealth`, `bCanBeDamaged`
- Nested (dot notation): `MyVector.X`, `Transform.Location.Z`
- Array indexing: `MyArray[0]`

Values use Unreal's text format -- the same format returned by `get_blueprint`:
- Numbers: `42.0`
- Booleans: `true`, `false`
- Vectors: `(X=1.0,Y=2.0,Z=3.0)`
- Enums: `MyEnum::Value`

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "success_count": 2,
  "fail_count": 1,
  "results": [
    {
      "property_path": "MaxHealth",
      "success": true,
      "type": "float",
      "old_value": "100.000000",
      "new_value": "200.000000"
    },
    {
      "property_path": "SpawnOffset.Z",
      "success": true,
      "type": "double",
      "old_value": "0.000000",
      "new_value": "100.000000"
    },
    {
      "property_path": "UnknownProp",
      "success": false,
      "error": "Property 'UnknownProp' not found on TestBP_C"
    }
  ],
  "compile_status": "UpToDate"
}
```

## Notes

- Partial success is possible: some properties may succeed while others fail. Check `success_count` and `fail_count`.
- Old values are captured before changes for undo support.
- Undo restores all changed properties to their previous values.
