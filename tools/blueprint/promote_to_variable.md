# promote_to_variable

Promote a pin on a Blueprint node to a new member variable.

## Category
Blueprint — Graph / Node Tools

## Parameters

| Name            | Type    | Required | Description |
| --------------- | ------- | -------- | ----------- |
| `asset_path`    | string  | Yes      | Blueprint asset path (e.g. `/Game/MyBP`) |
| `node_id`       | string  | Yes      | ID of the node containing the pin (use `search_blueprint_nodes` or `get_blueprint_graph` to discover) |
| `pin_name`      | string  | Yes      | Name of the pin to promote (e.g. `InString`, `ReturnValue`, `Duration`) |
| `variable_name` | string  | No       | Name for the new variable (default: pin name). Must not already exist |
| `graph_name`    | string  | No       | Graph containing the node (default: `EventGraph`) |
| `position_x`    | integer | No       | X position for the new node (default: auto-offset from source node) |
| `position_y`    | integer | No       | Y position for the new node (default: same Y as source node) |

## Behaviour

1. Finds the specified node and pin in the Blueprint graph.
2. Validates the pin can be promoted (rejects exec, hidden, and delegate pins).
3. Extracts the pin's `FEdGraphPinType` and creates a new Blueprint member variable with that type.
4. Sets the variable as instance-editable by default.
5. Creates the appropriate node based on pin direction:
   - **Input pin** → `UK2Node_VariableGet` (getter feeds data into the pin)
   - **Output pin** → `UK2Node_VariableSet` (setter stores the pin's output)
6. Breaks any existing connections on the target pin and wires the new variable node to it.
7. Compiles the Blueprint and marks the package dirty.

## Response

```jsonc
{
  "asset_path": "/Game/MyBP",
  "graph_name": "EventGraph",
  "variable_name": "MyMessage",
  "variable_type": "FString",
  "node_id": "K2Node_CallFunction_0",
  "pin_name": "InString",
  "pin_direction": "input",
  "created_node_id": "K2Node_VariableGet_0",
  "created_node_class": "K2Node_VariableGet",
  "connected": true,
  "position": { "x": 150, "y": 0 },
  "created_node_pins": [
    { "pin_id": "MyMessage", "type": "FString", "direction": "output" }
  ]
}
```

## Undo

The tool emits a `promote_to_variable` undo action with the variable name and created node ID. To reverse: remove the variable and the created getter/setter node.

## Errors

| Condition | Message (contains) |
| --------- | ------------------ |
| Blueprint not found | `not found` |
| Node not found | `not found` |
| Pin not found | `not found` |
| Exec pin | `Exec` |
| Hidden pin | `hidden` |
| Delegate pin | `Delegate` |
| Variable already exists | `already exists` |
| Missing `node_id` | `required` |
| Missing `pin_name` | `required` |

## Examples

### Promote an input pin (creates getter)

```json
{
  "tool": "promote_to_variable",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_id": "K2Node_CallFunction_0",
    "pin_name": "InString",
    "variable_name": "MyMessage"
  }
}
```

### Promote an output pin (creates setter)

```json
{
  "tool": "promote_to_variable",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_id": "K2Node_CallFunction_1",
    "pin_name": "ReturnValue",
    "variable_name": "ComputedResult",
    "position_x": 600,
    "position_y": 200
  }
}
```
