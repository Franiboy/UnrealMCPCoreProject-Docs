# create_control_rig

Create a new Control Rig Blueprint asset, optionally initialized from a Skeleton.

## Input

```json
{
  "name": "CR_MyCharacter",
  "path": "/Game/Rigs",
  "skeleton": "/Game/Characters/Mannequin/Skeleton.Skeleton"
}
```

|| Parameter  | Required | Default    | Description                                             |
|| ---------- | -------- | ---------- | ------------------------------------------------------- |
|| `name`     | **yes**  | ---        | Asset name for the new Control Rig.                     |
|| `path`     | no       | `"/Game"`  | Content folder path.                                    |
|| `skeleton` | no       | ---        | Skeleton or SkeletalMesh asset path to import bones from. |

## Output

|| Field           | Type   | Description                         |
|| --------------- | ------ | ----------------------------------- |
|| `name`          | string | Name of the created Control Rig     |
|| `asset_path`    | string | Full content path to the asset      |
|| `element_count` | number | Number of hierarchy elements        |

## Examples

### Create empty Control Rig

```json
{
  "name": "CR_Simple"
}
```

### Create from skeleton

```json
{
  "name": "CR_Mannequin",
  "skeleton": "/Game/Characters/Mannequin/Skeleton.Skeleton"
}
```

## Errors

|| Condition               | `isError` | Message                            |
|| ----------------------- | --------- | ---------------------------------- |
|| Missing `name`          | true      | 'name' is required                 |
|| Asset already exists    | true      | Asset already exists: ...          |
|| Skeleton not found      | true      | Skeleton or SkeletalMesh not found: ... |
|| Creation failed         | true      | Failed to create Control Rig Blueprint |
