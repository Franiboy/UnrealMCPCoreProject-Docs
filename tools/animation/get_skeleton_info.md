# get_skeleton_info

Get detailed information about a Skeleton asset. Returns the bone hierarchy (with reference pose transforms), sockets, virtual bones, compatible skeletons, animation slot groups, curve names, blend profiles, animation notifies, sync markers, and retarget sources.

## Input

```json
{
  "asset_path": "/Game/Characters/SK_Mannequin",
  "include_bones": true
}
```

| Parameter       | Required | Default | Description                                                  |
| --------------- | -------- | ------- | ------------------------------------------------------------ |
| `asset_path`    | **yes**  | —       | Content path to the Skeleton asset to inspect.               |
| `include_bones` | no       | `true`  | Include the full bone hierarchy with transforms. Set `false` for a lighter summary. |

## Output

### Top-Level Fields

| Field                      | Type   | Description                                  |
| -------------------------- | ------ | -------------------------------------------- |
| `name`                     | string | Skeleton name                                |
| `asset_path`               | string | Full content path                            |
| `num_bones`                | number | Real bone count (excluding virtual)          |
| `num_virtual_bones`        | number | Virtual bone count                           |
| `num_total_bones`          | number | Total (real + virtual)                       |
| `bones`                    | array  | Bone objects (only if `include_bones=true`)  |
| `sockets`                  | array  | Socket objects                               |
| `num_sockets`              | number | Number of sockets                            |
| `virtual_bones`            | array  | Virtual bone objects (if any)                |
| `compatible_skeletons`     | array  | Compatible skeleton references (if any)      |
| `num_compatible_skeletons` | number | Number of compatible skeletons               |
| `slot_groups`              | array  | Animation slot groups (if any)               |
| `num_slot_groups`          | number | Number of slot groups                        |
| `curves`                   | array  | Curve names (if any)                         |
| `num_curves`               | number | Number of curves                             |
| `blend_profiles`           | array  | Blend profile names (if any)                 |
| `num_blend_profiles`       | number | Number of blend profiles                     |
| `animation_notifies`       | array  | Animation notify names (if any)              |
| `num_animation_notifies`   | number | Number of animation notifies                 |
| `sync_markers`             | array  | Sync marker names (if any)                   |
| `num_sync_markers`         | number | Number of sync markers                       |
| `retarget_sources`         | array  | Retarget source names (if any)               |
| `num_retarget_sources`     | number | Number of retarget sources                   |

### Bone Object

| Field          | Type   | Description                                  |
| -------------- | ------ | -------------------------------------------- |
| `index`        | number | Bone index in the skeleton                   |
| `name`         | string | Bone name                                    |
| `parent_index` | number | Parent bone index (`-1` for root)            |
| `parent_name`  | string | Parent bone name (absent for root)           |
| `num_children` | number | Number of direct child bones                 |
| `ref_pose`     | object | Reference pose transform (see below)         |

### Bone ref_pose Object

| Field       | Type   | Description          |
| ----------- | ------ | -------------------- |
| `loc_x`     | number | Location X           |
| `loc_y`     | number | Location Y           |
| `loc_z`     | number | Location Z           |
| `rot_pitch` | number | Rotation pitch       |
| `rot_yaw`   | number | Rotation yaw         |
| `rot_roll`  | number | Rotation roll        |
| `scale_x`   | number | Scale X              |
| `scale_y`   | number | Scale Y              |
| `scale_z`   | number | Scale Z              |

### Socket Object

| Field                    | Type    | Description                        |
| ------------------------ | ------- | ---------------------------------- |
| `name`                   | string  | Socket name                        |
| `bone_name`              | string  | Attached bone name                 |
| `loc_x/y/z`             | number  | Relative location                  |
| `rot_pitch/yaw/roll`    | number  | Relative rotation                  |
| `scale_x/y/z`           | number  | Relative scale                     |
| `force_always_animated`  | boolean | Force bone to animate even if LOD would skip it |

### Virtual Bone Object

| Field         | Type   | Description          |
| ------------- | ------ | -------------------- |
| `name`        | string | Virtual bone name    |
| `source_bone` | string | Source bone name     |
| `target_bone` | string | Target bone name     |

### Compatible Skeleton Object

| Field  | Type   | Description                |
| ------ | ------ | -------------------------- |
| `path` | string | Soft object path           |
| `name` | string | Skeleton name (if loaded)  |

### Slot Group Object

| Field        | Type   | Description                |
| ------------ | ------ | -------------------------- |
| `group_name` | string | Slot group name            |
| `slot_names` | array  | Array of slot name strings |
| `num_slots`  | number | Number of slots in group   |

## Examples

### Full skeleton inspection

```json
{"asset_path": "/Game/Characters/SK_Mannequin"}
```

### Summary only (no bone hierarchy)

```json
{"asset_path": "/Game/Characters/SK_Mannequin", "include_bones": false}
```

## Errors

| Condition               | `isError` | Message                                          |
| ----------------------- | --------- | ------------------------------------------------ |
| Missing `asset_path`    | `true`    | `Missing required parameter 'asset_path'`        |
| Asset not found         | `true`    | `Asset '...' is not a Skeleton (not found)`      |
| Not a Skeleton          | `true`    | `Asset '...' is not a Skeleton (found ...)`      |
