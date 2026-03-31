# remove_pcg_node

Remove a node from a PCG Graph. Cannot remove built-in input/output nodes.

## Input

```json
{
  "asset_path": "/Game/PCG/MyGraph",
  "node_name": "PCGNode_0"
}
```

| Parameter    | Required | Description                                    |
| ------------ | -------- | ---------------------------------------------- |
| `asset_path` | **yes**  | Asset path of the PCG graph                    |
| `node_name`  | **yes**  | Name of the node to remove (from `add_pcg_node` or `get_pcg_graph_info`) |

## Output

```json
{
  "removed": true,
  "node_name": "PCGNode_0",
  "graph": "MyGraph",
  "remaining_node_count": 2
}
```

| Field                  | Type    | Description                                   |
| ---------------------- | ------- | --------------------------------------------- |
| `removed`              | boolean | Always `true` on success                      |
| `node_name`            | string  | Name of the removed node                      |
| `graph`                | string  | Graph asset name                              |
| `remaining_node_count` | number  | Number of user nodes remaining in the graph   |

## Examples

### Remove a node by name

```json
{"asset_path": "/Game/PCG/MyGraph", "node_name": "PCGNode_0"}
```

## Errors

| Condition                    | `isError` | Message                                             |
| ---------------------------- | --------- | --------------------------------------------------- |
| Missing asset_path           | `true`    | `asset_path is required`                            |
| Missing node_name            | `true`    | `node_name is required`                             |
| Graph not found              | `true`    | `PCG graph not found: <path>`                       |
| Node not found               | `true`    | `Node not found: <name>`                            |
| Removing input/output node   | `true`    | `Cannot remove built-in input/output node: <name>`  |
