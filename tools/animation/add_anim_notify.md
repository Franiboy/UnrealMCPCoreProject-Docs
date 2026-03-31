# add_anim_notify

Add a notify event to an AnimSequence or AnimMontage at a specified time. Creates either a custom named notify or an instance of a typed UAnimNotify / UAnimNotifyState class. For notify states, specify a duration.

## Input

```json
{
  "asset_path": "/Game/Animations/AS_Walk",
  "notify_name": "FootStep",
  "time": 0.5,
  "notify_class": "AnimNotify_PlaySound",
  "duration": 0.0,
  "track_index": 0
}
```

| Parameter      | Required | Default | Description                                                            |
| -------------- | -------- | ------- | ---------------------------------------------------------------------- |
| `asset_path`   | **yes**  | —       | Content path to the AnimSequence or AnimMontage.                       |
| `notify_name`  | **yes**  | —       | Name for the notify event (e.g. `"FootStep"`, `"Attack_Start"`).      |
| `time`         | **yes**  | —       | Time in seconds where the notify triggers (within animation length).   |
| `notify_class` | no       | —       | UAnimNotify or UAnimNotifyState class name. Omit for custom notifies.  |
| `duration`     | no       | `0`     | Duration in seconds (only for UAnimNotifyState subclasses).            |
| `track_index`  | no       | `0`     | Notify track index.                                                    |

## Output

| Field            | Type   | Description                                    |
| ---------------- | ------ | ---------------------------------------------- |
| `asset_path`     | string | Full content path of the animation asset       |
| `notify_name`    | string | Name of the added notify                       |
| `time`           | number | Trigger time in seconds                        |
| `notify_type`    | string | `"Custom"`, `"Notify"`, or `"NotifyState"`     |
| `index`          | number | Index in the notifies array                    |
| `total_notifies` | number | Total notify count after addition              |
| `duration`       | number | Duration (if notify state, omitted otherwise)  |

## Examples

### Add a custom named notify

```json
{
  "asset_path": "/Game/Animations/AS_Walk",
  "notify_name": "FootStep",
  "time": 0.3
}
```

### Add a typed notify with class

```json
{
  "asset_path": "/Game/Animations/AS_Attack",
  "notify_name": "PlaySound",
  "time": 0.5,
  "notify_class": "AnimNotify_PlaySound"
}
```

### Add a notify state with duration

```json
{
  "asset_path": "/Game/Animations/AS_Attack",
  "notify_name": "Trail",
  "time": 0.2,
  "notify_class": "AnimNotifyState_Trail",
  "duration": 0.5
}
```

## Errors

| Condition                | `isError` | Message                                              |
| ------------------------ | --------- | ---------------------------------------------------- |
| Missing `asset_path`     | `true`    | `Missing required parameter 'asset_path'`            |
| Missing `notify_name`    | `true`    | `Missing required parameter 'notify_name'`           |
| Missing `time`           | `true`    | `Missing required parameter 'time'`                  |
| Asset not found          | `true`    | `Animation asset not found: '...'`                   |
| Not an animation asset   | `true`    | `Asset '...' is not an AnimSequence or AnimMontage`  |
| Time out of range        | `true`    | `Time ... is outside animation range [0, ...]`       |
| Notify class not found   | `true`    | `Notify class '...' not found`                       |
| Invalid notify class     | `true`    | `Class '...' is not a subclass of UAnimNotify or...` |
