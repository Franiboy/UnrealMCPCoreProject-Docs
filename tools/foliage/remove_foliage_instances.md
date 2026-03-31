# remove_foliage_instances

Remove foliage instances from the current level by type, bounding box, or all.

## Input

```json
{
  "foliage_type": "/Game/Foliage/FT_OakTree",
  "remove_all": false,
  "min_x": -1000, "min_y": -1000, "min_z": -500,
  "max_x": 1000, "max_y": 1000, "max_z": 500
}
```

|| Parameter      | Required | Default | Description                                                                |
|| -------------- | -------- | ------- | -------------------------------------------------------------------------- |
|| `foliage_type` | no       | ---     | Content path to filter removal by foliage type.                            |
|| `remove_all`   | no       | ---     | If `true`, remove all instances (of specified type or all types).          |
|| `min_x`        | no       | ---     | Bounding box minimum X. All 6 bounds required together.                   |
|| `min_y`        | no       | ---     | Bounding box minimum Y. All 6 bounds required together.                   |
|| `min_z`        | no       | ---     | Bounding box minimum Z. All 6 bounds required together.                   |
|| `max_x`        | no       | ---     | Bounding box maximum X. All 6 bounds required together.                   |
|| `max_y`        | no       | ---     | Bounding box maximum Y. All 6 bounds required together.                   |
|| `max_z`        | no       | ---     | Bounding box maximum Z. All 6 bounds required together.                   |

> **Note:** At least one of `foliage_type`, bounding box (`min_x`..`max_z`), or `remove_all` must be specified.

## Output

|| Field                 | Type   | Description                                 |
|| --------------------- | ------ | ------------------------------------------- |
|| `foliage_type`        | string | Type path used or `"all"`                   |
|| `instances_removed`   | number | Number of instances removed                 |
|| `remaining_instances` | number | Remaining instances after removal           |

## Examples

### Remove all instances of a specific type

```json
{
  "foliage_type": "/Game/Foliage/FT_OakTree",
  "remove_all": true
}
```

### Remove instances within a bounding box

```json
{
  "foliage_type": "/Game/Foliage/FT_Grass",
  "min_x": -2000, "min_y": -2000, "min_z": 0,
  "max_x": 2000, "max_y": 2000, "max_z": 1000
}
```

### Remove all foliage from the level

```json
{
  "remove_all": true
}
```

## Errors

|| Condition              | `isError` | Message                                                                                      |
|| ---------------------- | --------- | -------------------------------------------------------------------------------------------- |
|| No criteria specified  | `true`    | `At least one of 'foliage_type', bounding box (min_x..max_z), or 'remove_all' must be specified` |
|| Type not found         | `true`    | `Foliage Type not found: ...`                                                                |
|| No editor world        | `true`    | `No editor world available`                                                                  |
