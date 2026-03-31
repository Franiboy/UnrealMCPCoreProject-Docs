# collapse_to_macro

Collapse a set of selected nodes from a Blueprint graph into a new macro.

## Category
Blueprint — Graph / Node Tools

## Parameters

| Name         | Type     | Required | Description |
| ------------ | -------- | -------- | ----------- |
| `asset_path` | string   | Yes      | Blueprint asset path (e.g. `/Game/MyBP`) |
| `node_ids`   | string[] | Yes      | Array of node IDs to collapse (use `search_blueprint_nodes` or `get_blueprint_graph` to discover IDs) |
| `macro_name` | string   | No       | Name for the new macro (default: `NewMacro`). Made unique automatically if a collision exists |
| `graph_name` | string   | No       | Source graph containing the nodes (default: `EventGraph`). Searches ubergraph, function, and macro graphs |

## Behaviour

1. Validates all node IDs exist in the source graph and can be encapsulated (not function terminators or tunnel nodes).
2. Analyses "gateway" connections — pins on selected nodes that link to non-selected nodes — to derive the macro's input/output signature.
3. Unlike `collapse_to_function`, macros support **multiple exec entry/exit pins**, so no single-exec validation is enforced.
4. Creates a new macro graph with input and output tunnel nodes. Adds user-defined pins on tunnel nodes matching the gateway connections.
5. Breaks external connections, moves selected nodes into the macro graph, and wires them to the tunnel pins.
6. Creates a `UK2Node_MacroInstance` node in the source graph at the center position of the collapsed nodes.
7. Wires all previously-external connections to the corresponding pins on the macro instance node.
8. Compiles the Blueprint and marks the package dirty.

## Response

```jsonc
{
  "asset_path": "/Game/MyBP",
  "source_graph": "EventGraph",
  "macro_name": "MyMacro",
  "macro_graph": "MyMacro",
  "macro_node_id": "K2Node_MacroInstance_0",
  "macro_node_class": "K2Node_MacroInstance",
  "nodes_collapsed": 2,
  "position": { "x": 400, "y": 100 },
  "input_tunnel": {
    "node_id": "K2Node_Tunnel_0",
    "pins": [ /* ... */ ]
  },
  "output_tunnel": {
    "node_id": "K2Node_Tunnel_1",
    "pins": [ /* ... */ ]
  },
  "macro_node_pins": [
    { "pin_id": "execute", "type": "exec", "direction": "input" },
    { "pin_id": "then", "type": "exec", "direction": "output" }
  ]
}
```

## Undo

The tool emits a `collapse_to_macro` undo action with the macro name. Note: the original node arrangement in the source graph is not automatically restored.

## Errors

| Condition | Message (contains) |
| --------- | ------------------ |
| Blueprint not found | `not found` |
| Graph not found | `not found` |
| Node(s) not found | `not found` |
| Node cannot be encapsulated | `cannot be encapsulated` |
| Missing/empty `node_ids` | `required` |

## Examples

### Collapse a single node

```json
{
  "tool": "collapse_to_macro",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_ids": ["K2Node_CallFunction_0"],
    "macro_name": "DoSomething"
  }
}
```

### Collapse multiple connected nodes

```json
{
  "tool": "collapse_to_macro",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_ids": ["K2Node_CallFunction_0", "K2Node_CallFunction_1", "K2Node_VariableGet_0"],
    "macro_name": "ProcessData",
    "graph_name": "EventGraph"
  }
}
```
