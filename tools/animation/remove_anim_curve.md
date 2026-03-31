# remove_anim_curve

Remove a float curve from an AnimSequence. Returns the removed curve name, its key count, and remaining curve count.

## Input

```json
{
  "asset_path": "/Game/Animations/AS_Walk",
  "curve_name": "FootIK_Alpha"
}
```

| Parameter    | Required | Default | Description                          |
| ------------ | -------- | ------- | ------------------------------------ |
| `asset_path` | **yes**  | —       | Content path to the AnimSequence.    |
| `curve_name` | **yes**  | —       | Name of the curve to remove.         |

## Output

| Field              | Type   | Description                              |
| ------------------ | ------ | ---------------------------------------- |
| `asset_path`       | string | Full content path of the animation asset |
| `removed_curve`    | string | Name of the removed curve                |
| `removed_key_count`| number | Number of keys the curve had             |
| `remaining_curves` | number | Remaining curve count after removal      |

## Examples

### Remove a curve

```json
{
  "asset_path": "/Game/Animations/AS_Walk",
  "curve_name": "FootIK_Alpha"
}
```

## Errors

| Condition              | `isError` | Message                                              |
| ---------------------- | --------- | ---------------------------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter 'asset_path'`            |
| Missing `curve_name`   | `true`    | `Missing required parameter 'curve_name'`            |
| Asset not found        | `true`    | `Animation asset not found: '...'`                   |
| Not an animation asset | `true`    | `Asset '...' is not an AnimSequence or AnimMontage`  |
| Curve not found        | `true`    | `Curve '...' not found on asset '...'`               |
