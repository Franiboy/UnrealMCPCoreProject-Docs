# remove_blend_space_sample

Remove a sample from a BlendSpace by index or by animation reference (first match). Returns the removed sample info and the remaining sample list.

## Input

```json
{
  "asset_path": "/Game/Animations/BS_Locomotion",
  "index": 0
}
```

| Parameter    | Required | Default | Description                                                                 |
| ------------ | -------- | ------- | --------------------------------------------------------------------------- |
| `asset_path` | **yes**  | —       | Content path to the BlendSpace asset.                                       |
| `index`      | no       | —       | Index of the sample to remove (0-based). Provide this or `animation`.       |
| `animation`  | no       | —       | Content path to the AnimSequence to remove (first match). Provide this or `index`. |

At least one of `index` or `animation` must be provided.

## Output

| Field               | Type   | Description                                     |
| ------------------- | ------ | ----------------------------------------------- |
| `asset_path`        | string | Full content path of the BlendSpace             |
| `removed_index`     | number | Index of the removed sample                     |
| `removed_animation` | string | Path of the removed AnimSequence                |
| `removed_x`         | number | X position of the removed sample                |
| `removed_y`         | number | Y position of the removed sample                |
| `remaining_samples` | number | Remaining sample count after removal            |
| `samples`           | array  | Updated sample list: `[{index, animation, x, y, rate_scale}, ...]` |

## Examples

### Remove by index

```json
{
  "asset_path": "/Game/Animations/BS_Locomotion",
  "index": 2
}
```

### Remove by animation reference

```json
{
  "asset_path": "/Game/Animations/BS_Locomotion",
  "animation": "/Game/Animations/AS_Walk"
}
```

## Errors

| Condition                  | `isError` | Message                                              |
| -------------------------- | --------- | ---------------------------------------------------- |
| Missing `asset_path`       | `true`    | `Missing required parameter 'asset_path'`            |
| No identifier provided     | `true`    | `Must provide 'index' or 'animation' to ...`        |
| BlendSpace not found       | `true`    | `BlendSpace asset not found: '...'`                  |
| Not a BlendSpace           | `true`    | `Asset '...' is not a BlendSpace`                    |
| Index out of range         | `true`    | `Sample index ... is out of range [0, ...)`          |
| Animation not found        | `true`    | `AnimSequence not found: '...'`                      |
| No sample with animation   | `true`    | `No sample with animation '...' found in BlendSpace` |
| Delete failed              | `true`    | `Failed to delete sample at index ...`               |
