# add_blend_space_sample

Add an animation sample to a BlendSpace at a specified position (X, Y). The animation must be an AnimSequence. For BlendSpace1D, only X is used.

## Input

```json
{
  "asset_path": "/Game/Animations/BS_Locomotion",
  "animation": "/Game/Animations/AS_Walk",
  "x": 150.0,
  "y": 0.0
}
```

| Parameter    | Required | Default | Description                                                        |
| ------------ | -------- | ------- | ------------------------------------------------------------------ |
| `asset_path` | **yes**  | —       | Content path to the BlendSpace asset.                              |
| `animation`  | **yes**  | —       | Content path to the AnimSequence to add as a sample.               |
| `x`          | **yes**  | —       | X position for the sample (blend parameter 0).                     |
| `y`          | no       | `0`     | Y position for the sample (blend parameter 1, 2D only).           |

## Output

| Field           | Type   | Description                                     |
| --------------- | ------ | ----------------------------------------------- |
| `asset_path`    | string | Full content path of the BlendSpace             |
| `animation`     | string | Path of the added AnimSequence                  |
| `x`             | number | X position of the sample                        |
| `y`             | number | Y position of the sample (2D only)              |
| `index`         | number | Index of the added sample                       |
| `total_samples` | number | Total sample count after addition               |
| `samples`       | array  | Full sample list: `[{index, animation, x, y, rate_scale}, ...]` |

## Examples

### Add a sample to a 2D BlendSpace

```json
{
  "asset_path": "/Game/Animations/BS_Locomotion",
  "animation": "/Game/Animations/AS_Walk",
  "x": 150.0,
  "y": 0.0
}
```

### Add a sample to a 1D BlendSpace

```json
{
  "asset_path": "/Game/Animations/BS_WalkRun",
  "animation": "/Game/Animations/AS_Run",
  "x": 375.0
}
```

## Errors

| Condition                | `isError` | Message                                              |
| ------------------------ | --------- | ---------------------------------------------------- |
| Missing `asset_path`     | `true`    | `Missing required parameter 'asset_path'`            |
| Missing `animation`      | `true`    | `Missing required parameter 'animation'`             |
| Missing `x`              | `true`    | `Missing required parameter 'x'`                     |
| BlendSpace not found     | `true`    | `BlendSpace asset not found: '...'`                  |
| Not a BlendSpace         | `true`    | `Asset '...' is not a BlendSpace`                    |
| AnimSequence not found   | `true`    | `AnimSequence not found: '...'`                      |
| Not an AnimSequence      | `true`    | `Asset '...' is not an AnimSequence`                 |
| Add failed               | `true`    | `Failed to add sample to BlendSpace`                 |
