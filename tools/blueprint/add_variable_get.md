# add_variable_get

Add a variable getter node to a Blueprint graph.

## Category
Blueprint — Graph / Node Tools

## Parameters

| Name            | Type    | Required | Description |
| --------------- | ------- | -------- | ----------- |
| `asset_path`    | string  | Yes      | Blueprint asset path (e.g. `/Game/MyBP`) |
| `variable_name` | string  | Yes      | Name of the variable to get (must exist in the Blueprint or parent class) |
| `graph_name`    | string  | No       | Target graph name (default: `EventGraph`). Searches ubergraph, function, and macro graphs |
| `position_x`    | integer | No       | X position in graph coordinates (default: 0) |
| `position_y`    | integer | No       | Y position in graph coordinates (default: 0) |

## Behaviour

1. Validates the variable exists in the Blueprint's own variables or the parent class properties.
2. Locates the target graph (ubergraph, function, or macro graphs).
3. Creates a `UK2Node_VariableGet` node referencing the variable as a self member.
4. Compiles the Blueprint and marks it as structurally modified.

## Response

```jsonc
{
  "asset_path": "/Game/MyBP",
  "graph_name": "EventGraph",
  "node_id": "K2Node_VariableGet_0",
  "node_class": "K2Node_VariableGet",
  "variable_name": "Health",
  "title": "Health",
  "position": { "x": 0, "y": 0 },
  "pins": [
    { "pin_id": "Health", "type": "float", "direction": "output" }
  ]
}
```

## Undo

The tool emits a `remove_node` undo action. Calling undo removes the getter node from the graph.

## Errors

| Condition | Message (contains) |
| --------- | ------------------ |
| Blueprint not found | `not found` |
| Variable not found | `not found` |
| Graph not found | `not found` |
| Missing required parameters | `required` |

## Examples

### Add a getter for a Blueprint variable

```json
{
  "tool": "add_variable_get",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "variable_name": "Health"
  }
}
```

### Add a getter with position

```json
{
  "tool": "add_variable_get",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "variable_name": "Health",
    "position_x": 200,
    "position_y": -50
  }
}
```

### Add a getter for an inherited property

```json
{
  "tool": "add_variable_get",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "variable_name": "bCanBeDamaged"
  }
}
```
