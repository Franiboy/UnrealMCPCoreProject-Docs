# add_skeleton_socket

Add a socket to a bone in a Skeleton asset. Sockets are attachment points used to attach meshes, effects, or other objects to specific bone locations. Specify optional position, rotation, and scale offsets relative to the bone.

## Input

```json
{
  "asset_path": "/Game/Characters/SK_Mannequin",
  "bone_name": "hand_r",
  "socket_name": "WeaponSocket",
  "location_x": 0,
  "location_y": 5.0,
  "location_z": 0,
  "rotation_pitch": 0,
  "rotation_yaw": 90,
  "rotation_roll": 0,
  "scale_x": 1,
  "scale_y": 1,
  "scale_z": 1
}
```

| Parameter        | Required | Default | Description                                                      |
| ---------------- | -------- | ------- | ---------------------------------------------------------------- |
| `asset_path`     | **yes**  | ---     | Content path to the Skeleton asset.                              |
| `bone_name`      | **yes**  | ---     | Name of the bone to attach the socket to. Must exist in skeleton.|
| `socket_name`    | **yes**  | ---     | Unique name for the new socket (e.g. `"WeaponSocket"`).          |
| `location_x`     | no       | `0`     | Relative location X offset.                                      |
| `location_y`     | no       | `0`     | Relative location Y offset.                                      |
| `location_z`     | no       | `0`     | Relative location Z offset.                                      |
| `rotation_pitch` | no       | `0`     | Relative rotation pitch in degrees.                              |
| `rotation_yaw`   | no       | `0`     | Relative rotation yaw in degrees.                                |
| `rotation_roll`  | no       | `0`     | Relative rotation roll in degrees.                               |
| `scale_x`        | no       | `1`     | Relative scale X.                                                |
| `scale_y`        | no       | `1`     | Relative scale Y.                                                |
| `scale_z`        | no       | `1`     | Relative scale Z.                                                |

## Output

| Field           | Type   | Description                              |
| --------------- | ------ | ---------------------------------------- |
| `asset_path`    | string | Content path to the Skeleton asset       |
| `socket_name`   | string | Name of the created socket               |
| `bone_name`     | string | Bone the socket is attached to           |
| `loc_x/y/z`     | number | Relative location offsets                |
| `rot_pitch/yaw/roll` | number | Relative rotation offsets           |
| `scale_x/y/z`   | number | Relative scale values                    |
| `total_sockets` | number | Total socket count after addition        |
| `sockets`       | array  | List of all sockets on the skeleton      |

## Examples

### Add a weapon socket with offset

```json
{
  "asset_path": "/Game/Characters/SK_Mannequin",
  "bone_name": "hand_r",
  "socket_name": "WeaponSocket",
  "location_y": 5.0,
  "rotation_yaw": 90
}
```

### Add a socket with default transform

```json
{
  "asset_path": "/Game/Characters/SK_Mannequin",
  "bone_name": "head",
  "socket_name": "HatSocket"
}
```

## Errors

| Condition                  | `isError` | Message                                          |
| -------------------------- | --------- | ------------------------------------------------ |
| Missing required parameter | `true`    | `Missing required parameter: asset_path...`      |
| Skeleton not found         | `true`    | `Skeleton not found at '...'`                    |
| Bone not found             | `true`    | `Bone '...' not found in skeleton '...'`         |
| Duplicate socket name      | `true`    | `Socket '...' already exists on skeleton '...'`  |
