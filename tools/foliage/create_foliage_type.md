# create_foliage_type

Create a new Foliage Type (Instanced Static Mesh) asset from a Static Mesh.

## Input

```json
{
  "name": "FT_OakTree",
  "mesh": "/Game/Meshes/SM_OakTree",
  "path": "/Game/Foliage",
  "density": 100.0,
  "radius": 200.0,
  "scale_min": 0.8,
  "scale_max": 1.2,
  "align_to_normal": true,
  "random_yaw": true
}
```

|| Parameter        | Required | Default    | Description                                              |
|| ---------------- | -------- | ---------- | -------------------------------------------------------- |
|| `name`           | **yes**  | ---        | Asset name for the new Foliage Type.                     |
|| `mesh`           | **yes**  | ---        | Content path to the Static Mesh.                         |
|| `path`           | no       | `"/Game"`  | Package path where the asset will be created.            |
|| `density`        | no       | ---        | Painting density. Clamped ≥ 0.                           |
|| `radius`         | no       | ---        | Minimum distance between instances. Clamped ≥ 0.        |
|| `scale_min`      | no       | `1.0`      | Uniform minimum scale (applied to X/Y/Z).               |
|| `scale_max`      | no       | `1.0`      | Uniform maximum scale (applied to X/Y/Z).               |
|| `align_to_normal`| no       | ---        | Align instances to surface normal.                       |
|| `random_yaw`     | no       | ---        | Apply random yaw rotation.                               |

## Output

|| Field             | Type    | Description                                   |
|| ----------------- | ------- | --------------------------------------------- |
|| `name`            | string  | Asset name                                    |
|| `asset_path`      | string  | Content path to the created Foliage Type      |
|| `mesh`            | string  | Static Mesh name                              |
|| `mesh_path`       | string  | Content path to the Static Mesh               |
|| `density`         | number  | Painting density                              |
|| `radius`          | number  | Minimum distance between instances            |
|| `scale_x_min`     | number  | Minimum X scale                               |
|| `scale_x_max`     | number  | Maximum X scale                               |
|| `align_to_normal` | bool    | Whether aligned to surface normal             |
|| `random_yaw`      | bool    | Whether random yaw is enabled                 |

## Examples

### Create a basic foliage type

```json
{
  "name": "FT_PineTree",
  "mesh": "/Game/Meshes/SM_PineTree"
}
```

### Create with custom density and scale range

```json
{
  "name": "FT_Grass",
  "mesh": "/Game/Meshes/SM_Grass_Clump",
  "path": "/Game/Foliage/Grass",
  "density": 500.0,
  "radius": 50.0,
  "scale_min": 0.5,
  "scale_max": 1.5,
  "align_to_normal": true,
  "random_yaw": true
}
```

## Errors

|| Condition              | `isError` | Message                                    |
|| ---------------------- | --------- | ------------------------------------------ |
|| Missing `name`         | `true`    | `'name' is required`                       |
|| Missing `mesh`         | `true`    | `'mesh' is required`                       |
|| Mesh not found         | `true`    | `Static Mesh not found: ...`               |
|| Already exists         | `true`    | `Foliage Type already exists: ...`         |
