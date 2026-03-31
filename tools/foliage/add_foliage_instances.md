# add_foliage_instances

Programmatically place foliage instances in the current level.

## Input

```json
{
  "foliage_type": "/Game/Foliage/FT_OakTree",
  "instances": [
    {
      "location": { "x": 100, "y": 200, "z": 0 },
      "rotation": { "pitch": 0, "yaw": 45, "roll": 0 },
      "scale": { "x": 1.0, "y": 1.0, "z": 1.0 }
    },
    {
      "location": { "x": 500, "y": -300, "z": 0 },
      "scale": 1.2
    }
  ]
}
```

|| Parameter                    | Required | Default       | Description                                         |
|| ---------------------------- | -------- | ------------- | --------------------------------------------------- |
|| `foliage_type`               | **yes**  | ---           | Content path to the Foliage Type asset.             |
|| `instances`                  | **yes**  | ---           | Array of instance objects to place.                 |
|| `instances[].location`       | **yes**  | ---           | `{x, y, z}` — World location.                      |
|| `instances[].rotation`       | no       | `{0, 0, 0}`  | `{pitch, yaw, roll}` — Rotation in degrees.        |
|| `instances[].scale`          | no       | `1.0`         | `{x, y, z}` object or uniform number.              |

## Output

|| Field              | Type   | Description                                          |
|| ------------------ | ------ | ---------------------------------------------------- |
|| `foliage_type`     | string | Content path of the foliage type used                |
|| `instances_added`  | number | Number of instances successfully placed              |
|| `total_instances`  | number | Total instances of this type in the level after addition |

## Examples

### Place instances with full transforms

```json
{
  "foliage_type": "/Game/Foliage/FT_OakTree",
  "instances": [
    {
      "location": { "x": 0, "y": 0, "z": 100 },
      "rotation": { "pitch": 0, "yaw": 90, "roll": 0 },
      "scale": { "x": 1.0, "y": 1.0, "z": 1.0 }
    }
  ]
}
```

### Place instances with uniform scale

```json
{
  "foliage_type": "/Game/Foliage/FT_Grass",
  "instances": [
    { "location": { "x": 100, "y": 100, "z": 0 }, "scale": 0.8 },
    { "location": { "x": 200, "y": 100, "z": 0 }, "scale": 1.0 },
    { "location": { "x": 300, "y": 100, "z": 0 }, "scale": 1.2 }
  ]
}
```

### Place instances with default rotation and scale

```json
{
  "foliage_type": "/Game/Foliage/FT_Rock",
  "instances": [
    { "location": { "x": 1000, "y": 2000, "z": 50 } },
    { "location": { "x": 1500, "y": 2500, "z": 75 } }
  ]
}
```

## Errors

|| Condition              | `isError` | Message                                                    |
|| ---------------------- | --------- | ---------------------------------------------------------- |
|| Missing `foliage_type` | `true`    | `'foliage_type' is required`                               |
|| Missing `instances`    | `true`    | `'instances' array is required and must not be empty`      |
|| Asset not found        | `true`    | `Foliage Type not found: ...`                              |
|| No valid instances     | `true`    | `No valid instances to add`                                |
|| No editor world        | `true`    | `No editor world available`                                |
