# add_variable_set

Add a variable setter node to a Blueprint graph.

## Category
Blueprint — Graph / Node Tools

## Parameters

| Name            | Type    | Required | Description |
| --------------- | ------- | -------- | ----------- |
| `asset_path`    | string  | Yes      | Blueprint asset path (e.g. `/Game/MyBP`) |
| `variable_name` | string  | Yes      | Name of the variable to set (must exist in the Blueprint or parent class) |
| `graph_name`    | string  | No       | Target graph name (default: `EventGraph`). Searches ubergraph, function, and macro graphs |
| `position_x`    | integer | No       | X position in graph coordinates (default: 0) |
| `position_y`    | integer | No       | Y position in graph coordinates (default: 0) |

## Behaviour

1. Validates the variable exists in the Blueprint's own variables or the parent class properties.
2. Locates the target graph (ubergraph, function, or macro graphs).
3. Creates a `UK2Node_VariableSet` node referencing the variable as a self member.
4. The setter node has exec input/output pins for execution flow, plus a typed input pin for the new value.
5. Compiles the Blueprint and marks it as structurally modified.

## Response

```jsonc
{
  "asset_path": "/Game/MyBP",
  "graph_name": "EventGraph",
  "node_id": "K2Node_VariableSet_0",
  "node_class": "K2Node_VariableSet",
  "variable_name": "Health",
  "title": "Set Health",
  "position": { "x": 0, "y": 0 },
  "pins": [
    { "pin_id": "execute", "type": "exec", "direction": "input" },
    { "pin_id": "then", "type": "exec", "direction": "output" },
    { "pin_id": "Health", "type": "float", "direction": "input" },
    { "pin_id": "Output_Get", "type": "float", "direction": "output" }
  ]
}
```

## Undo

The tool emits a `remove_node` undo action. Calling undo removes the setter node from the graph.

## Errors

| Condition | Message (contains) |
| --------- | ------------------ |
| Blueprint not found | `not found` |
| Variable not found | `not found` |
| Graph not found | `not found` |
| Missing required parameters | `required` |

## Examples

### Add a setter for a Blueprint variable

```json
{
  "tool": "add_variable_set",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "variable_name": "Health"
  }
}
```

### Add a setter with position

```json
{
  "tool": "add_variable_set",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "variable_name": "Health",
    "position_x": 400,
    "position_y": 100
  }
}
```

### Add a setter in a function graph

```json
{
  "tool": "add_variable_set",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "variable_name": "PlayerName",
    "graph_name": "MyFunction"
  }
}
```
