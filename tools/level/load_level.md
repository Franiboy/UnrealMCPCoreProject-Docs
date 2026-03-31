# load_level

Open a different level/map in the Unreal Editor. Accepts a full asset path, a path with object suffix, or a bare map name resolved via Asset Registry. Optionally saves the current level before switching.

## Input

```json
{
  "map_path": "/Game/Maps/MyLevel",
  "save_current": true
}
```

| Parameter      | Required | Description                                                                           |
| -------------- | -------- | ------------------------------------------------------------------------------------- |
| `map_path`     | **yes**  | Path to the map. Accepts: full asset path `/Game/Maps/MyMap`, path with suffix `/Game/Maps/MyMap.MyMap`, or bare name `MyMap` (resolved via Asset Registry). |
| `save_current` | no       | Save the current level before loading the new one (default: `true`). Only saves if there are unsaved changes. |

## Output

### Success — loaded

```json
{
  "status": "loaded",
  "level_name": "StarterMap",
  "package_name": "/Game/StarterContent/Maps/StarterMap",
  "actor_count": 258,
  "previous_level": "Minimal_Default",
  "previous_package": "/Game/StarterContent/Maps/Minimal_Default"
}
```

### Success — already loaded

If the requested map is already the current level, returns immediately without reloading:

```json
{
  "status": "already_loaded",
  "level_name": "Minimal_Default",
  "package_name": "/Game/StarterContent/Maps/Minimal_Default"
}
```

| Field              | Type   | Description                                            |
| ------------------ | ------ | ------------------------------------------------------ |
| `status`           | string | `"loaded"` or `"already_loaded"`                       |
| `level_name`       | string | Display name of the new level (PIE prefix stripped)    |
| `package_name`     | string | Full package path of the new level                     |
| `actor_count`      | number | Number of actors in the new level (only when `loaded`) |
| `previous_level`   | string | Display name of the previous level (only when `loaded`)|
| `previous_package` | string | Full package path of the previous level (only when `loaded`) |

### Error Cases

| Condition                  | Error message contains                                              |
| -------------------------- | ------------------------------------------------------------------- |
| Missing `map_path`         | `"Missing required parameter 'map_path'"`                           |
| Empty `map_path`           | `"Missing required parameter 'map_path'"`                           |
| Bare name not found        | `"Map 'X' not found. Use a full path like '/Game/Maps/MyMap'..."`   |
| Package path doesn't exist | `"Map package not found: '/Game/Maps/X'"`                           |
| Load failed                | `"Failed to load map: '/Game/Maps/X'"`                              |

## Notes

- Uses `FEditorFileUtils::LoadMap()` to switch the editor world. This is a synchronous operation that replaces the entire persistent level.
- When `save_current=true` (default), only saves if the current level's package is dirty (`IsDirty()`).
- Bare map names (no `/` characters) are resolved by querying the Asset Registry for all World assets and matching by `AssetName`.
- Object suffixes (e.g. `/Game/Maps/MyMap.MyMap`) are stripped automatically before loading.
- If the requested map is already the current editor level (same package name), returns `"already_loaded"` without reloading.
- Loading a new level invalidates all actor references from the previous level. Other tools operating on actors (e.g. `set_actor_property`, `get_actor_info`) will see the new level's actors after this call.
- Test file uses `"run_alone": true` to prevent parallel execution with other level tests (since load_level changes the shared editor world).
