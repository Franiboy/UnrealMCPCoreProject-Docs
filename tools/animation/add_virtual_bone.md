# add_virtual_bone

Add a virtual bone to a Skeleton asset between two existing bones. Virtual bones are non-physical bones computed as the transform between a source and target bone. They are useful for IK targets, aim offsets, and constraints. The name is auto-generated with a `VirtualBone_` prefix.

## Input

```json
{
  "asset_path": "/Game/Characters/SK_Mannequin",
  "source_bone": "hand_r",
  "target_bone": "foot_l"
}
```

| Parameter     | Required | Default | Description                                    |
| ------------- | -------- | ------- | ---------------------------------------------- |
| `asset_path`  | **yes**  | ---     | Content path to the Skeleton asset.            |
| `source_bone` | **yes**  | ---     | Name of the source bone (must exist).          |
| `target_bone` | **yes**  | ---     | Name of the target bone (must exist).          |

## Output

| Field                | Type   | Description                              |
| -------------------- | ------ | ---------------------------------------- |
| `asset_path`         | string | Content path to the Skeleton asset       |
| `virtual_bone_name`  | string | Auto-generated name of the virtual bone  |
| `source_bone`        | string | Source bone name                         |
| `target_bone`        | string | Target bone name                         |
| `total_virtual_bones`| number | Total virtual bone count after addition  |
| `virtual_bones`      | array  | List of all virtual bones on the skeleton|

## Examples

### Add a virtual bone for IK targeting

```json
{
  "asset_path": "/Game/Characters/SK_Mannequin",
  "source_bone": "hand_r",
  "target_bone": "foot_l"
}
```

### Add a virtual bone between spine and head

```json
{
  "asset_path": "/Game/Characters/SK_Mannequin",
  "source_bone": "spine_01",
  "target_bone": "head"
}
```

## Errors

| Condition                  | `isError` | Message                                                     |
| -------------------------- | --------- | ----------------------------------------------------------- |
| Missing required parameter | `true`    | `Missing required parameter: asset_path...`                 |
| Skeleton not found         | `true`    | `Skeleton not found at '...'`                               |
| Source bone not found      | `true`    | `Source bone '...' not found in skeleton '...'`             |
| Target bone not found      | `true`    | `Target bone '...' not found in skeleton '...'`             |
| Duplicate virtual bone     | `true`    | `Failed to add virtual bone ... may already exist`          |
