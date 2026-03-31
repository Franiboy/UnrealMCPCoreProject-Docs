# remove_anim_state

Remove a state from a state machine in an Animation Blueprint. All transitions connected to the state are automatically removed.

## Input

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "state_name": "Idle",
  "state_machine_name": "Locomotion"
}
```

| Parameter            | Required | Default | Description                                                  |
| -------------------- | -------- | ------- | ------------------------------------------------------------ |
| `asset_path`         | **yes**  | ---     | Content path to the Animation Blueprint.                     |
| `state_name`         | **yes**  | ---     | Name of the state to remove.                                 |
| `state_machine_name` | no       | first   | State machine name. Uses the first state machine if omitted. |

## Output

| Field                | Type   | Description                              |
| -------------------- | ------ | ---------------------------------------- |
| `asset_path`         | string | Content path to the Animation Blueprint  |
| `removed_state`      | string | Name of the removed state                |
| `transitions_removed`| number | Number of transitions also removed       |
| `remaining_states`   | array  | List of remaining states                 |
| `total_states`       | number | Total state count after removal          |

## Examples

### Remove a state from the first state machine

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "state_name": "Attack"
}
```

### Remove a state from a named state machine

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "state_name": "Jump",
  "state_machine_name": "Locomotion"
}
```

## Errors

| Condition               | `isError` | Message                                     |
| ----------------------- | --------- | ------------------------------------------- |
| Missing `asset_path`    | `true`    | `Missing required parameter: asset_path...` |
| Missing `state_name`    | `true`    | `Missing required parameter: ...state_name` |
| AnimBP not found        | `true`    | `Animation Blueprint not found at '...'`    |
| State machine not found | `true`    | `State machine '...' not found`             |
| State not found         | `true`    | `State '...' not found in state machine`    |
