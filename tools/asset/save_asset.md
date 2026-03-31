# save_asset

Save an asset's package to disk. Persists any in-memory modifications (property changes, node edits, component additions, etc.) that have not yet been written to the `.uasset` file. When the package is not dirty and `force` is false, the tool returns success without writing anything.

## Input

```json
{
  "asset_path": "/Game/Blueprints/BP_Player",
  "force": false
}
```

| Parameter    | Required | Description                                                                                  |
| ------------ | -------- | -------------------------------------------------------------------------------------------- |
| `asset_path` | yes      | Content path of the asset to save (e.g. `/Game/Blueprints/BP_Player`). Short form accepted.  |
| `force`      | no       | If `true`, save even when the package is not dirty. Default: `false`.                        |

## Output

### Success (package was dirty and saved)

```json
{
  "asset_path": "/Game/Blueprints/BP_Player",
  "asset_name": "BP_Player",
  "asset_class": "Blueprint",
  "package_name": "/Game/Blueprints/BP_Player",
  "package_file": "../../../Content/Blueprints/BP_Player.uasset",
  "was_dirty": true,
  "saved": true,
  "success": true
}
```

### Success (package not dirty, skipped)

```json
{
  "asset_path": "/Game/Blueprints/BP_Player",
  "asset_name": "BP_Player",
  "asset_class": "Blueprint",
  "package_name": "/Game/Blueprints/BP_Player",
  "was_dirty": false,
  "saved": false,
  "message": "Package is not dirty, nothing to save. Use force=true to save anyway.",
  "success": true
}
```

| Field          | Description                                                       |
| -------------- | ----------------------------------------------------------------- |
| `asset_path`   | The content path (as provided)                                    |
| `asset_name`   | Name of the asset                                                 |
| `asset_class`  | UE class name of the asset                                        |
| `package_name` | Package name                                                      |
| `package_file` | Disk path of the saved `.uasset` file (present only when saved)   |
| `was_dirty`    | Whether the package had unsaved changes before the save           |
| `saved`        | `true` if the package was actually written to disk                |
| `message`      | Informational message (present when save was skipped)             |
| `success`      | Always `true` on success                                          |

### Error Cases

| Condition                | Error message contains     |
| ------------------------ | -------------------------- |
| Missing `asset_path`     | `"asset_path"`             |
| Asset not found          | `"not found"`              |
| Save operation failed    | `"Failed to save"`         |

## Notes

- Uses `UPackage::Save()` with `FSavePackageArgs` (the same mechanism used by the editor's Save command).
- The `was_dirty` field tells you whether the asset actually had pending changes before the save.
- When `force=false` (default) and the package is clean, the response is still `success=true` but `saved=false` — this is intentional, not an error.
- This tool does not compile Blueprints. If you need to compile before saving, call `compile_blueprint` first.
- Complementary to all tools that modify assets in memory (e.g. `set_blueprint_defaults`, `add_blueprint_variable`, etc.).
