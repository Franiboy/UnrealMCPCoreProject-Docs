# set_anim_curve_keys

Set keyframes on a float curve of an AnimSequence. By default replaces all existing keys; set `clear_existing=false` to merge new keys with existing ones.

## Input

```json
{
  "asset_path": "/Game/Animations/AS_Walk",
  "curve_name": "FootIK_Alpha",
  "keys": [
    { "time": 0.0, "value": 0.0 },
    { "time": 0.5, "value": 1.0, "interpolation": "cubic" },
    { "time": 1.0, "value": 0.0 }
  ],
  "clear_existing": true
}
```

| Parameter        | Required | Default | Description                                                               |
| ---------------- | -------- | ------- | ------------------------------------------------------------------------- |
| `asset_path`     | **yes**  | —       | Content path to the AnimSequence.                                         |
| `curve_name`     | **yes**  | —       | Name of the curve to set keys on (must already exist).                    |
| `keys`           | **yes**  | —       | Array of keyframes: `[{time, value, interpolation?}, ...]`.               |
| `clear_existing` | no       | `true`  | If true, clear existing keys before setting new ones. If false, merge.    |

### Key object fields

| Field            | Required | Default    | Description                                 |
| ---------------- | -------- | ---------- | ------------------------------------------- |
| `time`           | **yes**  | —          | Time in seconds.                            |
| `value`          | **yes**  | —          | Curve value at this time.                   |
| `interpolation`  | no       | `"linear"` | `"linear"`, `"constant"`, or `"cubic"`.     |

## Output

| Field            | Type    | Description                              |
| ---------------- | ------- | ---------------------------------------- |
| `asset_path`     | string  | Full content path of the animation asset |
| `curve_name`     | string  | Name of the curve                        |
| `keys_set`       | number  | Number of keys that were set             |
| `cleared_existing` | boolean | Whether existing keys were cleared     |
| `total_keys`     | number  | Total key count after operation          |

## Examples

### Replace all keys

```json
{
  "asset_path": "/Game/Animations/AS_Walk",
  "curve_name": "FootIK_Alpha",
  "keys": [
    { "time": 0.0, "value": 0.0 },
    { "time": 0.5, "value": 1.0 },
    { "time": 1.0, "value": 0.0 }
  ]
}
```

### Merge keys with existing

```json
{
  "asset_path": "/Game/Animations/AS_Walk",
  "curve_name": "FootIK_Alpha",
  "keys": [
    { "time": 0.25, "value": 0.5, "interpolation": "cubic" }
  ],
  "clear_existing": false
}
```

## Errors

| Condition              | `isError` | Message                                              |
| ---------------------- | --------- | ---------------------------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter 'asset_path'`            |
| Missing `curve_name`   | `true`    | `Missing required parameter 'curve_name'`            |
| Missing/empty `keys`   | `true`    | `Missing or empty required parameter 'keys'`         |
| No valid keys          | `true`    | `No valid keys found in 'keys' array`                |
| Asset not found        | `true`    | `Animation asset not found: '...'`                   |
| Not an animation asset | `true`    | `Asset '...' is not an AnimSequence or AnimMontage`  |
| Curve not found        | `true`    | `Curve '...' not found on asset '...'`               |
