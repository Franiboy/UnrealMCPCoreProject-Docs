# add_anim_state

Add a new state to a state machine in an Animation Blueprint. The state is created with an empty animation graph and a result node. Use `set_anim_state_animation` to assign an animation to the state.

## Input

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "state_name": "Idle",
  "state_machine_name": "Locomotion",
  "position_x": 200,
  "position_y": 100
}
```

| Parameter            | Required | Default | Description                                                         |
| -------------------- | -------- | ------- | ------------------------------------------------------------------- |
| `asset_path`         | **yes**  | ---     | Content path to the Animation Blueprint.                            |
| `state_name`         | **yes**  | ---     | Name for the new state (e.g. `"Idle"`, `"Walk"`, `"Attack"`).      |
| `state_machine_name` | no       | first   | State machine name. Uses the first state machine if omitted.        |
| `position_x`         | no       | `0`     | X position in the state machine graph.                              |
| `position_y`         | no       | `0`     | Y position in the state machine graph.                              |

## Output

| Field          | Type   | Description                             |
| -------------- | ------ | --------------------------------------- |
| `asset_path`   | string | Content path to the Animation Blueprint |
| `state_name`   | string | Name of the created state               |
| `states`       | array  | List of all states in the state machine |
| `total_states` | number | Total state count after addition        |

## Examples

### Add a state to the first state machine

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "state_name": "Idle"
}
```

### Add a state to a named state machine at a specific position

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "state_name": "Jump",
  "state_machine_name": "Locomotion",
  "position_x": 400,
  "position_y": 200
}
```

## Errors

| Condition                 | `isError` | Message                                       |
| ------------------------- | --------- | --------------------------------------------- |
| Missing `asset_path`      | `true`    | `Missing required parameter: asset_path...`   |
| Missing `state_name`      | `true`    | `Missing required parameter: ...state_name..` |
| AnimBP not found          | `true`    | `Animation Blueprint not found at '...'`      |
| State machine not found   | `true`    | `State machine '...' not found`               |
| Duplicate state name      | `true`    | `State '...' already exists in state machine` |
