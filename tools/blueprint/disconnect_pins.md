# disconnect_pins

Break a wire between two pins in a Blueprint graph.

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

JSON object with the disconnected pin details:

| Field | Type | Description |
|-------|------|-------------|
| `asset_path` | string | Blueprint asset path |
| `graph_name` | string | Graph containing the disconnected nodes |
| `source` | object | `{node_id, pin_id, direction, type}` — source pin details |
| `target` | object | `{node_id, pin_id, direction, type}` — target pin details |

## Example

```jsonc
// Request
{
  "tool": "disconnect_pins",
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

- The two pins must actually be connected; otherwise an error is returned.
- The Blueprint is recompiled after the disconnection.
- Pin order does not matter — the tool checks both `SourcePin->LinkedTo` directions.
- Use `get_blueprint_graph` or `get_blueprint_node` to discover node IDs, pin IDs, and existing connections before calling this tool.
