# collapse_to_function

Collapse a set of selected nodes from a Blueprint graph into a new function.

## Category
Blueprint — Graph / Node Tools

## Parameters

| Name            | Type     | Required | Description |
| --------------- | -------- | -------- | ----------- |
| `asset_path`    | string   | Yes      | Blueprint asset path (e.g. `/Game/MyBP`) |
| `node_ids`      | string[] | Yes      | Array of node IDs to collapse (use `search_blueprint_nodes` or `get_blueprint_graph` to discover IDs) |
| `function_name` | string   | No       | Name for the new function (default: `NewFunction`). Made unique automatically if a collision exists |
| `graph_name`    | string   | No       | Source graph containing the nodes (default: `EventGraph`). Searches ubergraph, function, and macro graphs |

## Behaviour

1. Validates all node IDs exist in the source graph and can be encapsulated (not function terminators or tunnel nodes).
2. Analyses "gateway" connections — pins on selected nodes that link to non-selected nodes — to derive the function's input/output signature.
3. Validates the selection has at most one exec entry point and one exec exit target (required for a valid function).
4. Creates a new function graph with entry and result nodes. Adds user-defined pins on entry/result matching the gateway data connections.
5. Breaks external connections, moves selected nodes into the function graph, and wires them to entry/result pins.
6. Connects unconnected exec pins to entry/result to ensure a complete exec path through the function.
7. Compiles the Blueprint so the function exists in the generated class.
8. Creates a `UK2Node_CallFunction` node in the source graph at the center position of the collapsed nodes.
9. Wires all previously-external connections to the corresponding pins on the call node.
10. Final compile and mark package dirty.

## Response

```jsonc
{
  "asset_path": "/Game/MyBP",
  "source_graph": "EventGraph",
  "function_name": "MyFunction",
  "function_graph": "MyFunction",
  "call_node_id": "K2Node_CallFunction_3",
  "call_node_class": "K2Node_CallFunction",
  "nodes_collapsed": 2,
  "position": { "x": 400, "y": 100 },
  "entry_node": {
    "node_id": "K2Node_FunctionEntry_0",
    "pins": [ /* ... */ ]
  },
  "result_node": {
    "node_id": "K2Node_FunctionResult_0",
    "pins": [ /* ... */ ]
  },
  "call_node_pins": [
    { "pin_id": "execute", "type": "exec", "direction": "input" },
    { "pin_id": "then", "type": "exec", "direction": "output" }
  ]
}
```

## Undo

The tool emits a `remove_blueprint_function` undo action with the function name. Calling undo removes the entire function graph. Note: the original node arrangement in the source graph is not automatically restored.

## Errors

| Condition | Message (contains) |
| --------- | ------------------ |
| Blueprint not found | `not found` |
| Graph not found | `not found` |
| Node(s) not found | `not found` |
| Node cannot be encapsulated | `cannot be placed` |
| Multiple exec entry points | `multiple exec entry` |
| Multiple exec exit targets | `multiple exec exit` |
| Missing/empty `node_ids` | `required` |

## Examples

### Collapse a single node

```json
{
  "tool": "collapse_to_function",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_ids": ["K2Node_CallFunction_0"],
    "function_name": "DoSomething"
  }
}
```

### Collapse multiple connected nodes

```json
{
  "tool": "collapse_to_function",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_ids": ["K2Node_CallFunction_0", "K2Node_CallFunction_1", "K2Node_VariableGet_0"],
    "function_name": "ProcessData",
    "graph_name": "EventGraph"
  }
}
```
