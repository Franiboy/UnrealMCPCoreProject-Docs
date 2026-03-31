# get_niagara_emitter_info

Inspect a Niagara Emitter: renderers, simulation target (CPU/GPU), spawn/update scripts, event handlers, simulation stages, bounds, and emitter properties.

Accepts either a Niagara System path (with `emitter_name`) or a standalone Emitter asset path. For single-emitter systems, `emitter_name` is auto-selected.

## Input

```json
{
  "asset_path": "/Game/FX/NS_Fire",
  "emitter_name": "Flames"
}
```

| Parameter      | Required | Description                                                          |
| -------------- | -------- | -------------------------------------------------------------------- |
| `asset_path`   | **yes**  | Asset path of a Niagara System or standalone Emitter                 |
| `emitter_name` | varies   | Emitter name within the system (required for multi-emitter systems)  |

## Output

```json
{
  "emitter_name": "Flames",
  "source_asset": "/Game/FX/NS_Fire",
  "system_name": "NS_Fire",
  "sim_target": "GPU",
  "local_space": false,
  "determinism": false,
  "interpolated_spawning": false,
  "requires_persistent_ids": false,
  "allocation_mode": "AutomaticEstimate",
  "bounds_mode": "Dynamic",
  "renderers": [
    {
      "type": "SpriteRenderer",
      "class": "NiagaraSpriteRendererProperties",
      "enabled": true
    }
  ],
  "event_handlers": [],
  "simulation_stages": []
}
```

| Field                       | Type    | Description                                              |
| --------------------------- | ------- | -------------------------------------------------------- |
| `emitter_name`              | string  | Resolved emitter name                                    |
| `source_asset`              | string  | Original asset path provided                             |
| `system_name`               | string  | Parent system name (if from a system)                    |
| `sim_target`                | string  | Simulation target: `CPU` or `GPU`                        |
| `local_space`               | boolean | Whether the emitter uses local space                     |
| `determinism`               | boolean | Whether deterministic simulation is enabled              |
| `random_seed`               | number  | Random seed (only when determinism is true)              |
| `interpolated_spawning`     | boolean | Whether interpolated spawning is enabled                 |
| `requires_persistent_ids`   | boolean | Whether persistent particle IDs are required             |
| `allocation_mode`           | string  | `AutomaticEstimate`, `ManualEstimate`, or `FixedCount`   |
| `pre_allocation_count`      | number  | Pre-allocated particle count (non-automatic modes)       |
| `bounds_mode`               | string  | `Dynamic`, `Fixed`, or `Programmable`                    |
| `fixed_bounds`              | object  | Fixed bounds min/max (only when bounds_mode is Fixed)    |
| `renderers`                 | array   | Renderer list with type, class, and enabled status       |
| `renderers[].type`          | string  | Renderer type (e.g. `SpriteRenderer`, `MeshRenderer`)   |
| `renderers[].class`         | string  | Full renderer class name                                 |
| `renderers[].enabled`       | boolean | Whether the renderer is enabled                          |
| `event_handlers`            | array   | Event handler scripts                                    |
| `event_handlers[].source_event_name` | string | Source event name                               |
| `event_handlers[].execution_mode`    | string | `EveryParticle`, `SpawnedParticles`, `SingleParticle` |
| `event_handlers[].max_events_per_frame` | number | Max events per frame                          |
| `simulation_stages`         | array   | Additional simulation stages                             |
| `simulation_stages[].name`  | string  | Stage name                                               |
| `simulation_stages[].enabled` | boolean | Whether the stage is enabled                           |
| `simulation_stages[].class` | string  | Stage class name                                         |

## Examples

### Inspect emitter in a system

```json
{"asset_path": "/Game/FX/NS_Fire", "emitter_name": "Flames"}
```

### Auto-select from single-emitter system

```json
{"asset_path": "/Game/FX/NS_SimpleEffect"}
```

### Inspect standalone emitter asset

```json
{"asset_path": "/Game/FX/NE_CustomEmitter"}
```

## Errors

| Condition                        | `isError` | Message                                                      |
| -------------------------------- | --------- | ------------------------------------------------------------ |
| Missing/empty path               | `true`    | `asset_path is required`                                     |
| Asset not found                  | `true`    | `Niagara System or Emitter not found: <path>`                |
| Multi-emitter without name       | `true`    | `emitter_name is required for systems with multiple emitters`|
| Emitter not found in system      | `true`    | `Emitter '<name>' not found in system '<system>'`            |
| No emitter data                  | `true`    | `No emitter data available for: <path>`                      |
