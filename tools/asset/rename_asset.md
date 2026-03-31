# rename_asset

Rename or move any asset in the project. Works with all asset types (Blueprints, StaticMeshes, Materials, Textures, etc.). Unreal automatically creates redirectors so existing references keep working.

## Input

```json
{
  "asset_path": "/Game/Meshes/SM_Chair",
  "new_name": "SM_Table",
  "new_path": "/Game/Furniture"
}
```

| Parameter    | Required | Description                                                                 |
| ------------ | -------- | --------------------------------------------------------------------------- |
| `asset_path` | yes      | Current asset path (e.g. `/Game/Meshes/SM_Chair`). Short form accepted.     |
| `new_name`   | yes      | New asset name (without path, e.g. `SM_Table`).                             |
| `new_path`   | no       | New folder path (e.g. `/Game/Furniture`). Omit to stay in current folder.   |

### Path Resolution

The asset path is resolved flexibly:

1. **Short form** — `/Game/MyAsset` (auto-appends `.MyAsset`)
2. **Full object path** — `/Game/MyAsset.MyAsset`
3. **Package name fallback** — searches by folder + name if the above fail

## Output

```json
{
  "old_name": "SM_Chair",
  "old_asset_path": "/Game/Meshes/SM_Chair",
  "new_name": "SM_Table",
  "new_asset_path": "/Game/Furniture/SM_Table",
  "new_folder": "/Game/Furniture",
  "asset_class": "StaticMesh",
  "success": true
}
```

| Field            | Description                                    |
| ---------------- | ---------------------------------------------- |
| `old_name`       | Original asset name                            |
| `old_asset_path` | Original full path                             |
| `new_name`       | New asset name after rename                    |
| `new_asset_path` | New full path after rename                     |
| `new_folder`     | Folder the asset now resides in                |
| `asset_class`    | Asset class name (e.g. `Blueprint`, `StaticMesh`) |
| `success`        | `true` if rename succeeded                     |

## Errors

| Scenario                 | Error message contains   |
| ------------------------ | ------------------------ |
| Asset not found          | `"not found"`            |
| Failed to load           | `"Failed to load"`       |
| Target already exists    | `"already exists"`       |
| Rename failed            | `"Failed to rename"`     |
| Missing `new_name`       | `"new_name"`             |
| Missing `asset_path`     | `"asset_path"`           |

## Notes

- Uses `IAssetTools::RenameAssets()` which works for **all asset types**, not just Blueprints.
- Unreal automatically creates redirectors at the old path so existing references keep working.
- Supports rename-only (same folder) or move+rename (different folder via `new_path`).
- Undo data stores the original name and path for reversal.
