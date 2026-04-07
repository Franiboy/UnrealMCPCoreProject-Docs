# implement_function_override

Override a BlueprintImplementableEvent or BlueprintNativeEvent function from a parent C++ class or an implemented Blueprint interface. Creates a function graph with the correct signature (matching the parent's parameters and return type). For interface functions whose graph stub already exists, returns the existing stub info. Returns entry/result node IDs so you can add logic via `add_node`, `connect_pins`, etc.

## Category
Blueprint — Graph / Node Tools

## Parameters

| Name            | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| `asset_path`    | string | Yes      | Asset path of the Blueprint (e.g. `/Game/UI/WBP_MainMenu`) |
| `function_name` | string | Yes      | Name of the parent function to override (e.g. `BP_OnHandleBackAction`, `ReceiveBeginPlay`) |

### Parameter Aliases

- `blueprint_path` -> `asset_path`

### Tool Aliases

- `override_function`
- `add_function_override`
- `override_event`

## Behaviour

1. Locates the Blueprint at `asset_path`.
2. Walks the parent class hierarchy to find a `BlueprintImplementableEvent` or `BlueprintNativeEvent` function matching `function_name`. If not found in the parent class, searches all implemented interfaces.
3. Validates that the function is not already overridden (no existing function graph or event node).
4. For interface functions: if the interface graph stub already exists, returns its info directly (entry/result node IDs, parameters, etc.) without creating a new graph.
5. Creates a new function graph with the correct signature (parameters, return type) matching the parent declaration.
6. Populates the entry node and, if the function has a return value or out parameters, a result node.
7. Compiles the Blueprint and marks it as structurally modified.

## Response

```jsonc
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "function_name": "BP_OnHandleBackAction",
  "graph_name": "BP_OnHandleBackAction",
  "parent_class": "CommonActivatableWidget",
  "is_native_event": false,
  "has_return_value": true,
  "entry_node_id": "K2Node_FunctionEntry_0",
  "result_node_id": "K2Node_FunctionResult_0",
  "return_type": "bool",
  "inputs": [
    { "name": "SomeParam", "type": "int" }
  ],
  "outputs": [
    { "name": "ReturnValue", "type": "bool", "is_out": false }
  ],
  "undo": {
    "action": "remove_function",
    "asset_path": "/Game/UI/WBP_MainMenu",
    "function_name": "BP_OnHandleBackAction"
  }
}
```

| Field             | Description |
| ----------------- | ----------- |
| `asset_path`      | The Blueprint path |
| `function_name`   | The overridden function name |
| `graph_name`      | Name of the created function graph |
| `parent_class`    | Name of the class that owns the function |
| `is_native_event` | Whether it's a BlueprintNativeEvent (`true`) or BlueprintImplementableEvent (`false`) |
| `is_interface_function` | Whether the function comes from an implemented interface (only present for interface functions) |
| `has_return_value` | Whether the function returns a value |
| `entry_node_id`   | GUID of the FunctionEntry node |
| `result_node_id`  | GUID of the FunctionResult node (only present if function has return value/out params) |
| `return_type`     | String type of return value (only if `has_return_value`) |
| `inputs`          | Array of input parameters `[{name, type}]` |
| `outputs`         | Array of output parameters `[{name, type, is_out}]` |
| `undo`            | Action to remove the override |

## Undo

The tool emits a `remove_function` undo action. Calling undo removes the override function graph from the Blueprint.

## Errors

| Condition | Message (contains) |
| --------- | ------------------ |
| Blueprint not found | `not found` |
| Function not found in parent class hierarchy or implemented interfaces | `not found` |
| Function is not a BlueprintImplementableEvent or BlueprintNativeEvent | `not overridable` |
| Function already overridden (function graph or event node already exists) | `already exists` |
| Missing required parameters | `required` |

## Examples

### Override a void event (no return value)

```json
{
  "tool": "implement_function_override",
  "arguments": {
    "asset_path": "/Game/Blueprints/BP_MyActor",
    "function_name": "ReceiveDestroyed"
  }
}
```

Response:

```jsonc
{
  "asset_path": "/Game/Blueprints/BP_MyActor",
  "function_name": "ReceiveDestroyed",
  "graph_name": "ReceiveDestroyed",
  "parent_class": "Actor",
  "is_native_event": true,
  "has_return_value": false,
  "entry_node_id": "ABCD1234..."
}
```

### Override a function with return value

```json
{
  "tool": "implement_function_override",
  "arguments": {
    "asset_path": "/Game/UI/WBP_MainMenu",
    "function_name": "BP_OnHandleBackAction"
  }
}
```

Response:

```jsonc
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "function_name": "BP_OnHandleBackAction",
  "graph_name": "BP_OnHandleBackAction",
  "parent_class": "CommonActivatableWidget",
  "is_native_event": false,
  "has_return_value": true,
  "entry_node_id": "...",
  "result_node_id": "...",
  "return_type": "bool"
}
```

## Notes

- UE5 auto-populates some events (e.g. `ReceiveBeginPlay`, `ReceiveTick`) in new Actor Blueprints. These count as "already overridden".
- The created function graph can be further modified using `add_node`, `connect_pins`, `set_pin_default`, etc.
- Use `remove_blueprint_function` to remove an override.
- The Blueprint is compiled after creating the override.
- For Blueprint interfaces added via `add_blueprint_interface`, the engine creates graph stubs automatically. Calling this tool for such functions returns the existing stub info without duplicating it.
