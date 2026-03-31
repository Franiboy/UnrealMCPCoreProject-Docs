# set_blend_space_axis

Configure a BlendSpace axis (X or Y). Set the display name, min/max range, grid divisions, snap-to-grid, and wrap-input. Works with both BlendSpace (2D) and BlendSpace1D (X axis only).

## Input

```json
{
  "asset_path": "/Game/Animations/BS_Locomotion",
  "axis": "x",
  "name": "Speed",
  "min": 0.0,
  "max": 600.0,
  "grid_divisions": 6,
  "snap_to_grid": true,
  "wrap_input": false
}
```

| Parameter        | Required | Default | Description                                                        |
| ---------------- | -------- | ------- | ------------------------------------------------------------------ |
| `asset_path`     | **yes**  | —       | Content path to the BlendSpace asset.                              |
| `axis`           | **yes**  | —       | Which axis to configure: `"x"` or `"y"`. BlendSpace1D only has X. |
| `name`           | no       | —       | Display name for the axis (e.g. `"Speed"`, `"Direction"`).         |
| `min`            | no       | —       | Minimum axis value.                                                |
| `max`            | no       | —       | Maximum axis value.                                                |
| `grid_divisions` | no       | —       | Number of grid divisions (minimum 1).                              |
| `snap_to_grid`   | no       | —       | If true, samples snap to grid points on this axis.                 |
| `wrap_input`     | no       | —       | If true, input wraps around (cyclic). If false, input is clamped.  |

## Output

| Field              | Type   | Description                                         |
| ------------------ | ------ | --------------------------------------------------- |
| `asset_path`       | string | Full content path of the BlendSpace                 |
| `axis`             | string | Axis that was configured (`"x"` or `"y"`)           |
| `axis_config`      | object | `{name, min, max, grid_divisions, snap_to_grid, wrap_input}` |
| `blend_space_type` | string | `"BlendSpace"` or `"BlendSpace1D"`                  |
| `total_samples`    | number | Current number of samples in the BlendSpace         |

## Examples

### Set X axis range and name

```json
{
  "asset_path": "/Game/Animations/BS_Locomotion",
  "axis": "x",
  "name": "Speed",
  "min": 0.0,
  "max": 600.0
}
```

### Set Y axis with grid and snap

```json
{
  "asset_path": "/Game/Animations/BS_Locomotion",
  "axis": "y",
  "name": "Direction",
  "min": -180.0,
  "max": 180.0,
  "grid_divisions": 8,
  "snap_to_grid": true,
  "wrap_input": true
}
```

## Errors

| Condition              | `isError` | Message                                              |
| ---------------------- | --------- | ---------------------------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter 'asset_path'`            |
| Missing `axis`         | `true`    | `Missing required parameter 'axis'`                  |
| Invalid axis           | `true`    | `Invalid axis '...'. Must be 'x' or 'y'`            |
| Asset not found        | `true`    | `BlendSpace asset not found: '...'`                  |
| Not a BlendSpace       | `true`    | `Asset '...' is not a BlendSpace`                    |
| Y axis on 1D           | `true`    | `Cannot set Y axis on a BlendSpace1D`               |
| Invalid range          | `true`    | `Invalid range: min (...) must be less than max (...)` |
