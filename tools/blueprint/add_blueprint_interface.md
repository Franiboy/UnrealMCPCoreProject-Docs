# add_blueprint_interface

Implement an interface on a Blueprint. Accepts a class name or full path. Automatically creates function graphs for all interface functions.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "interface_class": "Interface_AssetUserData"
}
```

| Parameter         | Required | Description                                                                     |
| ----------------- | -------- | ------------------------------------------------------------------------------- |
| `asset_path`      | yes      | Blueprint asset path                                                            |
| `interface_class` | yes      | Interface class name (e.g. `Interface_AssetUserData`) or full path (e.g. `/Script/Engine.Interface_AssetUserData`) |

### Interface Class Resolution

The class name is resolved in order:

1. **Full path** — `/Script/Engine.Interface_AssetUserData` or `/Game/MyBPI.MyBPI_C`
2. **Blueprint Interface asset path** — `/Game/BPI_Damageable` (loads the BP and gets its generated class)
3. **Short name** — `Interface_AssetUserData` (searched across all loaded classes)

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "interface_name": "Interface_AssetUserData",
  "interface_path": "/Script/Engine.Default__Interface_AssetUserData",
  "functions": [
    "GetAssetUserData",
    "AddAssetUserData",
    "RemoveAssetUserData"
  ]
}
```

| Field              | Description                                              |
| ------------------ | -------------------------------------------------------- |
| `asset_path`       | The Blueprint that was modified                          |
| `interface_name`   | Short name of the implemented interface                  |
| `interface_path`   | Full path of the interface class                         |
| `functions`        | Array of function graph names created for the interface  |

## Errors

| Scenario                    | Error message contains          |
| --------------------------- | ------------------------------- |
| Blueprint not found         | `"not found at path"`           |
| Interface class not found   | `"not found"`                   |
| Not an interface class      | `"not an interface"`            |
| Already implemented         | `"already implemented"`         |
| Implementation failed       | `"Failed to implement"`         |

## Notes

- The Blueprint is compiled automatically after adding the interface.
- Undo removes the interface via `FBlueprintEditorUtils::RemoveInterface`.
- Works with both C++ interfaces (e.g. `Interface_AssetUserData`) and Blueprint Interfaces (pass the asset path).
