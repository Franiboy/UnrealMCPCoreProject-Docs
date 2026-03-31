# delete_asset

Delete any asset from the project. Works with all asset types (Blueprints, StaticMeshes, Materials, Textures, etc.). By default, checks for references and refuses to delete if the asset is referenced by other assets.

## Input

```json
{
  "asset_path": "/Game/Meshes/SM_Chair",
  "force": false
}
```

| Parameter    | Required | Description                                                                  |
| ------------ | -------- | ---------------------------------------------------------------------------- |
| `asset_path` | yes      | Asset path to delete (e.g. `/Game/Meshes/SM_Chair`). Short form accepted.    |
| `force`      | no       | If `true`, delete even if the asset is referenced (default: `false`).        |

### Path Resolution

The asset path is resolved flexibly:

1. **Short form** — `/Game/MyAsset` (auto-appends `.MyAsset`)
2. **Full object path** — `/Game/MyAsset.MyAsset`
3. **Package name fallback** — searches by folder + name if the above fail

## Output

### Success

```json
{
  "deleted": "/Game/Meshes/SM_Chair",
  "name": "SM_Chair",
  "asset_class": "StaticMesh",
  "success": true
}
```

| Field         | Description                                           |
| ------------- | ----------------------------------------------------- |
| `deleted`     | Full path of the deleted asset                        |
| `name`        | Asset name                                            |
| `asset_class` | Asset class name (e.g. `Blueprint`, `StaticMesh`)     |
| `success`     | `true` if deletion succeeded                          |

### Reference Error (when `force` is false)

```json
{
  "error": "Asset is referenced by other assets. Use 'force: true' to delete anyway.",
  "asset_path": "/Game/Meshes/SM_Chair",
  "asset_name": "SM_Chair",
  "asset_class": "StaticMesh",
  "referencers": ["/Game/Maps/MainLevel"],
  "reference_count": 1
}
```

## Errors

| Scenario                 | Error message contains     |
| ------------------------ | -------------------------- |
| Asset not found          | `"not found"`              |
| Failed to load           | `"Failed to load"`         |
| Has references           | `"has references"`         |
| Deletion failed          | `"Failed to delete"`       |
| Missing `asset_path`     | `"asset_path"`             |

## Notes

- Uses `ObjectTools::ForceDeleteObjects()` which works for **all asset types**, not just Blueprints.
- When `force` is false, the tool queries the Asset Registry for referencers and returns the list of referencing assets in the error response.
- Deletion is **irreversible** — there is no undo data for this tool.
- For Blueprint-specific deletion, `delete_blueprint` is also available (uses the same underlying mechanism but loads as `UBlueprint`).
