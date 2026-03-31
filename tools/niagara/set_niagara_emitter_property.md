# set_niagara_emitter_property

Set properties on an emitter within a Niagara System.

## Input

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "emitter_name": "SimpleSpriteBurst",
  "sim_target": "GPU",
  "local_space": true,
  "determinism": true,
  "random_seed": 42
}
```

| Parameter                    | Required | Description                                                           |
| ---------------------------- | -------- | --------------------------------------------------------------------- |
| `system_path`                | **yes**  | Asset path of the Niagara System                                      |
| `emitter_name`               | no       | Emitter name (auto-selected if the system has only one emitter)       |
| `sim_target`                 | no       | Simulation target: `CPU` or `GPU`                                     |
| `local_space`                | no       | Use local-space simulation (boolean)                                  |
| `determinism`                | no       | Enable deterministic simulation (boolean)                             |
| `random_seed`                | no       | Random seed for deterministic simulation (integer)                    |
| `interpolated_spawning`      | no       | Enable interpolated spawning (boolean)                                |
| `requires_persistent_ids`    | no       | Require persistent particle IDs (boolean)                             |
| `allocation_mode`            | no       | `AutomaticEstimate`, `ManualEstimate`, or `FixedCount`                |
| `pre_allocation_count`       | no       | Pre-allocation particle count (integer)                               |
| `bounds_mode`                | no       | Bounds calculation: `Dynamic`, `Fixed`, or `Programmable`             |
| `fixed_bounds`               | no       | Fixed bounds: `{"min":{"x","y","z"}, "max":{"x","y","z"}}`           |
| `max_gpu_particles_per_frame`| no       | Max GPU particles spawned per frame (integer)                         |

At least one property (besides `system_path` and `emitter_name`) must be provided.

## Output

```json
{
  "system": "NS_Fire",
  "emitter_name": "SimpleSpriteBurst",
  "properties_set": 4,
  "updated": {
    "sim_target": "GPU",
    "local_space": true,
    "determinism": true,
    "random_seed": 42
  }
}
```

| Field            | Type   | Description                          |
| ---------------- | ------ | ------------------------------------ |
| `system`         | string | System asset name                    |
| `emitter_name`   | string | Name of the modified emitter         |
| `properties_set` | number | Number of properties that were set   |
| `updated`        | object | Key-value pairs of updated properties|

## Examples

### Set simulation target to GPU

```json
{"system_path": "/Game/FX/NS_Fire", "emitter_name": "Sparks", "sim_target": "GPU"}
```

### Set fixed bounds

```json
{
  "system_path": "/Game/FX/NS_Fire",
  "emitter_name": "Sparks",
  "bounds_mode": "Fixed",
  "fixed_bounds": {
    "min": {"x": -500, "y": -500, "z": -500},
    "max": {"x": 500, "y": 500, "z": 500}
  }
}
```

## Errors

| Condition              | Message                                              |
| ---------------------- | ---------------------------------------------------- |
| Missing `system_path`  | system_path is required                              |
| System not found       | System not found: ...                                |
| Emitter not found      | Emitter '...' not found. Available: ...              |
| Ambiguous emitter      | emitter_name required for systems with N emitters    |
| No properties given    | No valid properties provided. Supported: ...         |
