# remove_virtual_bone

Remove a virtual bone from a Skeleton asset by name. Child virtual bones are automatically reparented. Returns the removed bone's details and the remaining virtual bones.

## Input

```json
{
  "asset_path": "/Game/Characters/SK_Mannequin",
  "virtual_bone_name": "VirtualBone_hand_r_foot_l"
}
```

| Parameter           | Required | Default | Description                                         |
| ------------------- | -------- | ------- | --------------------------------------------------- |
| `asset_path`        | **yes**  | ---     | Content path to the Skeleton asset.                 |
| `virtual_bone_name` | **yes**  | ---     | Name of the virtual bone to remove (e.g. `"VirtualBone_hand_r_foot_l"`). |

## Output

| Field                      | Type   | Description                              |
| -------------------------- | ------ | ---------------------------------------- |
| `asset_path`               | string | Content path to the Skeleton asset       |
| `removed_virtual_bone`     | string | Name of the removed virtual bone         |
| `source_bone`              | string | Source bone the virtual bone was between  |
| `target_bone`              | string | Target bone the virtual bone was between  |
| `total_virtual_bones`      | number | Total virtual bone count after removal   |
| `remaining_virtual_bones`  | array  | List of remaining virtual bones          |

## Examples

### Remove a virtual bone by name

```json
{
  "asset_path": "/Game/Characters/SK_Mannequin",
  "virtual_bone_name": "VirtualBone_hand_r_foot_l"
}
```

## Errors

| Condition                  | `isError` | Message                                            |
| -------------------------- | --------- | -------------------------------------------------- |
| Missing required parameter | `true`    | `Missing required parameter: asset_path...`        |
| Skeleton not found         | `true`    | `Skeleton not found at '...'`                      |
| Virtual bone not found     | `true`    | `Virtual bone '...' not found in skeleton '...'`   |
