# move_asset

Move any asset to a different content folder while keeping its name. Works with all asset types (Blueprints, StaticMeshes, Materials, Textures, etc.). Unreal automatically creates redirectors so existing references keep working.

## Input

```json
{
  "asset_path": "/Game/Meshes/SM_Chair",
  "destination_path": "/Game/Environment/Furniture"
}
```

| Parameter          | Required | Description                                                                  |
| ------------------ | -------- | ---------------------------------------------------------------------------- |
| `asset_path`       | yes      | Current asset path (e.g. `/Game/Meshes/SM_Chair`). Short form accepted.      |
| `destination_path` | yes      | Destination folder (e.g. `/Game/Environment/Furniture`). Asset keeps its name.|

### Path Resolution

The asset path is resolved flexibly:

1. **Short form** — `/Game/MyAsset` (auto-appends `.MyAsset`)
2. **Full object path** — `/Game/MyAsset.MyAsset`
3. **Package name fallback** — searches by folder + name if the above fail

## Output

```json
{
  "asset_name": "SM_Chair",
  "old_path": "/Game/Meshes/SM_Chair",
  "new_path": "/Game/Environment/Furniture/SM_Chair",
  "old_folder": "/Game/Meshes",
  "new_folder": "/Game/Environment/Furniture",
  "asset_class": "StaticMesh",
  "success": true
}
```

| Field         | Description                                           |
| ------------- | ----------------------------------------------------- |
| `asset_name`  | Asset name (unchanged)                                |
| `old_path`    | Original full path                                    |
| `new_path`    | New full path after move                              |
| `old_folder`  | Folder the asset was in before the move               |
| `new_folder`  | Folder the asset now resides in                       |
| `asset_class` | Asset class name (e.g. `Blueprint`, `StaticMesh`)     |
| `success`     | `true` if move succeeded                              |

## Errors

| Scenario                  | Error message contains    |
| ------------------------- | ------------------------- |
| Asset not found           | `"not found"`             |
| Failed to load            | `"Failed to load"`        |
| Already in that folder    | `"already in folder"`     |
| Name conflict at target   | `"already exists"`        |
| Move failed               | `"Failed to move"`        |
| Missing `destination_path`| `"destination_path"`      |
| Missing `asset_path`      | `"asset_path"`            |

## Notes

- Uses `IAssetTools::RenameAssets()` which works for **all asset types**, not just Blueprints.
- Unreal automatically creates redirectors at the old path so existing references keep working.
- The asset name is preserved — use `rename_asset` if you also need to change the name.
- Undo data stores the original folder for reversal.
