# get_animation_info

Get detailed information about a single animation asset. Works with AnimSequences, AnimMontages, BlendSpaces (1D/2D), and Animation Blueprints. Returns skeleton, timing, notifies, curves, and type-specific metadata.

## Input

```json
{
  "asset_path": "/Game/Animations/Walk_Fwd"
}
```

| Parameter    | Required | Description                                         |
| ------------ | -------- | --------------------------------------------------- |
| `asset_path` | **yes**  | Content path to the animation asset to inspect.     |

## Output

The output varies by asset type. All types share these common fields:

| Field        | Type   | Description          |
| ------------ | ------ | -------------------- |
| `name`       | string | Asset name           |
| `asset_path` | string | Full content path    |
| `type`       | string | Asset type label     |
| `skeleton`   | string | Skeleton name        |
| `skeleton_path` | string | Skeleton asset path (if available) |

### AnimSequence

```json
{
  "name": "Walk_Fwd",
  "asset_path": "/Game/Animations/Walk_Fwd.Walk_Fwd",
  "type": "AnimSequence",
  "skeleton": "SK_Mannequin",
  "skeleton_path": "/Game/Characters/SK_Mannequin.SK_Mannequin",
  "duration": 1.033,
  "num_frames": 31,
  "rate_scale": 1.0,
  "sampling_rate": 30.0,
  "additive_anim_type": "None",
  "interpolation": "Linear",
  "num_notifies": 2,
  "notifies": [
    {"name": "FootstepL", "time": 0.3, "trigger_time_offset": 0.0, "class": "AnimNotify_PlaySound", "notify_type": "Notify"},
    {"name": "FootstepR", "time": 0.8, "trigger_time_offset": 0.0, "class": "AnimNotify_PlaySound", "notify_type": "Notify"}
  ],
  "num_curves": 1,
  "curves": [
    {"name": "Speed", "type": "float", "num_keys": 5}
  ]
}
```

| Field               | Type   | Description                                    |
| ------------------- | ------ | ---------------------------------------------- |
| `duration`          | number | Play length in seconds                         |
| `num_frames`        | number | Number of sampled keys                         |
| `rate_scale`        | number | Playback rate scale                            |
| `sampling_rate`     | number | Sampling frame rate (e.g. 30.0)                |
| `additive_anim_type`| string | `None`, `LocalSpace`, or `MeshSpace`           |
| `interpolation`     | string | `Linear` or `Step`                             |
| `notifies`          | array  | Animation notifies with name, time, class      |
| `curves`            | array  | Animation curves with name, type, key count    |

### AnimMontage

Includes all AnimSequence fields plus:

| Field            | Type   | Description                              |
| ---------------- | ------ | ---------------------------------------- |
| `slot_tracks`    | array  | Slot tracks with `slot_name`             |
| `sections`       | array  | Montage sections (name, start_time, next_section, index) |
| `num_sections`   | number | Number of sections                       |
| `blend_in_time`  | number | Blend-in duration in seconds             |
| `blend_out_time` | number | Blend-out duration in seconds            |

### BlendSpace / BlendSpace1D

| Field        | Type   | Description                                   |
| ------------ | ------ | --------------------------------------------- |
| `blend_type` | string | `1D` or `2D`                                  |
| `axes`       | array  | Axis definitions (name, min, max, grid_divisions, axis) |
| `samples`    | array  | Blend samples (index, x, y, animation, animation_path, rate_scale) |
| `num_samples`| number | Number of samples                             |

### AnimBlueprint

| Field          | Type   | Description                          |
| -------------- | ------ | ------------------------------------ |
| `parent_class` | string | Parent class name                    |
| `graphs`       | array  | Graphs (name, graph_type, num_nodes) |
| `num_graphs`   | number | Number of graphs                     |
| `variables`    | array  | Variables (name, type, category)     |
| `num_variables`| number | Number of variables                  |

## Examples

### Inspect an AnimSequence

```json
{"asset_path": "/Game/Animations/Walk_Fwd"}
```

### Inspect a Montage

```json
{"asset_path": "/Game/Animations/Attack_Montage"}
```

### Inspect a BlendSpace

```json
{"asset_path": "/Game/Animations/BS_Locomotion"}
```

## Errors

| Condition               | `isError` | Message                                                |
| ----------------------- | --------- | ------------------------------------------------------ |
| Missing `asset_path`    | `true`    | `Missing required parameter 'asset_path'`              |
| Asset not found         | `true`    | `Animation asset not found: '...'`                     |
| Not an animation type   | `true`    | `Asset '...' is a ..., not a supported animation type` |
