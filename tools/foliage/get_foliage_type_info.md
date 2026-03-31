# get_foliage_type_info

Detailed inspection of a Foliage Type asset: mesh, painting, placement, instance settings, and scalability.

## Input

```json
{
  "asset_path": "/Game/Foliage/FT_OakTree"
}
```

|| Parameter    | Required | Default | Description                                |
|| ------------ | -------- | ------- | ------------------------------------------ |
|| `asset_path` | **yes**  | ---     | Content path to the Foliage Type asset.    |

## Output

|| Field                                  | Type    | Description                                          |
|| -------------------------------------- | ------- | ---------------------------------------------------- |
|| `name`                                 | string  | Asset name                                           |
|| `asset_path`                           | string  | Content path to the Foliage Type asset               |
|| `type`                                 | string  | `"InstancedStaticMesh"` or `"Actor"`                 |
|| `mesh`                                 | string  | Static Mesh name (ISM types only)                    |
|| `mesh_path`                            | string  | Content path to the Static Mesh (ISM types only)     |
|| `override_materials`                   | array   | Array of override materials, if any                  |
|| `painting.density`                     | number  | Painting density                                     |
|| `painting.density_adjustment_factor`   | number  | Density adjustment factor                            |
|| `painting.radius`                      | number  | Minimum distance between instances                   |
|| `painting.scaling`                     | string  | Scaling mode                                         |
|| `painting.scale_x`                     | object  | `{min, max}` — X axis scale range                    |
|| `painting.scale_y`                     | object  | `{min, max}` — Y axis scale range                    |
|| `painting.scale_z`                     | object  | `{min, max}` — Z axis scale range                    |
|| `placement.z_offset`                   | object  | `{min, max}` — Z offset range                        |
|| `placement.align_to_normal`            | bool    | Align instances to surface normal                    |
|| `placement.align_max_angle`            | number  | Maximum alignment angle in degrees                   |
|| `placement.random_yaw`                 | bool    | Apply random yaw rotation                            |
|| `placement.random_pitch_angle`         | number  | Random pitch angle in degrees                        |
|| `placement.ground_slope_angle`         | object  | `{min, max}` — Ground slope angle range              |
|| `placement.height`                     | object  | `{min, max}` — Placement height range                |
|| `placement.landscape_layers`           | array   | Landscape layer names for painting                   |
|| `placement.collision_with_world`       | bool    | Check collision with world geometry                  |
|| `instance_settings.mobility`           | string  | Mobility: `"Static"`, `"Stationary"`, or `"Movable"` |
|| `instance_settings.cull_distance`      | object  | `{start, end}` — Cull distance range                 |
|| `instance_settings.cast_shadow`        | bool    | Whether instances cast shadows                       |
|| `instance_settings.cast_dynamic_shadow`| bool    | Whether instances cast dynamic shadows               |
|| `instance_settings.cast_static_shadow` | bool    | Whether instances cast static shadows                |
|| `instance_settings.receives_decals`    | bool    | Whether instances receive decals                     |
|| `instance_settings.override_lightmap_res` | bool | Override lightmap resolution                         |
|| `instance_settings.overridden_lightmap_res` | number | Overridden lightmap resolution value           |
|| `instance_settings.use_as_occluder`    | bool    | Use instances as occluders                           |
|| `scalability.enable_density_scaling`   | bool    | Enable density-based scalability                     |
|| `scalability.enable_discard_on_load`   | bool    | Enable discard-on-load scalability                   |
|| `scalability.enable_cull_distance_scaling` | bool | Enable cull-distance-based scalability              |

## Examples

### Inspect a foliage type

```json
{
  "asset_path": "/Game/Foliage/FT_OakTree"
}
```

### Inspect an engine foliage type

```json
{
  "asset_path": "/Game/Environment/Foliage/FT_Grass_01"
}
```

## Errors

|| Condition              | `isError` | Message                                |
|| ---------------------- | --------- | -------------------------------------- |
|| Missing `asset_path`   | `true`    | `'asset_path' is required`             |
|| Asset not found        | `true`    | `Foliage Type not found: ...`          |
