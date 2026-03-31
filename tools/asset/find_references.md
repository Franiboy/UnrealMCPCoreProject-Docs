# find_references

Find all assets that reference a given asset. Returns a list of referencers with their name, path, and class. Useful for impact analysis before renaming, moving, or deleting an asset.

## Input

```json
{
  "asset_path": "/Game/Meshes/SM_Chair",
  "recursive": false
}
```

| Parameter    | Required | Description                                                                                                    |
| ------------ | -------- | -------------------------------------------------------------------------------------------------------------- |
| `asset_path` | yes      | Asset path to find references for (e.g. `/Game/Meshes/SM_Chair`). Short form accepted.                         |
| `recursive`  | no       | If `true`, recursively find indirect referencers (transitive closure). Default: `false` (direct referencers only). |

### Path Resolution

The asset path is resolved flexibly:

1. **Short form** — `/Game/MyAsset` (auto-appends `.MyAsset`)
2. **Full object path** — `/Game/MyAsset.MyAsset`
3. **Package name fallback** — searches by folder + name if the above fail

## Output

### Success (direct mode)

```json
{
  "asset_path": "/Game/Meshes/SM_Chair.SM_Chair",
  "name": "SM_Chair",
  "asset_class": "StaticMesh",
  "referencers": [
    {
      "package_name": "/Game/Maps/MainLevel",
      "name": "MainLevel",
      "asset_path": "/Game/Maps/MainLevel.MainLevel",
      "asset_class": "World"
    },
    {
      "package_name": "/Game/Blueprints/BP_Chair",
      "name": "BP_Chair",
      "asset_path": "/Game/Blueprints/BP_Chair.BP_Chair",
      "asset_class": "Blueprint"
    }
  ],
  "referencer_count": 2,
  "recursive": false
}
```

### Success (recursive mode)

```json
{
  "asset_path": "/Game/Meshes/SM_Chair.SM_Chair",
  "name": "SM_Chair",
  "asset_class": "StaticMesh",
  "referencers": [
    {
      "package_name": "/Game/Blueprints/BP_Chair",
      "name": "BP_Chair",
      "asset_path": "/Game/Blueprints/BP_Chair.BP_Chair",
      "asset_class": "Blueprint",
      "direct": true
    },
    {
      "package_name": "/Game/Maps/MainLevel",
      "name": "MainLevel",
      "asset_path": "/Game/Maps/MainLevel.MainLevel",
      "asset_class": "World",
      "direct": false
    }
  ],
  "referencer_count": 2,
  "recursive": true
}
```

| Field              | Description                                                              |
| ------------------ | ------------------------------------------------------------------------ |
| `asset_path`       | Full path of the queried asset                                           |
| `name`             | Asset name                                                               |
| `asset_class`      | Asset class name                                                         |
| `referencers`      | Array of assets that reference this asset                                |
| `referencer_count`  | Number of referencers found                                              |
| `recursive`        | Whether recursive mode was used                                          |

### Referencer Object Fields

| Field          | Description                                                                   |
| -------------- | ----------------------------------------------------------------------------- |
| `package_name` | Package name of the referencing asset                                         |
| `name`         | Asset name (present if the package contains a known asset)                    |
| `asset_path`   | Full object path (present if the package contains a known asset)              |
| `asset_class`  | Asset class name (present if the package contains a known asset)              |
| `direct`       | `true` for direct referencers, `false` for indirect (only in recursive mode)  |

## Errors

| Scenario               | Error message contains  |
| ---------------------- | ----------------------- |
| Asset not found        | `"not found"`           |
| Missing `asset_path`   | `"asset_path"`          |

## Notes

- Uses the Asset Registry's `GetReferencers()` API — does not load assets into memory.
- Only package-level references are returned (property/value references are filtered out).
- Self-references are excluded.
- In recursive mode, a BFS traversal finds all direct and indirect referencers. Each result includes a `direct` boolean to distinguish direct from indirect references.
- Useful before `delete_asset`, `rename_asset`, or `move_asset` to understand the impact of changes.
