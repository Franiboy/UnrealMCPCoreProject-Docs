# add_pin

Add one or more dynamic pins to nodes that support it, such as Sequence, Switch, MakeArray, DoOnceMultiInput, and commutative math operator nodes. The Blueprint is compiled after adding pins.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "node_id": "K2Node_ExecutionSequence_0",
  "count": 2
}
```

| Parameter    | Required | Description                                                                                |
| ------------ | -------- | ------------------------------------------------------------------------------------------ |
| `asset_path` | yes      | Blueprint asset path (e.g. `/Game/Blueprints/BP_MyActor`)                                  |
| `node_id`    | yes      | Node ID returned by `add_node` or found via `get_blueprint_graph`                          |
| `graph_name` | no       | Target graph name. If omitted, searches all graphs (EventGraph, functions, macros).        |
| `count`      | no       | Number of pins to add (default: 1, max: 26). Stops early if the node reports it cannot accept more. |

## Supported Node Types

| Node Class                                 | Pin Type Added     |
| ------------------------------------------ | ------------------ |
| `K2Node_ExecutionSequence`                 | Output exec pins   |
| `K2Node_MakeArray`                         | Input element pins |
| `K2Node_DoOnceMultiInput`                  | Input exec pins    |
| `K2Node_CommutativeAssociativeBinaryOperator` | Input value pins |
| `K2Node_SwitchInteger` / `SwitchString`    | Case pins          |

Any node implementing `IK2Node_AddPinInterface` or deriving from `UK2Node_Switch` is supported.

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "graph_name": "EventGraph",
  "node_id": "K2Node_ExecutionSequence_0",
  "node_class": "K2Node_ExecutionSequence",
  "pins_added": 2,
  "total_pins": 5,
  "pins": [
    {"name": "execute", "direction": "input", "type": "exec"},
    {"name": "then 0", "direction": "output", "type": "exec"},
    {"name": "then 1", "direction": "output", "type": "exec"},
    {"name": "then 2", "direction": "output", "type": "exec"},
    {"name": "then 3", "direction": "output", "type": "exec"}
  ]
}
```

| Field        | Description                                          |
| ------------ | ---------------------------------------------------- |
| `pins_added` | Number of pins actually added (may be less than `count` if the node's limit was reached) |
| `total_pins` | Total pin count after adding                         |
| `pins`       | Full list of non-hidden pins after the operation     |

## Errors

| Condition                        | Error Message                                    |
| -------------------------------- | ------------------------------------------------ |
| Missing `asset_path` or `node_id` | Missing required parameter                     |
| Blueprint not found              | Blueprint not found at '...'                     |
| Node not found                   | Node '...' not found in '...'                    |
| Node doesn't support add_pin    | Node '...' does not support adding pins          |
| Node at pin limit                | Cannot add pins to node '...'                    |

## Notes

- Use `add_node` to create the target node first, then `add_pin` to expand it.
- The `node_id` is the engine-generated name like `K2Node_ExecutionSequence_0`.
- Undo removes the added pins.
