# create_blend_space

Create a BlendSpace asset (1D or 2D) for a target skeleton. Configure axis names, ranges, and grid divisions. Samples can be added later with `add_blend_space_sample`.

## Input

```json
{
  "name": "BS_Locomotion",
  "skeleton": "/Game/Characters/SK_Mannequin",
  "type": "2d",
  "axis_x": { "name": "Speed", "min": 0, "max": 600, "grid_divisions": 6 },
  "axis_y": { "name": "Direction", "min": -180, "max": 180, "grid_divisions": 8 },
  "path": "/Game/Animations"
}
```

| Parameter  | Required | Default   | Description                                                                 |
| ---------- | -------- | --------- | --------------------------------------------------------------------------- |
| `name`     | **yes**  | —         | Name for the new BlendSpace asset.                                          |
| `skeleton` | **yes**  | —         | Content path to the target Skeleton asset.                                  |
| `type`     | no       | `"2d"`    | `"1d"` for BlendSpace1D or `"2d"` for BlendSpace.                          |
| `axis_x`   | no       | see below | X axis configuration object.                                               |
| `axis_y`   | no       | see below | Y axis configuration object (2D only, ignored for 1D).                     |
| `path`     | no       | `"/Game"` | Content folder path for the new asset.                                      |

### Axis Configuration Object

| Field            | Type   | Default | Description                        |
| ---------------- | ------ | ------- | ---------------------------------- |
| `name`           | string | `"X"` / `"Y"` | Display name for the axis.  |
| `min`            | number | `-100`  | Minimum axis value.                |
| `max`            | number | `100`   | Maximum axis value.                |
| `grid_divisions` | number | `4`     | Number of grid divisions.          |

## Output

| Field        | Type   | Description                                |
| ------------ | ------ | ------------------------------------------ |
| `name`       | string | Created asset name                         |
| `asset_path` | string | Full content path                          |
| `class`      | string | `"BlendSpace"` or `"BlendSpace1D"`         |
| `skeleton`   | string | Skeleton path                              |
| `axis_x`     | object | X axis config (name, min, max, grid_divisions) |
| `axis_y`     | object | Y axis config (2D only)                    |

## Examples

### Create 2D BlendSpace with custom axes

```json
{
  "name": "BS_Locomotion",
  "skeleton": "/Game/Characters/SK_Mannequin",
  "axis_x": { "name": "Speed", "min": 0, "max": 600 },
  "axis_y": { "name": "Direction", "min": -180, "max": 180 }
}
```

### Create 1D BlendSpace

```json
{
  "name": "BS_WalkRun",
  "skeleton": "/Game/Characters/SK_Mannequin",
  "type": "1d",
  "axis_x": { "name": "Speed", "min": 0, "max": 600, "grid_divisions": 3 }
}
```

### Create with defaults

```json
{
  "name": "BS_Default",
  "skeleton": "/Game/Characters/SK_Mannequin"
}
```

## Errors

| Condition              | `isError` | Message                                              |
| ---------------------- | --------- | ---------------------------------------------------- |
| Missing `name`         | `true`    | `Missing required parameter 'name'`                  |
| Missing `skeleton`     | `true`    | `Missing required parameter 'skeleton'`              |
| Skeleton not found     | `true`    | `Skeleton not found or not a Skeleton: '...'`        |
| Invalid type           | `true`    | `Invalid type '...'. Must be '1d' or '2d'.`          |
| Asset already exists   | `true`    | `An asset already exists at '...'`                   |
| Invalid path           | `true`    | `Invalid path '...'. Must start with /Game or /Engine.` |
