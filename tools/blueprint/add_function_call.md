# add_function_call

Add a function call node to a Blueprint graph.

## Category
Blueprint — Graph / Node Tools

## Parameters

| Name                 | Type    | Required | Description |
| -------------------- | ------- | -------- | ----------- |
| `asset_path`         | string  | Yes      | Blueprint asset path (e.g. `/Game/MyBP`) |
| `function_reference` | string  | Yes      | Function to call: `ClassName::FunctionName` (e.g. `KismetSystemLibrary::PrintString`) or bare name (e.g. `PrintString`). Bare names search the parent class hierarchy first, then static BlueprintCallable library functions |
| `target_class`       | string  | No       | Owning class for bare function names (e.g. `KismetSystemLibrary`). Not needed when using `ClassName::FunctionName` format |
| `graph_name`         | string  | No       | Target graph name (default: `EventGraph`). Searches ubergraph, function, and macro graphs |
| `position_x`         | integer | No       | X position in graph coordinates (default: 0) |
| `position_y`         | integer | No       | Y position in graph coordinates (default: 0) |

## Behaviour

1. Locates the target graph in the Blueprint (ubergraph, function, or macro graphs).
2. Resolves the function reference:
   - `ClassName::FunctionName` — finds the class, then the function on that class.
   - Bare name with `target_class` — looks up the function on the specified class.
   - Bare name alone — searches the Blueprint's parent class hierarchy, then all static `BlueprintCallable` library functions.
3. Creates a `UK2Node_CallFunction` node, sets the function reference (self or external member).
4. Compiles the Blueprint and marks it as structurally modified.

## Response

```jsonc
{
  "asset_path": "/Game/MyBP",
  "graph_name": "EventGraph",
  "node_id": "K2Node_CallFunction_0",
  "node_class": "K2Node_CallFunction",
  "title": "Print String",
  "function_name": "PrintString",
  "function_owner": "KismetSystemLibrary",
  "is_self_context": false,
  "position": { "x": 0, "y": 0 },
  "pins": [
    { "pin_id": "execute", "type": "exec", "direction": "input" },
    { "pin_id": "then", "type": "exec", "direction": "output" },
    { "pin_id": "InString", "type": "FString", "direction": "input" }
  ]
}
```

## Undo

The tool emits a `remove_node` undo action. Calling undo removes the function call node from the graph.

## Errors

| Condition | Message (contains) |
| --------- | ------------------ |
| Blueprint not found | `not found` |
| Graph not found | `not found` |
| Missing required parameters | `required` |
| Target class not found | `not found` |
| Function not found on class | `not found` |
| Function not found anywhere | `not found` |

## Examples

### Add a function call with ClassName::FunctionName

```json
{
  "tool": "add_function_call",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "function_reference": "KismetSystemLibrary::PrintString"
  }
}
```

### Add a function call with target_class

```json
{
  "tool": "add_function_call",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "function_reference": "Delay",
    "target_class": "KismetSystemLibrary"
  }
}
```

### Add a function call with auto-discovery

```json
{
  "tool": "add_function_call",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "function_reference": "PrintString"
  }
}
```

### Add a self-context function call

```json
{
  "tool": "add_function_call",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "function_reference": "K2_GetActorLocation",
    "position_x": 400,
    "position_y": -100
  }
}
```
