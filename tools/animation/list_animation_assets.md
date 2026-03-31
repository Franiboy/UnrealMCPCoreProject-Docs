# list_animation_assets

List animation assets in the project: AnimSequences, AnimMontages, BlendSpaces (1D and 2D), and Animation Blueprints. Returns name, asset path, type, skeleton, and type-specific metadata for each asset.

## Input

```json
{
  "path_prefix": "/Game/Animations",
  "name_filter": "Walk",
  "type_filter": "AnimSequence",
  "skeleton_filter": "SK_Mannequin",
  "limit": 50
}
```

| Parameter         | Required | Description                                                                 |
| ----------------- | -------- | --------------------------------------------------------------------------- |
| `path_prefix`     | no       | Only list assets under this content path. Default: `/Game`.                 |
| `name_filter`     | no       | Case-insensitive substring filter on the asset name.                        |
| `type_filter`     | no       | Filter by type: `AnimSequence`, `AnimMontage`, `BlendSpace`, `BlendSpace1D`, `AnimBlueprint`, or `all` (default). |
| `skeleton_filter` | no       | Filter by skeleton name or path (case-insensitive substring match).         |
| `limit`           | no       | Maximum number of assets to return (default: all).                          |

## Output

```json
{
  "count": 3,
  "assets": [
    {
      "name": "Walk_Fwd",
      "asset_path": "/Game/Animations/Walk_Fwd",
      "type": "AnimSequence",
      "skeleton": "SK_Mannequin",
      "skeleton_path": "/Game/Characters/Mannequin/Mesh/SK_Mannequin",
      "duration": 1.033,
      "num_frames": 31,
      "rate_scale": 1.0,
      "num_notifies": 2
    },
    {
      "name": "Attack_Montage",
      "asset_path": "/Game/Animations/Attack_Montage",
      "type": "AnimMontage",
      "skeleton": "SK_Mannequin",
      "skeleton_path": "/Game/Characters/Mannequin/Mesh/SK_Mannequin",
      "duration": 2.0,
      "num_frames": 60,
      "num_sections": 3,
      "num_notifies": 1,
      "slot_name": "DefaultSlot"
    },
    {
      "name": "ABP_Character",
      "asset_path": "/Game/Animations/ABP_Character",
      "type": "AnimBlueprint",
      "skeleton": "SK_Mannequin",
      "skeleton_path": "/Game/Characters/Mannequin/Mesh/SK_Mannequin",
      "parent_class": "AnimInstance"
    }
  ]
}
```

### Common fields (all types)

| Field           | Type   | Description                            |
| --------------- | ------ | -------------------------------------- |
| `name`          | string | Asset name                             |
| `asset_path`    | string | Content path                           |
| `type`          | string | Asset type label                       |
| `skeleton`      | string | Skeleton name (or `"None"`)            |
| `skeleton_path` | string | Skeleton asset path (if available)     |

### AnimSequence-specific fields

| Field          | Type   | Description                  |
| -------------- | ------ | ---------------------------- |
| `duration`     | number | Play length in seconds       |
| `num_frames`   | number | Number of sampled keys       |
| `rate_scale`   | number | Playback rate scale          |
| `num_notifies` | number | Number of animation notifies |

### AnimMontage-specific fields

| Field          | Type   | Description                  |
| -------------- | ------ | ---------------------------- |
| `duration`     | number | Play length in seconds       |
| `num_frames`   | number | Number of sampled keys       |
| `num_sections` | number | Number of montage sections   |
| `num_notifies` | number | Number of animation notifies |
| `slot_name`    | string | First slot track name        |

### BlendSpace / BlendSpace1D-specific fields

| Field         | Type   | Description                          |
| ------------- | ------ | ------------------------------------ |
| `num_samples` | number | Number of animation samples          |
| `blend_type`  | string | `"1D"` or `"2D"`                     |

### AnimBlueprint-specific fields

| Field          | Type   | Description                |
| -------------- | ------ | -------------------------- |
| `parent_class` | string | Parent class name          |

## Examples

### List all AnimSequences

```json
{"type_filter": "AnimSequence"}
```

### List BlendSpaces for a specific skeleton

```json
{"type_filter": "BlendSpace", "skeleton_filter": "SK_Mannequin"}
```

### Search by name

```json
{"name_filter": "Walk", "limit": 10}
```

## Errors

| Condition           | `isError` | Message                                                |
| ------------------- | --------- | ------------------------------------------------------ |
| Invalid type_filter | `true`    | `Unknown type_filter '...'. Valid values: AnimSequence, AnimMontage, BlendSpace, BlendSpace1D, AnimBlueprint, all` |
