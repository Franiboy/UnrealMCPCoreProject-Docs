# set_world_settings

Modify world settings for the current editor level. Accepts a properties array of `{property_path, value}` objects using the same `section.key` naming as `get_world_settings`. Returns old and new values for each change, with partial success support.

## Input

```json
{
  "properties": [
    {"property_path": "physics.kill_z", "value": -50000.0},
    {"property_path": "physics.global_gravity_set", "value": true},
    {"property_path": "lightmass.diffuse_boost", "value": 2.5}
  ]
}
```

|| Parameter    | Required | Description                                                                     |
|| ------------ | -------- | ------------------------------------------------------------------------------- |
|| `properties` | **yes**  | Array of property changes. Each entry has `property_path` (string) and `value`. |

### Supported property paths

#### Physics (`physics.*`)

|| Path                                | Type   | Description                                      |
|| ----------------------------------- | ------ | ------------------------------------------------ |
|| `physics.global_gravity_z`          | number | Custom gravity Z value                           |
|| `physics.global_gravity_set`        | bool   | Whether custom gravity is enabled                |
|| `physics.enable_world_bounds_checks`| bool   | Enable world bounds checking                     |
|| `physics.kill_z`                    | number | Z value below which actors are destroyed         |
|| `physics.world_to_meters`           | number | World-to-meters scale (default: 100)             |

#### Tick / Time (`tick.*`)

|| Path                                | Type   | Description                                      |
|| ----------------------------------- | ------ | ------------------------------------------------ |
|| `tick.min_global_time_dilation`      | number | Minimum allowed global time dilation             |
|| `tick.max_global_time_dilation`      | number | Maximum allowed global time dilation             |
|| `tick.min_undilated_frame_time`      | number | Minimum undilated frame time                     |
|| `tick.max_undilated_frame_time`      | number | Maximum undilated frame time                     |

#### Rendering (`rendering.*`)

|| Path                                                      | Type   | Description                                      |
|| --------------------------------------------------------- | ------ | ------------------------------------------------ |
|| `rendering.default_max_distance_field_occlusion_distance` | number | Max distance field occlusion distance             |
|| `rendering.global_distance_field_view_distance`           | number | Global distance field view distance               |
|| `rendering.dynamic_indirect_shadows_self_shadowing_intensity` | number | Dynamic indirect shadow self-shadowing        |
|| `rendering.precompute_visibility`                         | bool   | Enable precomputed visibility                     |
|| `rendering.visibility_cell_size`                          | number | Size of visibility cells (int)                    |

#### Lightmass (`lightmass.*`)

|| Path                                              | Type   | Description                                      |
|| ------------------------------------------------- | ------ | ------------------------------------------------ |
|| `lightmass.static_lighting_level_scale`           | number | Static lighting level scale                      |
|| `lightmass.num_indirect_lighting_bounces`         | number | Number of indirect lighting bounces (int)        |
|| `lightmass.num_sky_lighting_bounces`              | number | Number of sky lighting bounces (int)             |
|| `lightmass.indirect_lighting_quality`             | number | Indirect lighting quality multiplier             |
|| `lightmass.indirect_lighting_smoothness`          | number | Indirect lighting smoothness                     |
|| `lightmass.environment_intensity`                 | number | Environment color intensity                      |
|| `lightmass.diffuse_boost`                         | number | Diffuse boost multiplier                         |
|| `lightmass.use_ambient_occlusion`                 | bool   | Enable lightmass ambient occlusion               |
|| `lightmass.compress_lightmaps`                    | bool   | Compress lightmap textures                       |
|| `lightmass.force_no_precomputed_lighting`         | bool   | Force disable all precomputed lighting           |
|| `lightmass.packed_light_and_shadow_map_texture_size` | number | Packed lightmap/shadowmap texture size (int)  |

#### Misc (`misc.*`)

|| Path                           | Type | Description                                         |
|| ------------------------------ | ---- | --------------------------------------------------- |
|| `misc.minimize_bsp_sections`   | bool | Minimize BSP sections for optimization              |

### Value types

- **bool properties**: Use JSON `true` / `false`
- **number properties**: Use JSON numbers (`42`, `3.14`, `-1000000.0`)

## Output

### Success

```json
{
  "success_count": 2,
  "fail_count": 0,
  "results": [
    {
      "property_path": "physics.kill_z",
      "success": true,
      "old_value": -1000000.0,
      "new_value": -50000.0
    },
    {
      "property_path": "lightmass.diffuse_boost",
      "success": true,
      "old_value": 1.0,
      "new_value": 2.5
    }
  ]
}
```

|| Field           | Description                                                          |
|| --------------- | -------------------------------------------------------------------- |
|| `success_count` | Number of properties successfully changed                            |
|| `fail_count`    | Number of properties that failed                                     |
|| `results`       | Per-property result array with path, success, old/new values         |

### Partial Success

When some properties succeed and others fail, `isError` is `false`. Check `fail_count` and individual `results[].success` for details.

```json
{
  "success_count": 1,
  "fail_count": 1,
  "results": [
    {"property_path": "physics.kill_z", "success": true, "old_value": -1000000.0, "new_value": -50000.0},
    {"property_path": "nonexistent.path", "success": false, "error": "Unknown or unsupported property path 'nonexistent.path'"}
  ]
}
```

### Error Cases

|| Condition                  | Error message contains                                |
|| -------------------------- | ----------------------------------------------------- |
|| Missing `properties`       | `"Missing or empty required parameter: properties"`   |
|| Empty `properties` array   | `"Missing or empty required parameter: properties"`   |
|| Unknown property path      | `"Unknown or unsupported property path '...'"`        |
|| Missing `value` field      | `"Missing value"`                                     |
|| No editor world            | `"No editor world available"`                         |
|| No world settings          | `"No world settings available"`                       |

## Notes

- Property paths use the same `section.key` naming as `get_world_settings` output, making it easy to read a value and write it back.
- The `old_value` and `new_value` fields can be fed back to `set_world_settings` for undo.
- The lightmass section is only available in editor builds (`WITH_EDITORONLY_DATA`).
- Boolean properties on `AWorldSettings` are stored as `uint8` bitfields, so they are reported as string `"true"` / `"false"` in the results.
- Numeric properties report their values as JSON numbers.
- The world settings actor is marked as modified after successful changes (`Modify()` + `MarkPackageDirty()`).
