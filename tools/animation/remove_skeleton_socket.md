# remove_skeleton_socket

Remove a socket from a Skeleton asset by name. Returns the removed socket's details and the remaining sockets list.

## Input

```json
{
  "asset_path": "/Game/Characters/SK_Mannequin",
  "socket_name": "WeaponSocket"
}
```

| Parameter     | Required | Default | Description                         |
| ------------- | -------- | ------- | ----------------------------------- |
| `asset_path`  | **yes**  | ---     | Content path to the Skeleton asset. |
| `socket_name` | **yes**  | ---     | Name of the socket to remove.       |

## Output

| Field               | Type   | Description                            |
| ------------------- | ------ | -------------------------------------- |
| `asset_path`        | string | Content path to the Skeleton asset     |
| `removed_socket`    | string | Name of the removed socket             |
| `bone_name`         | string | Bone the socket was attached to        |
| `total_sockets`     | number | Total socket count after removal       |
| `remaining_sockets` | array  | List of remaining sockets              |

## Examples

### Remove a socket by name

```json
{
  "asset_path": "/Game/Characters/SK_Mannequin",
  "socket_name": "WeaponSocket"
}
```

## Errors

| Condition                  | `isError` | Message                                        |
| -------------------------- | --------- | ---------------------------------------------- |
| Missing required parameter | `true`    | `Missing required parameter: asset_path...`    |
| Skeleton not found         | `true`    | `Skeleton not found at '...'`                  |
| Socket not found           | `true`    | `Socket '...' not found on skeleton '...'`     |
