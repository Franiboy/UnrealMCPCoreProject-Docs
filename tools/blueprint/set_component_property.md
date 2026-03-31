# set_component_property

Set properties on a Blueprint component's template with support for nested property paths (e.g. `RelativeLocation.X`, `bVisible`).

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "component_name": "MyMesh",
  "compile": true,
  "properties": [
    {"property_path": "bVisible", "value": "false"},
    {"property_path": "RelativeLocation.X", "value": "100.0"},
    {"property_path": "CastShadow", "value": "false"}
  ]
}
```

| Parameter        | Required | Description                                        |
| ---------------- | -------- | -------------------------------------------------- |
| `asset_path`     | yes      | Blueprint asset path                               |
| `component_name` | yes      | Variable name of the component to modify           |
| `properties`     | yes      | Array of `{property_path, value}` pairs            |
| `compile`        | no       | Recompile after changes (default: `true`)          |

### Property Path Syntax

- Simple: `bVisible`, `CastShadow`, `Intensity`
- Nested (dot notation): `RelativeLocation.X`, `RelativeScale3D.Z`
- Array indexing: `MyArray[0]`

Values use Unreal's text format:
- Numbers: `42.0`, `5000.0`
- Booleans: `true`, `false`
- Vectors: `(X=1.0,Y=2.0,Z=3.0)`
- Enums: `ECollisionEnabled::NoCollision`

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "component_name": "MyMesh",
  "component_class": "StaticMeshComponent",
  "success_count": 2,
  "fail_count": 1,
  "results": [
    {
      "property_path": "bVisible",
      "success": true,
      "type": "bool",
      "old_value": "True",
      "new_value": "False"
    },
    {
      "property_path": "RelativeLocation.X",
      "success": true,
      "type": "double",
      "old_value": "0.000000",
      "new_value": "100.000000"
    },
    {
      "property_path": "UnknownProp",
      "success": false,
      "error": "Property 'UnknownProp' not found on StaticMeshComponent"
    }
  ]
}
```

## Notes

- Operates on the component **template** (the SCS node's default object), not on spawned instances.
- Partial success is possible: some properties may succeed while others fail. Check `success_count` and `fail_count`.
- Old values are captured before changes for undo support.
- Undo restores all changed properties to their previous values.
- Works with any property type supported by `ImportText` / `ExportText` (bool, int, float, vector, rotator, enum, etc.).
