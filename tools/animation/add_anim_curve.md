# add_anim_curve

Add a float curve to an AnimSequence. The curve is created empty; use `set_anim_curve_keys` to populate it with keyframes.

## Input

```json
{
  "asset_path": "/Game/Animations/AS_Walk",
  "curve_name": "FootIK_Alpha"
}
```

| Parameter    | Required | Default | Description                                  |
| ------------ | -------- | ------- | -------------------------------------------- |
| `asset_path` | **yes**  | —       | Content path to the AnimSequence.            |
| `curve_name` | **yes**  | —       | Name for the new curve (e.g. `"FootIK_Alpha"`). |

## Output

| Field          | Type   | Description                              |
| -------------- | ------ | ---------------------------------------- |
| `asset_path`   | string | Full content path of the animation asset |
| `curve_name`   | string | Name of the added curve                  |
| `curve_type`   | string | Always `"float"`                         |
| `total_curves` | number | Total curve count after addition         |

## Examples

### Add a curve

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
| Curve already exists   | `true`    | `Curve '...' already exists on asset '...'`          |
| Controller add failed  | `true`    | `Failed to add curve '...' to asset '...'`           |
