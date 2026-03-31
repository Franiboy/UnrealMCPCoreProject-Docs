# get_niagara_system_info

Get detailed information about a Niagara System asset including its emitters, exposed parameters, bounds, warmup settings, and system scripts.

## Input

```json
{
  "asset_path": "/Game/FX/NS_Fire",
  "include_emitters": true,
  "include_parameters": true
}
```

| Parameter            | Required | Description                                              |
| -------------------- | -------- | -------------------------------------------------------- |
| `asset_path`         | **yes**  | Asset path of the Niagara System                         |
| `include_emitters`   | no       | Include emitter details (default: `true`)                |
| `include_parameters` | no       | Include exposed parameters (default: `true`)             |

## Output

```json
{
  "name": "NS_Fire",
  "path": "/Game/FX/NS_Fire.NS_Fire",
  "is_valid": true,
  "is_ready_to_run": true,
  "emitter_count": 3,
  "warmup_time": 0.0,
  "fixed_bounds": false,
  "effect_type": "Default",
  "emitters": [
    {
      "name": "Flames",
      "id": "XXXXXXXX-...",
      "enabled": true,
      "index": 0,
      "sim_target": "GPU",
      "local_space": false,
      "renderer_count": 1,
      "event_handler_count": 0,
      "simulation_stage_count": 0
    }
  ],
  "exposed_parameters": [
    {
      "name": "User.Color",
      "type": "NiagaraFloat",
      "is_data_interface": false,
      "is_uobject": false
    }
  ]
}
```

| Field                           | Type    | Description                                          |
| ------------------------------- | ------- | ---------------------------------------------------- |
| `name`                          | string  | System asset name                                    |
| `path`                          | string  | Full asset path                                      |
| `is_valid`                      | boolean | Whether the system is valid                          |
| `is_ready_to_run`              | boolean | Whether the system is ready to execute               |
| `emitter_count`                 | number  | Number of emitters in the system                     |
| `warmup_time`                   | number  | Warmup time in seconds                               |
| `fixed_bounds`                  | boolean | Whether fixed bounds are enabled                     |
| `bounds`                        | object  | Fixed bounds min/max (only when `fixed_bounds` is true) |
| `effect_type`                   | string  | Effect type name (if assigned)                       |
| `emitters`                      | array   | Emitter summaries (when `include_emitters` is true)  |
| `emitters[].name`               | string  | Emitter name within the system                       |
| `emitters[].id`                 | string  | Emitter GUID                                         |
| `emitters[].enabled`            | boolean | Whether the emitter is enabled                       |
| `emitters[].index`              | number  | Emitter index in the system                          |
| `emitters[].sim_target`         | string  | Simulation target: `CPU` or `GPU`                    |
| `emitters[].local_space`        | boolean | Whether the emitter uses local space                 |
| `emitters[].renderer_count`     | number  | Number of renderers on the emitter                   |
| `emitters[].event_handler_count`| number  | Number of event handlers                             |
| `emitters[].simulation_stage_count` | number | Number of simulation stages                      |
| `exposed_parameters`            | array   | User-exposed parameters (when `include_parameters` is true) |
| `exposed_parameters[].name`     | string  | Parameter name                                       |
| `exposed_parameters[].type`     | string  | Parameter type                                       |
| `exposed_parameters[].is_data_interface` | boolean | Whether it's a data interface parameter     |
| `exposed_parameters[].is_uobject` | boolean | Whether it's a UObject parameter                  |

## Examples

### Get full system info

```json
{"asset_path": "/Game/FX/NS_Fire"}
```

### Get system info without parameters

```json
{"asset_path": "/Game/FX/NS_Fire", "include_parameters": false}
```

## Errors

| Condition            | `isError` | Message                                   |
| -------------------- | --------- | ----------------------------------------- |
| Missing/empty path   | `true`    | `asset_path is required`                  |
| System not found     | `true`    | `Niagara System not found: <path>`        |
