# connect_pins

Wire two pins together in a Blueprint graph.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | yes | Blueprint asset path (e.g. `/Game/MyBP`) |
| `source_node_id` | string | yes | Source node ID (e.g. `K2Node_Event_0`). Use `get_blueprint_graph` to discover node IDs. |
| `source_pin_id` | string | yes | Source pin ID (e.g. `then`, `ReturnValue`). Use `get_blueprint_graph` with pins to discover pin IDs. |
| `target_node_id` | string | yes | Target node ID (e.g. `K2Node_CallFunction_0`). |
| `target_pin_id` | string | yes | Target pin ID (e.g. `execute`, `InString`). |
| `graph_name` | string | no | Optional graph name to narrow the node search. If omitted, all graphs are searched. |

## Output

JSON object with the connection details:

| Field | Type | Description |
|-------|------|-------------|
| `asset_path` | string | Blueprint asset path |
| `graph_name` | string | Graph containing the connected nodes |
| `source` | object | `{node_id, pin_id, direction, type}` — source pin details |
| `target` | object | `{node_id, pin_id, direction, type}` — target pin details |

## Example

```jsonc
// Request
{
  "tool": "connect_pins",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "source_node_id": "K2Node_Event_0",
    "source_pin_id": "then",
    "target_node_id": "K2Node_CallFunction_0",
    "target_pin_id": "execute"
  }
}

// Response
{
  "asset_path": "/Game/MyBP",
  "graph_name": "EventGraph",
  "source": {
    "node_id": "K2Node_Event_0",
    "pin_id": "then",
    "direction": "output",
    "type": "exec"
  },
  "target": {
    "node_id": "K2Node_CallFunction_0",
    "pin_id": "execute",
    "direction": "input",
    "type": "exec"
  }
}
```

## Notes

- Uses the graph schema (`UEdGraphSchema_K2::TryCreateConnection`) to validate type compatibility and pin direction. Incompatible connections are rejected with a descriptive error.
- Both nodes must be in the same graph.
- The Blueprint is recompiled after the connection is made.
- Pin direction does not matter for the source/target order — the schema handles auto-swapping output-to-input.
- Use `get_blueprint_graph` or `get_blueprint_node` to discover node IDs and pin IDs before calling this tool.
