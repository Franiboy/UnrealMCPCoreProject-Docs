# get_foliage_instances

Query foliage instances placed in the current level, with optional type and bounding box filters.

## Input

```json
{
  "foliage_type": "/Game/Foliage/FT_OakTree",
  "min_x": -1000, "min_y": -1000, "min_z": -500,
  "max_x": 1000, "max_y": 1000, "max_z": 500,
  "max_results": 500
}
```

|| Parameter      | Required | Default | Description                                                  |
|| -------------- | -------- | ------- | ------------------------------------------------------------ |
|| `foliage_type` | no       | ---     | Filter by foliage type path or mesh name substring.          |
|| `min_x`        | no       | ---     | Bounding box minimum X. All 6 bounds required together.      |
|| `min_y`        | no       | ---     | Bounding box minimum Y. All 6 bounds required together.      |
|| `min_z`        | no       | ---     | Bounding box minimum Z. All 6 bounds required together.      |
|| `max_x`        | no       | ---     | Bounding box maximum X. All 6 bounds required together.      |
|| `max_y`        | no       | ---     | Bounding box maximum Y. All 6 bounds required together.      |
|| `max_z`        | no       | ---     | Bounding box maximum Z. All 6 bounds required together.      |
|| `max_results`  | no       | `1000`  | Maximum instances returned per foliage type.                 |

## Output

|| Field                                        | Type   | Description                                    |
|| -------------------------------------------- | ------ | ---------------------------------------------- |
|| `type_count`                                 | number | Number of distinct foliage types found         |
|| `total_instances`                            | number | Total foliage instances across all types       |
|| `foliage_types`                              | array  | Array of foliage type result objects           |
|| `foliage_types[].mesh`                       | string | Static Mesh name                               |
|| `foliage_types[].mesh_path`                  | string | Content path to the Static Mesh                |
|| `foliage_types[].total_instances`            | number | Total instances of this type in the level      |
|| `foliage_types[].returned_instances`         | number | Number of instances returned (≤ max_results)   |
|| `foliage_types[].instances`                  | array  | Array of instance objects                      |
|| `foliage_types[].instances[].index`          | number | Instance index                                 |
|| `foliage_types[].instances[].location`       | object | `{x, y, z}` — World location                  |
|| `foliage_types[].instances[].rotation`       | object | `{pitch, yaw, roll}` — Rotation in degrees    |
|| `foliage_types[].instances[].scale`          | object | `{x, y, z}` — Scale                           |

## Examples

### Query all foliage instances

```json
{}
```

### Filter by type

```json
{
  "foliage_type": "/Game/Foliage/FT_OakTree"
}
```

### Query within a bounding box

```json
{
  "min_x": -5000, "min_y": -5000, "min_z": 0,
  "max_x": 5000, "max_y": 5000, "max_z": 10000,
  "max_results": 200
}
```

## Errors

|| Condition              | `isError` | Message                                |
|| ---------------------- | --------- | -------------------------------------- |
|| No editor world        | `true`    | `No editor world available`            |
