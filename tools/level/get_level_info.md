# get_level_info

Get information about the current editor level. Returns the level/map name, package path, actor counts grouped by class, streaming sub-levels, world bounds, and world type. All optional sections (class counts, streaming levels, bounds) are included by default and can be excluded individually.

## Input

```json
{
  "include_class_counts": true,
  "include_streaming_levels": true,
  "include_bounds": true
}
```

| Parameter                  | Required | Description                                                          |
| -------------------------- | -------- | -------------------------------------------------------------------- |
| `include_class_counts`     | no       | Include per-class actor breakdown (default: `true`).                 |
| `include_streaming_levels` | no       | Include streaming sub-level information (default: `true`).           |
| `include_bounds`           | no       | Compute and include world bounds from all actors (default: `true`).  |

All parameters are optional — calling with `{}` returns the full level info.

## Output

### Success

```json
{
  "level_name": "MyLevel",
  "package_name": "/Game/Maps/MyLevel",
  "file_path": "C:/MyProject/Content/Maps/MyLevel.umap",
  "world_type": "Editor",
  "is_dirty": false,
  "actor_count": 42,
  "actor_counts_by_class": {
    "StaticMeshActor": 15,
    "PointLight": 8,
    "DirectionalLight": 1,
    "SkyLight": 1,
    "PlayerStart": 1,
    "WorldSettings": 1
  },
  "streaming_level_count": 2,
  "streaming_levels": [
    {
      "package_name": "/Game/Maps/SubLevel_A",
      "is_loaded": true,
      "is_visible": true
    },
    {
      "package_name": "/Game/Maps/SubLevel_B",
      "is_loaded": false,
      "is_visible": false,
      "transform": {
        "location_x": 1000.0,
        "location_y": 0.0,
        "location_z": 0.0,
        "rotation_pitch": 0.0,
        "rotation_yaw": 0.0,
        "rotation_roll": 0.0,
        "scale_x": 1.0,
        "scale_y": 1.0,
        "scale_z": 1.0
      }
    }
  ],
  "bounds": {
    "min_x": -5000.0,
    "min_y": -5000.0,
    "min_z": -100.0,
    "max_x": 5000.0,
    "max_y": 5000.0,
    "max_z": 3000.0,
    "size_x": 10000.0,
    "size_y": 10000.0,
    "size_z": 3100.0
  }
}
```

| Field                    | Description                                                                   |
| ------------------------ | ----------------------------------------------------------------------------- |
| `level_name`             | Map name (PIE prefix stripped if present)                                     |
| `package_name`           | Unreal package name of the world                                              |
| `file_path`              | Absolute disk path of the `.umap` file (present if file exists)               |
| `world_type`             | `"Editor"`, `"PIE"`, `"Game"`, or `"Other"`                                  |
| `is_dirty`               | Whether the world package has unsaved changes                                 |
| `actor_count`            | Total number of actors in the persistent level                                |
| `actor_counts_by_class`  | Object mapping class name to count, sorted by count descending (when `include_class_counts=true`) |
| `streaming_level_count`  | Number of streaming sub-levels (when `include_streaming_levels=true`)         |
| `streaming_levels`       | Array of streaming level info (when count > 0 and `include_streaming_levels=true`) |
| `bounds`                 | World bounding box computed from all actor bounds (when `include_bounds=true` and actors exist) |

### Error Cases

| Condition            | Error message contains        |
| -------------------- | ----------------------------- |
| No editor world      | `"No editor world available"` |

## Notes

- Uses `GEditor->GetEditorWorldContext().World()` to get the current editor world.
- Actor counts use `TActorIterator<AActor>` which iterates the persistent level.
- Streaming levels are queried via `UWorld::GetStreamingLevels()`. Each entry shows load/visibility state and any level transform offset.
- World bounds are computed from all actors' component bounding boxes. The bounds field is omitted if no valid bounds are found.
- The `actor_counts_by_class` object is sorted by count descending, making it easy to see which actor types dominate the level.
