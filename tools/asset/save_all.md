# save_all

Save all dirty (modified) asset packages to disk in one call. Finds every loaded package with unsaved changes and writes it to its `.uasset` file. Returns per-package results showing what was saved. Optionally includes map (level) packages.

## Input

```json
{
  "include_maps": false
}
```

| Parameter      | Required | Description                                                                                        |
| -------------- | -------- | -------------------------------------------------------------------------------------------------- |
| `include_maps` | no       | If `true`, also save dirty map/level packages. Default: `false` (content packages only).           |

All parameters are optional — calling with `{}` saves all dirty content packages.

## Output

### Success (some packages saved)

```json
{
  "dirty_count": 3,
  "saved_count": 3,
  "failed_count": 0,
  "include_maps": false,
  "saved": [
    {
      "package_name": "/Game/Blueprints/BP_Player",
      "file": "C:/MyProject/Content/Blueprints/BP_Player.uasset"
    },
    {
      "package_name": "/Game/Blueprints/BP_Enemy",
      "file": "C:/MyProject/Content/Blueprints/BP_Enemy.uasset"
    },
    {
      "package_name": "/Game/Materials/M_Ground",
      "file": "C:/MyProject/Content/Materials/M_Ground.uasset"
    }
  ]
}
```

### Success (nothing dirty)

```json
{
  "dirty_count": 0,
  "saved_count": 0,
  "failed_count": 0,
  "include_maps": false
}
```

| Field          | Description                                                            |
| -------------- | ---------------------------------------------------------------------- |
| `dirty_count`  | Number of dirty packages found (after filtering)                       |
| `saved_count`  | Number of packages successfully saved                                  |
| `failed_count` | Number of packages that failed to save                                 |
| `include_maps` | Whether map packages were included in the scan                         |
| `saved`        | Array of saved package entries (present only when `saved_count > 0`)   |
| `failed`       | Array of failed package entries with error detail (present only when `failed_count > 0`) |

Each entry in `saved` or `failed` contains:

| Field          | Description                                   |
| -------------- | --------------------------------------------- |
| `package_name` | Unreal package name (e.g. `/Game/BP_Player`)  |
| `file`         | Absolute disk path of the `.uasset` file      |
| `error`        | Error message (only in `failed` entries)       |

### Error Cases

The tool returns `isError: true` only when every save operation fails. Partial failures are reported in the `failed` array while the tool still returns success.

## Notes

- Iterates all loaded `UPackage` objects and checks `IsDirty()` — only project content packages are included (engine, script, and transient packages are skipped).
- By default, map/level packages (`ContainsMap()`) are excluded. Pass `include_maps: true` to save those as well.
- Uses `UPackage::Save()` with `FSavePackageArgs` per package — the same mechanism used by the editor's Save command.
- After a successful `save_all`, calling it again immediately will report `dirty_count: 0` and `saved_count: 0`.
- Complementary to `save_asset` (which saves a single asset) — use `save_all` when you've made changes to multiple assets and want to persist everything at once.
