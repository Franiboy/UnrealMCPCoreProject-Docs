# remove_anim_notify

Remove a notify event from an AnimSequence or AnimMontage. Identify the notify by name (first match) or by index.

## Input

```json
{
  "asset_path": "/Game/Animations/AS_Walk",
  "notify_name": "FootStep"
}
```

| Parameter     | Required | Default | Description                                                              |
| ------------- | -------- | ------- | ------------------------------------------------------------------------ |
| `asset_path`  | **yes**  | —       | Content path to the AnimSequence or AnimMontage.                         |
| `notify_name` | no       | —       | Name of the notify to remove (first match). Provide this or `index`.     |
| `index`       | no       | —       | Index of the notify to remove (0-based). Provide this or `notify_name`.  |

At least one of `notify_name` or `index` must be provided.

## Output

| Field                | Type   | Description                                |
| -------------------- | ------ | ------------------------------------------ |
| `asset_path`         | string | Full content path of the animation asset   |
| `removed_notify`     | string | Name of the removed notify                 |
| `removed_time`       | number | Trigger time of the removed notify         |
| `removed_index`      | number | Index of the removed notify                |
| `remaining_notifies` | number | Remaining notify count after removal       |

## Examples

### Remove by name

```json
{
  "asset_path": "/Game/Animations/AS_Walk",
  "notify_name": "FootStep"
}
```

### Remove by index

```json
{
  "asset_path": "/Game/Animations/AS_Walk",
  "index": 0
}
```

## Errors

| Condition                | `isError` | Message                                              |
| ------------------------ | --------- | ---------------------------------------------------- |
| Missing `asset_path`     | `true`    | `Missing required parameter 'asset_path'`            |
| No identifier provided   | `true`    | `Must provide 'notify_name' or 'index' to ...`      |
| Asset not found          | `true`    | `Animation asset not found: '...'`                   |
| Not an animation asset   | `true`    | `Asset '...' is not an AnimSequence or AnimMontage`  |
| Notify not found by name | `true`    | `Notify '...' not found on asset '...'`              |
| Index out of range       | `true`    | `Notify index ... is out of range [0, ...)`          |
