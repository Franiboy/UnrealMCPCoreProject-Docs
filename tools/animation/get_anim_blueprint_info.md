# get_anim_blueprint_info

Get deep information about an Animation Blueprint. Returns the target skeleton, parent class, state machines (with states, transitions, conduits, entry state), event graphs, function graphs, and variables with types and defaults.

For non-AnimBP assets (AnimSequence, AnimMontage, BlendSpace), use `get_animation_info` instead.

## Input

```json
{
  "asset_path": "/Game/Animations/ABP_Character"
}
```

| Parameter    | Required | Description                                                  |
| ------------ | -------- | ------------------------------------------------------------ |
| `asset_path` | **yes**  | Content path to the Animation Blueprint to inspect.          |

## Output

| Field                | Type   | Description                               |
| -------------------- | ------ | ----------------------------------------- |
| `name`               | string | Asset name                                |
| `asset_path`         | string | Full content path                         |
| `type`               | string | Always `"AnimBlueprint"`                  |
| `skeleton`           | string | Target skeleton name                      |
| `skeleton_path`      | string | Skeleton asset path (if available)        |
| `parent_class`       | string | Parent class name                         |
| `state_machines`     | array  | State machines (see below)                |
| `num_state_machines` | number | Number of state machines                  |
| `event_graphs`       | array  | Event graphs (name, num_nodes)            |
| `num_event_graphs`   | number | Number of event graphs                    |
| `function_graphs`    | array  | Function graphs (name, num_nodes)         |
| `num_function_graphs`| number | Number of function graphs                 |
| `variables`          | array  | Blueprint variables (see below)           |
| `num_variables`      | number | Number of variables                       |

### State Machine Object

| Field              | Type   | Description                                  |
| ------------------ | ------ | -------------------------------------------- |
| `name`             | string | State machine node title                     |
| `graph_name`       | string | Parent graph containing this state machine   |
| `entry_state`      | string | Default/entry state name (if set)            |
| `states`           | array  | State objects (see below)                    |
| `num_states`       | number | Number of states                             |
| `transitions`      | array  | Transition objects (see below)               |
| `num_transitions`  | number | Number of transitions                        |
| `conduits`         | array  | Conduit nodes (name only, if any)            |
| `num_conduits`     | number | Number of conduits (if any)                  |

### State Object

| Field                      | Type    | Description                                  |
| -------------------------- | ------- | -------------------------------------------- |
| `name`                     | string  | State name                                   |
| `state_type`               | string  | `SingleAnimation` or `BlendGraph`            |
| `always_reset_on_entry`    | boolean | Whether the state resets on re-entry         |
| `num_outgoing_transitions` | number  | Count of outgoing transitions                |

### Transition Object

| Field                | Type    | Description                                     |
| -------------------- | ------- | ----------------------------------------------- |
| `source`             | string  | Source state name                                |
| `target`             | string  | Target state name                                |
| `priority`           | number  | Priority order (lower = higher priority)         |
| `crossfade_duration` | number  | Crossfade blend duration in seconds              |
| `bidirectional`      | boolean | Whether the transition works in both directions  |
| `blend_mode`         | string  | Blend mode (e.g. `Linear`, `Cubic`, `Custom`)   |
| `has_custom_blend`   | boolean | Whether a custom blend graph is set              |

### Variable Object

| Field          | Type    | Description                              |
| -------------- | ------- | ---------------------------------------- |
| `name`         | string  | Variable name                            |
| `type`         | string  | Pin category (e.g. `bool`, `float`)      |
| `sub_type`     | string  | Sub-category object name (if applicable) |
| `category`     | string  | Variable category                        |
| `is_array`     | boolean | Whether the variable is an array         |
| `default_value`| string  | Default value (if set)                   |

## Example Response

```json
{
  "name": "ABP_Character",
  "asset_path": "/Game/Animations/ABP_Character.ABP_Character",
  "type": "AnimBlueprint",
  "skeleton": "SK_Mannequin",
  "skeleton_path": "/Game/Characters/SK_Mannequin.SK_Mannequin",
  "parent_class": "AnimInstance",
  "num_state_machines": 1,
  "state_machines": [
    {
      "name": "Locomotion",
      "graph_name": "AnimGraph",
      "entry_state": "Idle",
      "num_states": 3,
      "states": [
        {"name": "Idle", "state_type": "SingleAnimation", "always_reset_on_entry": false, "num_outgoing_transitions": 1},
        {"name": "Walk", "state_type": "SingleAnimation", "always_reset_on_entry": false, "num_outgoing_transitions": 2},
        {"name": "Run", "state_type": "BlendGraph", "always_reset_on_entry": false, "num_outgoing_transitions": 1}
      ],
      "num_transitions": 4,
      "transitions": [
        {"source": "Idle", "target": "Walk", "priority": 1, "crossfade_duration": 0.2, "bidirectional": false, "blend_mode": "Linear", "has_custom_blend": false},
        {"source": "Walk", "target": "Run", "priority": 1, "crossfade_duration": 0.2, "bidirectional": false, "blend_mode": "Linear", "has_custom_blend": false},
        {"source": "Run", "target": "Walk", "priority": 1, "crossfade_duration": 0.3, "bidirectional": false, "blend_mode": "Linear", "has_custom_blend": false},
        {"source": "Walk", "target": "Idle", "priority": 2, "crossfade_duration": 0.2, "bidirectional": false, "blend_mode": "Linear", "has_custom_blend": false}
      ]
    }
  ],
  "num_event_graphs": 1,
  "event_graphs": [
    {"name": "EventGraph", "num_nodes": 5}
  ],
  "num_function_graphs": 1,
  "function_graphs": [
    {"name": "AnimGraph", "num_nodes": 3}
  ],
  "num_variables": 2,
  "variables": [
    {"name": "Speed", "type": "real", "category": "Default", "is_array": false, "default_value": "0.0"},
    {"name": "IsInAir", "type": "bool", "category": "Default", "is_array": false}
  ]
}
```

## Errors

| Condition               | `isError` | Message                                                |
| ----------------------- | --------- | ------------------------------------------------------ |
| Missing `asset_path`    | `true`    | `Missing required parameter 'asset_path'`              |
| Asset not found         | `true`    | `Asset '...' is not an Animation Blueprint (not found)`|
| Not an AnimBlueprint    | `true`    | `Asset '...' is not an Animation Blueprint (found ...)` |
