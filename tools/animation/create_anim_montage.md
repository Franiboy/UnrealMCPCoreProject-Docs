# create_anim_montage

Create an AnimMontage asset. Optionally populate it from a source AnimSequence. The montage is created with a single slot track (configurable name) and, when a source animation is provided, a segment covering the full sequence duration.

## Input

```json
{
  "name": "AM_Attack",
  "skeleton": "/Game/Characters/SK_Mannequin",
  "source_animation": "/Game/Animations/AS_Attack",
  "slot_name": "DefaultSlot",
  "path": "/Game/Animations"
}
```

| Parameter          | Required | Default          | Description                                                      |
| ------------------ | -------- | ---------------- | ---------------------------------------------------------------- |
| `name`             | **yes**  | —                | Name for the new AnimMontage asset.                              |
| `skeleton`         | **yes**  | —                | Content path to the target Skeleton asset.                       |
| `source_animation` | no       | —                | Content path to an AnimSequence to populate the montage with.    |
| `slot_name`        | no       | `"DefaultSlot"`  | Slot name for the montage track.                                 |
| `path`             | no       | `"/Game"`        | Content folder path for the new asset.                           |

## Output

| Field              | Type   | Description                                      |
| ------------------ | ------ | ------------------------------------------------ |
| `name`             | string | Created asset name                               |
| `asset_path`       | string | Full content path                                |
| `class`            | string | Always `"AnimMontage"`                           |
| `skeleton`         | string | Skeleton path                                    |
| `source_animation` | string | Source AnimSequence path (if provided)            |
| `duration`         | number | Montage play length in seconds (if source given) |
| `slot_name`        | string | Name of the slot track                           |
| `section_count`    | number | Number of composite sections                     |

## Examples

### Create empty montage

```json
{
  "name": "AM_EmptyMontage",
  "skeleton": "/Game/Characters/SK_Mannequin"
}
```

### Create montage from AnimSequence with custom slot

```json
{
  "name": "AM_Attack",
  "skeleton": "/Game/Characters/SK_Mannequin",
  "source_animation": "/Game/Animations/AS_Attack",
  "slot_name": "UpperBody",
  "path": "/Game/Animations/Montages"
}
```

## Errors

| Condition                    | `isError` | Message                                                       |
| ---------------------------- | --------- | ------------------------------------------------------------- |
| Missing `name`               | `true`    | `Missing required parameter 'name'`                           |
| Missing `skeleton`           | `true`    | `Missing required parameter 'skeleton'`                       |
| Skeleton not found           | `true`    | `Skeleton not found or not a Skeleton: '...'`                 |
| Source animation not found   | `true`    | `Source animation not found or not an AnimSequence: '...'`    |
| Skeleton mismatch            | `true`    | `Source animation skeleton '...' does not match target ...`   |
| Asset already exists         | `true`    | `An asset already exists at '...'`                            |
| Invalid path                 | `true`    | `Invalid path '...'. Must start with /Game or /Engine.`       |
