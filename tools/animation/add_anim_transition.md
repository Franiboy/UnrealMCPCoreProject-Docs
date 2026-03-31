# add_anim_transition

Add a transition between two states in a state machine. Configure crossfade duration, priority, and bidirectional mode.

## Input

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "source_state": "Idle",
  "target_state": "Walk",
  "state_machine_name": "Locomotion",
  "duration": 0.2,
  "priority": 1,
  "bidirectional": false
}
```

| Parameter            | Required | Default | Description                                                      |
| -------------------- | -------- | ------- | ---------------------------------------------------------------- |
| `asset_path`         | **yes**  | ---     | Content path to the Animation Blueprint.                         |
| `source_state`       | **yes**  | ---     | Name of the source state.                                        |
| `target_state`       | **yes**  | ---     | Name of the target state.                                        |
| `state_machine_name` | no       | first   | State machine name. Uses the first state machine if omitted.     |
| `duration`           | no       | `0.2`   | Crossfade duration in seconds.                                   |
| `priority`           | no       | `1`     | Transition priority. Higher values take precedence.              |
| `bidirectional`      | no       | `false` | If true, the transition works in both directions.                |

## Output

| Field                | Type    | Description                              |
| -------------------- | ------- | ---------------------------------------- |
| `asset_path`         | string  | Content path to the Animation Blueprint  |
| `source_state`       | string  | Source state name                        |
| `target_state`       | string  | Target state name                        |
| `crossfade_duration` | number  | Crossfade duration in seconds            |
| `priority`           | number  | Transition priority                      |
| `bidirectional`      | boolean | Whether bidirectional                    |
| `transitions`        | array   | List of all transitions                  |
| `total_transitions`  | number  | Total transition count after addition    |

## Examples

### Add a simple transition

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "source_state": "Idle",
  "target_state": "Walk"
}
```

### Add a bidirectional transition with custom crossfade

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "source_state": "Walk",
  "target_state": "Run",
  "duration": 0.3,
  "bidirectional": true
}
```

## Errors

| Condition               | `isError` | Message                                       |
| ----------------------- | --------- | --------------------------------------------- |
| Missing `asset_path`    | `true`    | `Missing required parameter: asset_path...`   |
| Missing `source_state`  | `true`    | `Missing required parameter: ...source_state` |
| Missing `target_state`  | `true`    | `Missing required parameter: ...target_state` |
| AnimBP not found        | `true`    | `Animation Blueprint not found at '...'`      |
| State machine not found | `true`    | `State machine '...' not found`               |
| Source state not found  | `true`    | `State '...' not found in state machine`      |
| Target state not found  | `true`    | `State '...' not found in state machine`      |
