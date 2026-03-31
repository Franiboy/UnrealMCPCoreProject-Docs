# get_streaming_levels

List all streaming (sub-) levels in the current world with their load/visibility state, streaming priority, blocking flags, and optional transform offset and actor count.

## Input

```json
{
  "include_transform": true,
  "include_actor_count": false
}
```

| Parameter            | Required | Description                                                                     |
| -------------------- | -------- | ------------------------------------------------------------------------------- |
| `include_transform`  | no       | Include the level transform offset (default: `true`). Only appears when non-identity. |
| `include_actor_count`| no       | Include the actor count for loaded levels (default: `false`).                   |

All parameters are optional — calling with `{}` returns all streaming levels with default flags.

## Output

### Success

```json
{
  "count": 1,
  "levels": [
    {
      "package_name": "/Game/Maps/SubLevel_Forest",
      "class": "LevelStreamingDynamic",
      "streaming_state": "LoadedVisible",
      "is_loaded": true,
      "is_visible": true,
      "should_be_loaded": true,
      "should_be_visible": true,
      "is_state_pending": false,
      "streaming_priority": 0,
      "should_block_on_load": false,
      "should_block_on_unload": false,
      "disable_distance_streaming": false,
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
      },
      "actor_count": 42
    }
  ]
}
```

| Field                        | Type    | Description                                                         |
| ---------------------------- | ------- | ------------------------------------------------------------------- |
| `count`                      | number  | Total number of streaming levels in the world                       |
| `levels`                     | array   | Array of streaming level objects                                    |
| `levels[].package_name`      | string  | World asset package name (e.g. `/Game/Maps/SubLevel`)               |
| `levels[].class`             | string  | ULevelStreaming subclass name (e.g. `LevelStreamingDynamic`)        |
| `levels[].streaming_state`   | string  | Current state: `Removed`, `Unloaded`, `FailedToLoad`, `Loading`, `LoadedNotVisible`, `MakingVisible`, `LoadedVisible`, `MakingInvisible` |
| `levels[].is_loaded`         | bool    | Whether the level is currently loaded in memory                     |
| `levels[].is_visible`        | bool    | Whether the level is currently visible                              |
| `levels[].should_be_loaded`  | bool    | Whether the level should be loaded (target state)                   |
| `levels[].should_be_visible` | bool    | Whether the level should be visible (target state)                  |
| `levels[].is_state_pending`  | bool    | Whether a streaming state change is in progress                     |
| `levels[].streaming_priority`| number  | Streaming priority (higher = loaded first)                          |
| `levels[].should_block_on_load`   | bool | Block gameplay until this level finishes loading               |
| `levels[].should_block_on_unload` | bool | Block gameplay until this level finishes unloading             |
| `levels[].disable_distance_streaming` | bool | Disable distance-based streaming for this level            |
| `levels[].transform`         | object  | Level transform offset (only when `include_transform=true` and non-identity) |
| `levels[].actor_count`       | number  | Number of actors in the loaded level (only when `include_actor_count=true`)  |

### Error Cases

| Condition       | Error message contains         |
| --------------- | ------------------------------ |
| No editor world | `"No editor world available"`  |

## Notes

- Uses `World->GetStreamingLevels()` to iterate `ULevelStreaming*` objects.
- The `transform` field is omitted when the level transform equals the identity transform (no offset), even when `include_transform=true`.
- The `actor_count` field returns `0` when the level is not loaded (no `ULevel*` available).
- Projects with a single persistent level and no sub-levels will return `count: 0` with an empty `levels` array.
- The `streaming_state` maps directly from `ELevelStreamingState` enum values.
