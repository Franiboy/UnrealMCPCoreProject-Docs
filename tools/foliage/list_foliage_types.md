# list_foliage_types

List Foliage Type assets (Instanced Static Mesh) with optional path/name filters.

## Input

```json
{
  "path_prefix": "/Game/Foliage",
  "name_filter": "Tree",
  "include_engine": false
}
```

|| Parameter        | Required | Default    | Description                                        |
|| ---------------- | -------- | ---------- | -------------------------------------------------- |
|| `path_prefix`    | no       | all paths  | Content path prefix to restrict search.            |
|| `name_filter`    | no       | ---        | Substring match on asset name.                     |
|| `include_engine` | no       | `false`    | Include engine/plugin assets in the results.       |

## Output

|| Field                    | Type   | Description                                     |
|| ------------------------ | ------ | ----------------------------------------------- |
|| `count`                  | number | Number of foliage types returned                |
|| `foliage_types`          | array  | Array of foliage type objects                   |
|| `foliage_types[].name`   | string | Asset name                                      |
|| `foliage_types[].asset_path` | string | Content path to the Foliage Type asset     |
|| `foliage_types[].mesh`   | string | Static Mesh name                                |
|| `foliage_types[].mesh_path` | string | Content path to the Static Mesh             |
|| `foliage_types[].density`    | number | Painting density                           |
|| `foliage_types[].radius`     | number | Minimum distance between instances         |
|| `foliage_types[].scale_x_min` | number | Minimum X scale                          |
|| `foliage_types[].scale_x_max` | number | Maximum X scale                          |
|| `foliage_types[].scale_y_min` | number | Minimum Y scale                          |
|| `foliage_types[].scale_y_max` | number | Maximum Y scale                          |
|| `foliage_types[].scale_z_min` | number | Minimum Z scale                          |
|| `foliage_types[].scale_z_max` | number | Maximum Z scale                          |

## Examples

### List all foliage types

```json
{}
```

### Filter by path and name

```json
{
  "path_prefix": "/Game/Environment/Foliage",
  "name_filter": "Bush"
}
```

### Include engine assets

```json
{
  "include_engine": true
}
```

## Errors

|| Condition | `isError` | Message |
|| --------- | --------- | ------- |
|| *(none)*  | ---       | Always succeeds; returns empty array if no matches. |
