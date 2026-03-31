# get_asset_info

Get detailed metadata about a specific asset without loading it into memory.

## Input

```json
{
  "asset_path": "/Game/Meshes/SM_Chair",
  "include_referencers": true,
  "include_dependencies": true,
  "include_tags": true
}
```

| Parameter              | Required | Description                                                                 |
| ---------------------- | -------- | --------------------------------------------------------------------------- |
| `asset_path`           | **yes**  | Asset path (short `/Game/MyAsset` or full `/Game/MyAsset.MyAsset`)          |
| `include_referencers`  | no       | Include assets that reference this asset (default: `true`)                  |
| `include_dependencies` | no       | Include assets this asset depends on (default: `true`)                      |
| `include_tags`         | no       | Include all asset registry tags/metadata (default: `true`)                  |

## Output

```json
{
  "name": "SM_Chair",
  "asset_path": "/Game/Meshes/SM_Chair.SM_Chair",
  "package_name": "/Game/Meshes/SM_Chair",
  "package_path": "/Game/Meshes",
  "asset_class": "StaticMesh",
  "asset_class_path": "/Script/Engine.StaticMesh",
  "disk_size": 524288,
  "is_loaded": false,
  "is_dirty": false,
  "tag_count": 12,
  "tags": {
    "NaniteEnabled": "False",
    "Triangles": "1234",
    "LODs": "4"
  },
  "referencer_count": 2,
  "referencers": [
    {
      "package_name": "/Game/Maps/MainLevel",
      "name": "MainLevel",
      "asset_path": "/Game/Maps/MainLevel.MainLevel",
      "asset_class": "World"
    }
  ],
  "dependency_count": 1,
  "dependencies": [
    {
      "package_name": "/Game/Materials/M_Wood",
      "name": "M_Wood",
      "asset_path": "/Game/Materials/M_Wood.M_Wood",
      "asset_class": "Material"
    }
  ]
}
```

| Field              | Description                                                       |
| ------------------ | ----------------------------------------------------------------- |
| `name`             | Asset name                                                        |
| `asset_path`       | Full object path                                                  |
| `package_name`     | Package name                                                      |
| `package_path`     | Package directory                                                 |
| `asset_class`      | Short class name (e.g. `StaticMesh`, `Blueprint`)                 |
| `asset_class_path` | Full class path (e.g. `/Script/Engine.StaticMesh`)                |
| `disk_size`        | On-disk size in bytes (0 if not saved yet)                        |
| `is_loaded`        | Whether the asset is currently loaded in memory                   |
| `is_dirty`         | Whether the package has unsaved changes (only if loaded)          |
| `tags`             | All asset registry tags as key-value pairs (if `include_tags`)    |
| `tag_count`        | Number of tags                                                    |
| `referencers`      | Assets referencing this asset (if `include_referencers`)          |
| `referencer_count`  | Number of referencers                                             |
| `dependencies`     | Assets this depends on, excluding `/Script/` (if `include_dependencies`) |
| `dependency_count` | Number of dependencies                                            |
