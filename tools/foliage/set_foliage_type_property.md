# set_foliage_type_property

Set properties on an existing Foliage Type asset (painting, placement, instance settings, scalability).

## Input

```json
{
  "asset_path": "/Game/Foliage/FT_OakTree",
  "properties": {
    "density": 150.0,
    "radius": 250.0,
    "align_to_normal": true,
    "cull_distance_end": 15000,
    "enable_density_scaling": true
  }
}
```

|| Parameter    | Required | Default | Description                                                               |
|| ------------ | -------- | ------- | ------------------------------------------------------------------------- |
|| `asset_path` | **yes**  | ---     | Content path to the Foliage Type asset.                                   |
|| `properties` | **yes**  | ---     | Object with one or more properties to set (see property list below).      |

### Painting properties

|| Property                    | Type   | Description                                                            |
|| --------------------------- | ------ | ---------------------------------------------------------------------- |
|| `density`                   | number | Painting density                                                       |
|| `density_adjustment_factor` | number | Density adjustment factor                                              |
|| `radius`                    | number | Minimum distance between instances                                     |
|| `scaling`                   | string | Scaling mode: `"Uniform"`, `"Free"`, `"LockXY"`, `"LockXZ"`, `"LockYZ"` |
|| `scale_x_min`               | number | Minimum X scale                                                        |
|| `scale_x_max`               | number | Maximum X scale                                                        |
|| `scale_y_min`               | number | Minimum Y scale                                                        |
|| `scale_y_max`               | number | Maximum Y scale                                                        |
|| `scale_z_min`               | number | Minimum Z scale                                                        |
|| `scale_z_max`               | number | Maximum Z scale                                                        |

### Placement properties

|| Property                  | Type   | Description                                      |
|| ------------------------- | ------ | ------------------------------------------------ |
|| `z_offset_min`            | number | Minimum Z offset                                 |
|| `z_offset_max`            | number | Maximum Z offset                                 |
|| `align_to_normal`         | bool   | Align instances to surface normal                |
|| `align_max_angle`         | number | Maximum alignment angle in degrees               |
|| `random_yaw`              | bool   | Apply random yaw rotation                        |
|| `random_pitch_angle`      | number | Random pitch angle in degrees                    |
|| `ground_slope_angle_min`  | number | Minimum ground slope angle                       |
|| `ground_slope_angle_max`  | number | Maximum ground slope angle                       |
|| `height_min`              | number | Minimum placement height                         |
|| `height_max`              | number | Maximum placement height                         |
|| `collision_with_world`    | bool   | Check collision with world geometry              |

### Instance settings properties

|| Property                  | Type   | Description                                      |
|| ------------------------- | ------ | ------------------------------------------------ |
|| `mobility`                | string | `"Static"`, `"Stationary"`, or `"Movable"`       |
|| `cull_distance_start`     | number | Start cull distance                              |
|| `cull_distance_end`       | number | End cull distance                                |
|| `cast_shadow`             | bool   | Whether instances cast shadows                   |
|| `cast_dynamic_shadow`     | bool   | Whether instances cast dynamic shadows           |
|| `cast_static_shadow`      | bool   | Whether instances cast static shadows            |
|| `receives_decals`         | bool   | Whether instances receive decals                 |
|| `override_lightmap_res`   | bool   | Override lightmap resolution                     |
|| `overridden_lightmap_res` | number | Overridden lightmap resolution value             |
|| `use_as_occluder`         | bool   | Use instances as occluders                       |

### Scalability properties

|| Property                        | Type | Description                           |
|| ------------------------------- | ---- | ------------------------------------- |
|| `enable_density_scaling`        | bool | Enable density-based scalability      |
|| `enable_cull_distance_scaling`  | bool | Enable cull-distance scalability      |

### Mesh property

|| Property | Type   | Description                                             |
|| -------- | ------ | ------------------------------------------------------- |
|| `mesh`   | string | Content path to a new Static Mesh (ISM types only)      |

## Output

|| Field            | Type   | Description                                     |
|| ---------------- | ------ | ----------------------------------------------- |
|| `asset_path`     | string | Content path to the Foliage Type asset          |
|| `properties_set` | number | Count of properties successfully modified       |
|| `values`         | object | Map of property name → new value                |

## Examples

### Update painting properties

```json
{
  "asset_path": "/Game/Foliage/FT_OakTree",
  "properties": {
    "density": 200.0,
    "radius": 300.0,
    "scale_x_min": 0.8,
    "scale_x_max": 1.3
  }
}
```

### Update placement and instance settings

```json
{
  "asset_path": "/Game/Foliage/FT_OakTree",
  "properties": {
    "align_to_normal": true,
    "random_yaw": true,
    "cull_distance_start": 5000,
    "cull_distance_end": 15000,
    "cast_shadow": true
  }
}
```

### Change the mesh

```json
{
  "asset_path": "/Game/Foliage/FT_OakTree",
  "properties": {
    "mesh": "/Game/Meshes/SM_OakTree_v2"
  }
}
```

## Errors

|| Condition                    | `isError` | Message                                                |
|| ---------------------------- | --------- | ------------------------------------------------------ |
|| Missing `asset_path`         | `true`    | `'asset_path' is required`                             |
|| Missing `properties`         | `true`    | `'properties' object is required`                      |
|| No recognized properties     | `true`    | `No recognized properties in 'properties' object`      |
|| Asset not found              | `true`    | `Foliage Type not found: ...`                          |
