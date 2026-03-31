# remove_node

Remove a node from a Blueprint graph by node ID.

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `asset_path` | string | yes | Blueprint asset path (e.g. `/Game/MyBP`) |
| `node_id` | string | yes | Node ID to remove (e.g. `K2Node_CallFunction_0`). Use `get_blueprint_graph` or `search_blueprint_nodes` to discover node IDs. |
| `graph_name` | string | no | Optional graph name to narrow the search. If omitted, all graphs (Ubergraph, Function, Macro) are searched. |

## Output

JSON object with the removed node's details:

| Field | Type | Description |
|-------|------|-------------|
| `asset_path` | string | Blueprint asset path |
| `graph_name` | string | Graph the node was removed from |
| `node_id` | string | ID of the removed node |
| `node_class` | string | Class name of the removed node |
| `title` | string | Display title of the removed node |
| `position` | object | `{x, y}` — position the node had in the graph |
| `comment` | string | Node comment (if any) |

## Example

```jsonc
// Request
{
  "tool": "remove_node",
  "arguments": {
    "asset_path": "/Game/MyBP",
    "node_id": "K2Node_CallFunction_3",
    "graph_name": "EventGraph"
  }
}

// Response
{
  "asset_path": "/Game/MyBP",
  "graph_name": "EventGraph",
  "node_id": "K2Node_CallFunction_3",
  "node_class": "K2Node_CallFunction",
  "title": "Print String",
  "position": { "x": 300, "y": 100 }
}
```

## Notes

- The Blueprint is recompiled after the node is removed.
- If `graph_name` is omitted, the node is searched across all graphs (EventGraph, function graphs, macro graphs).
- Use `get_blueprint_graph` or `search_blueprint_nodes` to discover node IDs before calling this tool.
- Removing structural nodes (like `K2Node_FunctionEntry`) may cause compilation errors.
