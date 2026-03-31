# duplicate_asset

Duplicate any asset in the project with a new name. Works with all asset types (Blueprints, StaticMeshes, Materials, Textures, etc.). Optionally place the copy in a different folder.

## Input

```json
{
  "asset_path": "/Game/Meshes/SM_Chair",
  "new_name": "SM_Chair_Copy",
  "new_path": "/Game/Copies"
}
```

| Parameter    | Required | Description                                                                      |
| ------------ | -------- | -------------------------------------------------------------------------------- |
| `asset_path` | yes      | Source asset path (e.g. `/Game/Meshes/SM_Chair`). Short form accepted.           |
| `new_name`   | yes      | Name for the duplicated asset (without path, e.g. `SM_Chair_Copy`).              |
| `new_path`   | no       | Destination folder (e.g. `/Game/Copies`). Omit to place in the same folder.      |

### Path Resolution

The asset path is resolved flexibly:

1. **Short form** — `/Game/MyAsset` (auto-appends `.MyAsset`)
2. **Full object path** — `/Game/MyAsset.MyAsset`
3. **Package name fallback** — searches by folder + name if the above fail

## Output

```json
{
  "source_name": "SM_Chair",
  "source_path": "/Game/Meshes/SM_Chair",
  "new_name": "SM_Chair_Copy",
  "new_path": "/Game/Copies/SM_Chair_Copy",
  "new_folder": "/Game/Copies",
  "asset_class": "StaticMesh",
  "success": true
}
```

| Field         | Description                                           |
| ------------- | ----------------------------------------------------- |
| `source_name` | Original asset name                                   |
| `source_path` | Original full path                                    |
| `new_name`    | Name of the duplicated asset                          |
| `new_path`    | Full path of the duplicated asset                     |
| `new_folder`  | Folder the duplicate resides in                       |
| `asset_class` | Asset class name (e.g. `Blueprint`, `StaticMesh`)     |
| `success`     | `true` if duplication succeeded                       |

## Errors

| Scenario                 | Error message contains   |
| ------------------------ | ------------------------ |
| Asset not found          | `"not found"`            |
| Failed to load           | `"Failed to load"`       |
| Target already exists    | `"already exists"`       |
| Duplication failed       | `"Failed to duplicate"`  |
| Missing `new_name`       | `"new_name"`             |
| Missing `asset_path`     | `"asset_path"`           |

## Notes

- Uses `IAssetTools::DuplicateAsset()` which works for **all asset types**, not just Blueprints.
- The source asset is left untouched — only a copy is created.
- Undo deletes the duplicated asset (reuses the `delete_asset` undo action).
- For Blueprint-specific duplication with compilation and parent class info, see `duplicate_blueprint`.
