# search_blueprint_nodes

Search nodes across all graphs in a Blueprint by title, node class, or comment text. Returns matching nodes with their graph context. Use this to find specific nodes (e.g. all function calls, all events, nodes mentioning a variable) without fetching every graph in full.

## Input

```json
{
  "asset_path": "/Game/Blueprints/BP_MyActor",
  "query": "Print",
  "node_class": "K2Node_CallFunction",
  "graph_name": "EventGraph",
  "include_pins": false,
  "max_results": 50
}
```

| Parameter      | Required | Description                                                                    |
| -------------- | -------- | ------------------------------------------------------------------------------ |
| `asset_path`   | yes      | Asset path of the Blueprint                                                    |
| `query`        | no*      | Substring to match against node title, comment, and pin names (case-insensitive) |
| `node_class`   | no*      | Filter by node class name, substring match (e.g. `K2Node_CallFunction`)        |
| `graph_name`   | no       | Restrict search to a specific graph (e.g. `EventGraph`)                        |
| `include_pins` | no       | Include pin data in results (default: `false`)                                 |
| `max_results`  | no       | Maximum results to return (default: `50`, `0` = no limit)                      |

\* At least one of `query` or `node_class` is required.

## Output

```json
{
  "asset_path": "/Game/Blueprints/BP_MyActor",
  "name": "BP_MyActor",
  "match_count": 2,
  "total_nodes_scanned": 15,
  "query": "Print",
  "node_class_filter": "K2Node_CallFunction",
  "results": [
    {
      "graph_name": "EventGraph",
      "graph_type": "UbergraphPage",
      "node_id": "K2Node_CallFunction_0",
      "node_class": "K2Node_CallFunction",
      "title": "Print String",
      "position": { "x": 200, "y": 0 }
    }
  ]
}
```

## Result Entry Fields

| Field        | Type   | Description                                              |
| ------------ | ------ | -------------------------------------------------------- |
| `graph_name` | string | Name of the graph containing this node                   |
| `graph_type` | string | `UbergraphPage`, `FunctionGraph`, `MacroGraph`, `DelegateSignatureGraph` |
| `node_id`    | string | Unique node identifier within the graph                  |
| `node_class` | string | Node class (e.g. `K2Node_Event`, `K2Node_CallFunction`)  |
| `title`      | string | Human-readable title                                     |
| `full_title` | string | Extended title (only if different from `title`)          |
| `position`   | object | `{ "x": int, "y": int }` position in graph editor       |
| `comment`    | string | Node comment text (if any)                               |
| `pins[]`     | array  | Pin data (only when `include_pins` is `true`)            |
