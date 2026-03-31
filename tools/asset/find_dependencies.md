# find_dependencies

Find all assets that a given asset depends on. Returns a list of dependencies with their name, path, and class. Engine/script packages (`/Script/Engine`, `/Script/CoreUObject`, etc.) are excluded by default.

## Input

```json
{
  "asset_path": "/Game/Blueprints/BP_Player",
  "recursive": false,
  "include_script_packages": false
}
```

| Parameter                 | Required | Description                                                                                                    |
| ------------------------- | -------- | -------------------------------------------------------------------------------------------------------------- |
| `asset_path`              | yes      | Asset path to find dependencies for (e.g. `/Game/Meshes/SM_Chair`). Short form accepted.                       |
| `recursive`               | no       | If `true`, recursively find indirect dependencies (transitive closure). Default: `false`.                      |
| `include_script_packages` | no       | If `true`, include engine/script packages (e.g. `/Script/Engine`) in results. Default: `false`.                |

### Path Resolution

The asset path is resolved flexibly:

1. **Short form** — `/Game/MyAsset` (auto-appends `.MyAsset`)
2. **Full object path** — `/Game/MyAsset.MyAsset`
3. **Package name fallback** — searches by folder + name if the above fail

## Output

### Success (direct mode)

```json
{
  "asset_path": "/Game/Blueprints/BP_Player.BP_Player",
  "name": "BP_Player",
  "asset_class": "Blueprint",
  "dependencies": [
    {
      "package_name": "/Game/Meshes/SK_Mannequin",
      "name": "SK_Mannequin",
      "asset_path": "/Game/Meshes/SK_Mannequin.SK_Mannequin",
      "asset_class": "SkeletalMesh"
    },
    {
      "package_name": "/Game/Animations/ABP_Player",
      "name": "ABP_Player",
      "asset_path": "/Game/Animations/ABP_Player.ABP_Player",
      "asset_class": "AnimBlueprint"
    }
  ],
  "dependency_count": 2,
  "recursive": false,
  "include_script_packages": false
}
```

### Success (recursive mode)

```json
{
  "asset_path": "/Game/Blueprints/BP_Player.BP_Player",
  "name": "BP_Player",
  "asset_class": "Blueprint",
  "dependencies": [
    {
      "package_name": "/Game/Meshes/SK_Mannequin",
      "name": "SK_Mannequin",
      "asset_path": "/Game/Meshes/SK_Mannequin.SK_Mannequin",
      "asset_class": "SkeletalMesh",
      "direct": true
    },
    {
      "package_name": "/Game/Textures/T_Mannequin_D",
      "name": "T_Mannequin_D",
      "asset_path": "/Game/Textures/T_Mannequin_D.T_Mannequin_D",
      "asset_class": "Texture2D",
      "direct": false
    }
  ],
  "dependency_count": 2,
  "recursive": true,
  "include_script_packages": false
}
```

| Field                     | Description                                                              |
| ------------------------- | ------------------------------------------------------------------------ |
| `asset_path`              | Full path of the queried asset                                           |
| `name`                    | Asset name                                                               |
| `asset_class`             | Asset class name                                                         |
| `dependencies`            | Array of assets this asset depends on                                    |
| `dependency_count`        | Number of dependencies found                                             |
| `recursive`               | Whether recursive mode was used                                          |
| `include_script_packages` | Whether engine/script packages were included                             |

### Dependency Object Fields

| Field          | Description                                                                   |
| -------------- | ----------------------------------------------------------------------------- |
| `package_name` | Package name of the dependency                                                |
| `name`         | Asset name (present if the package contains a known asset)                    |
| `asset_path`   | Full object path (present if the package contains a known asset)              |
| `asset_class`  | Asset class name (present if the package contains a known asset)              |
| `direct`       | `true` for direct dependencies, `false` for indirect (only in recursive mode) |

## Errors

| Scenario               | Error message contains  |
| ---------------------- | ----------------------- |
| Asset not found        | `"not found"`           |
| Missing `asset_path`   | `"asset_path"`          |

## Notes

- Uses the Asset Registry's `GetDependencies()` API — does not load assets into memory.
- Only package-level dependencies are returned (property/value references are filtered out).
- Self-dependencies are excluded.
- By default, engine/script packages (`/Script/*`) are excluded to keep results focused on project content. Set `include_script_packages: true` to include them.
- In recursive mode, a BFS traversal finds all direct and indirect dependencies. Each result includes a `direct` boolean to distinguish direct from indirect dependencies.
- Complementary to `find_references` — use both together for full impact analysis.
