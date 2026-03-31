# set_anim_montage_slot

Change the slot name on a montage track. Each AnimMontage has one or more slot tracks; this tool changes the slot name on a track at the given index.

## Input

```json
{
  "asset_path": "/Game/Animations/AM_Attack",
  "slot_name": "UpperBody",
  "slot_index": 0
}
```

| Parameter    | Required | Default | Description                                            |
| ------------ | -------- | ------- | ------------------------------------------------------ |
| `asset_path` | **yes**  | —       | Content path to the AnimMontage.                       |
| `slot_name`  | **yes**  | —       | New slot name for the track (e.g. `"UpperBody"`).      |
| `slot_index` | no       | `0`     | Index of the slot track to modify (0-based).           |

## Output

| Field              | Type   | Description                                |
| ------------------ | ------ | ------------------------------------------ |
| `asset_path`       | string | Full content path of the montage           |
| `slot_index`       | number | Index of the modified slot track           |
| `old_slot_name`    | string | Previous slot name                         |
| `new_slot_name`    | string | New slot name                              |
| `total_slot_tracks`| number | Total number of slot tracks on the montage |

## Examples

### Change default slot to UpperBody

```json
{
  "asset_path": "/Game/Animations/AM_Attack",
  "slot_name": "UpperBody"
}
```

### Change a specific slot track by index

```json
{
  "asset_path": "/Game/Animations/AM_MultiSlot",
  "slot_name": "LowerBody",
  "slot_index": 1
}
```

## Errors

| Condition            | `isError` | Message                                              |
| -------------------- | --------- | ---------------------------------------------------- |
| Missing `asset_path` | `true`    | `Missing required parameter 'asset_path'`            |
| Missing `slot_name`  | `true`    | `Missing required parameter 'slot_name'`             |
| Asset not found      | `true`    | `Animation asset not found: '...'`                   |
| Not an AnimMontage   | `true`    | `Asset '...' is not an AnimMontage`                  |
| Index out of range   | `true`    | `Slot index ... is out of range [0, ...)`            |
