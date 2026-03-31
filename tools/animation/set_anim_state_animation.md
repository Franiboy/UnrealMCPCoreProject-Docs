# set_anim_state_animation

Set the animation played by a state in a state machine. Creates a SequencePlayer node in the state's animation graph if one does not already exist, sets the animation, and wires it to the result node.

## Input

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "state_name": "Idle",
  "animation": "/Game/Animations/AS_Idle",
  "state_machine_name": "Locomotion"
}
```

| Parameter            | Required | Default | Description                                                  |
| -------------------- | -------- | ------- | ------------------------------------------------------------ |
| `asset_path`         | **yes**  | ---     | Content path to the Animation Blueprint.                     |
| `state_name`         | **yes**  | ---     | Name of the state to set the animation for.                  |
| `animation`          | **yes**  | ---     | Content path to the AnimSequence to play in this state.      |
| `state_machine_name` | no       | first   | State machine name. Uses the first state machine if omitted. |

## Output

| Field                | Type    | Description                                     |
| -------------------- | ------- | ----------------------------------------------- |
| `asset_path`         | string  | Content path to the Animation Blueprint         |
| `state_name`         | string  | Name of the state                               |
| `animation`          | string  | Content path to the assigned animation          |
| `animation_name`     | string  | Name of the animation asset                     |
| `created_new_player` | boolean | Whether a new SequencePlayer node was created   |

## Examples

### Set animation on a state

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "state_name": "Idle",
  "animation": "/Game/Animations/AS_Idle"
}
```

### Update animation on a state (replaces existing)

```json
{
  "asset_path": "/Game/Animations/ABP_Character",
  "state_name": "Walk",
  "animation": "/Game/Animations/AS_Walk_Fast",
  "state_machine_name": "Locomotion"
}
```

## Errors

| Condition               | `isError` | Message                                       |
| ----------------------- | --------- | --------------------------------------------- |
| Missing `asset_path`    | `true`    | `Missing required parameter: asset_path...`   |
| Missing `state_name`    | `true`    | `Missing required parameter: ...state_name`   |
| Missing `animation`     | `true`    | `Missing required parameter: ...animation`    |
| AnimBP not found        | `true`    | `Animation Blueprint not found at '...'`      |
| Animation not found     | `true`    | `Animation asset not found at '...'`          |
| State machine not found | `true`    | `State machine '...' not found`               |
| State not found         | `true`    | `State '...' not found in state machine`      |
| No animation graph      | `true`    | `State '...' has no animation graph`          |
