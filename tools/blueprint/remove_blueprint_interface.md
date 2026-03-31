# remove_blueprint_interface

Remove an interface implementation from a Blueprint. Accepts a class name or full path. Removes the associated function graphs.

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
| `interface_class` | yes      | Interface class name (e.g. `Interface_AssetUserData`) or full path              |

### Interface Class Resolution

Same as `add_blueprint_interface`:

1. **Full path** — `/Script/Engine.Interface_AssetUserData` or `/Game/MyBPI.MyBPI_C`
2. **Blueprint Interface asset path** — `/Game/BPI_Damageable` (loads the BP and gets its generated class)
3. **Short name** — `Interface_AssetUserData` (searched across all loaded classes)

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "interface_name": "Interface_AssetUserData",
  "interface_path": "/Script/Engine.Default__Interface_AssetUserData",
  "removed_functions": [
    "GetAssetUserData",
    "AddAssetUserData",
    "RemoveAssetUserData"
  ]
}
```

| Field               | Description                                               |
| ------------------- | --------------------------------------------------------- |
| `asset_path`        | The Blueprint that was modified                           |
| `interface_name`    | Short name of the removed interface                       |
| `interface_path`    | Full path of the interface class                          |
| `removed_functions` | Array of function graph names that were removed           |

## Errors

| Scenario                    | Error message contains          |
| --------------------------- | ------------------------------- |
| Blueprint not found         | `"not found at path"`           |
| Interface class not found   | `"not found"`                   |
| Interface not implemented   | `"not implemented"`             |
| Missing `interface_class`   | `"interface_class"`             |

## Notes

- The Blueprint is compiled automatically after removing the interface.
- Undo re-adds the interface via `FBlueprintEditorUtils::ImplementNewInterface`.
- Works with both C++ interfaces and Blueprint Interfaces.
