# remove_anim_transition

Remove a transition between two states in a state machine. Identifies the transition by source and target state names (first match).

## Input

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "source_state": "Idle",
  "target_state": "Walk",
  "state_machine_name": "Locomotion"
}
```

| Parameter            | Required | Default | Description                                                  |
| -------------------- | -------- | ------- | ------------------------------------------------------------ |
| `asset_path`         | **yes**  | ---     | Content path to the Animation Blueprint.                     |
| `source_state`       | **yes**  | ---     | Name of the source state of the transition to remove.        |
| `target_state`       | **yes**  | ---     | Name of the target state of the transition to remove.        |
| `state_machine_name` | no       | first   | State machine name. Uses the first state machine if omitted. |

## Output

| Field                    | Type   | Description                              |
| ------------------------ | ------ | ---------------------------------------- |
| `asset_path`             | string | Content path to the Animation Blueprint  |
| `removed_source`         | string | Source state of the removed transition   |
| `removed_target`         | string | Target state of the removed transition   |
| `remaining_transitions`  | array  | List of remaining transitions            |
| `total_transitions`      | number | Total transition count after removal     |

## Examples

### Remove a transition between two states

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "source_state": "Idle",
  "target_state": "Walk"
}
```

### Remove from a specific state machine

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "source_state": "Walk",
  "target_state": "Run",
  "state_machine_name": "Locomotion"
}
```

## Errors

| Condition               | `isError` | Message                                        |
| ----------------------- | --------- | ---------------------------------------------- |
| Missing `asset_path`    | `true`    | `Missing required parameter: asset_path...`    |
| Missing `source_state`  | `true`    | `Missing required parameter: ...source_state`  |
| Missing `target_state`  | `true`    | `Missing required parameter: ...target_state`  |
| AnimBP not found        | `true`    | `Animation Blueprint not found at '...'`       |
| State machine not found | `true`    | `State machine '...' not found`                |
| Transition not found    | `true`    | `Transition '...' -> '...' not found`          |
