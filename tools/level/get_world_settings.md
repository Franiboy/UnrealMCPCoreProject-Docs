# get_world_settings

Get the world settings for the current editor level. Returns game mode, physics/gravity, kill Z, world-to-meters scale, streaming/world partition, navigation, rendering, lightmass, and tick settings. Use include flags to control which sections are returned.

## Input

```json
{
  "include_physics": true,
  "include_rendering": true,
  "include_lightmass": true,
  "include_all_properties": false
}
```

|| Parameter              | Required | Description                                                                        |
|| ---------------------- | -------- | ---------------------------------------------------------------------------------- |
|| `include_physics`      | no       | Include physics, gravity, kill Z, and world-to-meters settings (default: `true`).  |
|| `include_rendering`    | no       | Include rendering and visibility settings (default: `true`).                       |
|| `include_lightmass`    | no       | Include lightmass build settings (default: `true`).                                |
|| `include_all_properties` | no     | Include a full reflection dump of all non-default world settings (default: `false`). |

All parameters are optional — calling with `{}` returns the full structured settings.

## Output

### Success

```json
{
  "game_mode": {
    "default_game_mode": "None"
  },
  "physics": {
    "global_gravity_set": false,
    "global_gravity_z": 0.0,
    "enable_world_bounds_checks": true,
    "kill_z": -1000000.0,
    "world_to_meters": 100.0
  },
  "streaming": {
    "enable_world_composition": false,
    "use_client_side_level_streaming_volumes": true,
    "enable_world_origin_rebasing": false,
    "has_world_partition": false
  },
  "navigation": {
    "enable_ai_system": true,
    "navigation_system_config_class": "NavigationSystemConfig"
  },
  "rendering": {
    "default_max_distance_field_occlusion_distance": 600.0,
    "global_distance_field_view_distance": 20000.0,
    "dynamic_indirect_shadows_self_shadowing_intensity": 0.8,
    "default_color_scale": { "r": 1.0, "g": 1.0, "b": 1.0 },
    "precompute_visibility": false,
    "visibility_cell_size": 200.0
  },
  "lightmass": {
    "static_lighting_level_scale": 1.0,
    "num_indirect_lighting_bounces": 3,
    "num_sky_lighting_bounces": 1,
    "indirect_lighting_quality": 1.0,
    "indirect_lighting_smoothness": 1.0,
    "environment_intensity": 1.0,
    "diffuse_boost": 1.0,
    "use_ambient_occlusion": false,
    "compress_lightmaps": true,
    "force_no_precomputed_lighting": false,
    "packed_light_and_shadow_map_texture_size": 1024
  },
  "tick": {
    "min_global_time_dilation": 0.0001,
    "max_global_time_dilation": 10.0,
    "min_undilated_frame_time": 0.0005,
    "max_undilated_frame_time": 0.04
  },
  "misc": {
    "minimize_bsp_sections": false
  }
}
```

|| Section        | Description                                                                      |
|| -------------- | -------------------------------------------------------------------------------- |
|| `game_mode`    | Default game mode class (always included)                                        |
|| `physics`      | Gravity, kill Z, world-to-meters, bounds checks (when `include_physics=true`)    |
|| `streaming`    | World composition, world partition, origin rebasing (always included)             |
|| `navigation`   | AI system enabled, navigation config class (always included)                     |
|| `rendering`    | Distance fields, color scale, precomputed visibility (when `include_rendering=true`) |
|| `lightmass`    | Lightmass build settings, lighting bounces, AO (when `include_lightmass=true`)   |
|| `tick`         | Global time dilation min/max, undilated frame time (always included)              |
|| `misc`         | BSP and other miscellaneous flags (always included)                              |
|| `all_properties` | Full reflection dump of non-default properties (when `include_all_properties=true`) |

### Error Cases

|| Condition            | Error message contains              |
|| -------------------- | ----------------------------------- |
|| No editor world      | `"No editor world available"`       |
|| No world settings    | `"No world settings available"`     |

## Notes

- Uses `GEditor->GetEditorWorldContext().World()->GetWorldSettings()` to access the current level's settings.
- The `lightmass` section is only available in editor builds (`WITH_EDITORONLY_DATA`).
- The `navigation.enable_ai_system` field is read via reflection since it is a protected member.
- The `include_all_properties` flag triggers `ExtractObjectProperties`, which compares against the CDO and only includes non-default editable properties.
- The `game_mode`, `streaming`, `navigation`, `tick`, and `misc` sections are always returned regardless of include flags.
