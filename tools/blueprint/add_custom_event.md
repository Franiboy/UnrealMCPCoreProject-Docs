# add_custom_event

Add a custom event node to a Blueprint graph.

## Category
Blueprint — Graph / Node Tools

## Parameters

| Name           | Type    | Required | Description |
| -------------- | ------- | -------- | ----------- |
| `asset_path`   | string  | Yes      | Blueprint asset path (e.g. `/Game/MyBP`) |
| `event_name`   | string  | Yes      | Name of the custom event (must be unique within the Blueprint) |
| `graph_name`   | string  | No       | Target graph name (default: `EventGraph`) |
| `parameters`   | array   | No       | Output parameters as `[{name, type}]`. Type uses the same strings as `add_blueprint_variable` (e.g. `float`, `int`, `bool`, `FString`, `FVector`) |
| `position_x`   | integer | No       | X position in graph coordinates (default: 0) |
| `position_y`   | integer | No       | Y position in graph coordinates (default: 0) |

## Behaviour

1. Locates the target ubergraph in the Blueprint.
2. Validates that no custom event with the same name already exists.
3. Creates a `UK2Node_CustomEvent` node with the given name.
4. Adds any typed output parameters via `CreateUserDefinedPin`.
5. Compiles the Blueprint and marks it as structurally modified.

## Response

```jsonc
{
  "asset_path": "/Game/MyBP",
  "graph_name": "EventGraph",
  "node_id": "K2Node_CustomEvent_0",
  "node_class": "K2Node_CustomEvent",
  "event_name": "OnGameStart",
  "position": { "x": 0, "y": 0 },
  "pins": [
    { "pin_id": "then", "type": "exec", "direction": "output" },
    { "pin_id": "NewHealth", "type": "float", "direction": "output" }
  ],
  "parameters": [
    { "name": "NewHealth", "type": "float" }
  ]
}
```

## Undo

The tool emits a `remove_node` undo action. Calling undo removes the custom event node from the graph.

## Errors

| Condition | Message (contains) |
| --------- | ------------------ |
| Blueprint not found | `not found` |
| Graph not found | `not found` |
| Missing required parameters | `required` |
| Duplicate event name | `already exists` |
| Invalid parameter type | Type-specific error from `StringToPinType` |

## Examples

### Add a simple custom event

```json
{
  "tool": "add_custom_event",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "event_name": "OnGameStart"
  }
}
```

### Add a custom event with typed parameters

```json
{
  "tool": "add_custom_event",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "event_name": "OnHealthChanged",
    "parameters": [
      { "name": "NewHealth", "type": "float" },
      { "name": "DamageCauser", "type": "Name" }
    ]
  }
}
```

### Add a positioned custom event

```json
{
  "tool": "add_custom_event",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "event_name": "OnPlayerDied",
    "position_x": 400,
    "position_y": -200
  }
}
```
