# list_assets

List and search all asset types in the project via the Asset Registry.

## Input

```json
{
  "path": "/Game/Meshes",
  "class": "StaticMesh",
  "search": "Chair",
  "include_engine_content": false,
  "limit": 100,
  "offset": 0
}
```

| Parameter              | Required | Description                                                                                           |
| ---------------------- | -------- | ----------------------------------------------------------------------------------------------------- |
| `path`                 | no       | Restrict to this path (default: `/Game`)                                                              |
| `class`                | no       | Filter by asset class name (e.g. `StaticMesh`, `Texture2D`, `Material`, `Blueprint`). Case-insensitive |
| `search`               | no       | Filter by name substring (case-insensitive)                                                           |
| `include_engine_content` | no     | Include engine/plugin content (default: `false`)                                                      |
| `limit`                | no       | Maximum assets to return (default: `1000`)                                                            |
| `offset`               | no       | Number of assets to skip for pagination (default: `0`)                                                |

### Supported Asset Classes

`StaticMesh`, `SkeletalMesh`, `Texture2D`, `TextureCube`, `Material`, `MaterialInstance`, `MaterialFunction`, `SoundWave`, `SoundCue`, `ParticleSystem`, `Blueprint`, `AnimSequence`, `AnimMontage`, `AnimBlueprint`, `BlendSpace`, `BlendSpace1D`, `Skeleton`, `PhysicsAsset`, `DataTable`, `CurveFloat`, `CurveVector`, `Font`, `World`, `WidgetBlueprint`, `NiagaraSystem`, `NiagaraEmitter`, and more.

## Output

```json
{
  "count": 2,
  "total_count": 15,
  "offset": 0,
  "limit": 100,
  "has_more": false,
  "assets": [
    {
      "name": "SM_Chair",
      "asset_path": "/Game/Meshes/SM_Chair.SM_Chair",
      "package_path": "/Game/Meshes",
      "asset_class": "StaticMesh",
      "asset_class_path": "/Script/Engine.StaticMesh",
      "disk_size": 524288
    }
  ],
  "path_filter": "/Game/Meshes",
  "class_filter": "StaticMesh",
  "search_filter": "Chair"
}
```

| Field          | Description                                                    |
| -------------- | -------------------------------------------------------------- |
| `count`        | Number of assets in this response page                         |
| `total_count`  | Total matching assets (before pagination)                      |
| `offset`       | Current offset (for pagination)                                |
| `limit`        | Current limit                                                  |
| `has_more`     | `true` if more results exist beyond this page                  |
| `next_offset`  | Next offset to use for the next page (only present if `has_more`) |
| `assets`       | Array of asset objects                                         |
| `path_filter`  | Applied path filter (if any)                                   |
| `class_filter` | Applied class filter (if any)                                  |
| `search_filter`| Applied search filter (if any)                                 |
