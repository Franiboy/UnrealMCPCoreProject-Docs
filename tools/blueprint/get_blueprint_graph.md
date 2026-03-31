# get_blueprint_graph

Get detailed node-level graph data from a Blueprint asset. Returns all nodes with their pins, connections, positions, and types. Supports all Blueprint types including Animation Blueprints.

## Input

```json
{
  "asset_path": "/Game/Blueprints/BP_MyActor",
  "graph_name": "EventGraph",
  "include_pin_defaults": true,
  "max_nodes": 0
}
```

| Parameter              | Required | Description                                                       |
| ---------------------- | -------- | ----------------------------------------------------------------- |
| `asset_path`           | yes      | Asset path of the Blueprint                                       |
| `graph_name`           | no       | Filter to a specific graph by name (e.g. `EventGraph`)            |
| `include_pin_defaults` | no       | Include pin default values (default: `true`)                      |
| `max_nodes`            | no       | Max nodes per graph, 0 = no limit (default: `0`)                  |

## Output

```json
{
  "asset_path": "/Game/Blueprints/BP_MyActor",
  "name": "BP_MyActor",
  "blueprint_type": "Normal",
  "graph_count": 2,
  "graphs": [
    {
      "name": "EventGraph",
      "graph_type": "UbergraphPage",
      "schema": "EdGraphSchema_K2",
      "node_count": 3,
      "nodes": [
        {
          "node_id": "K2Node_Event_0",
          "node_class": "K2Node_Event",
          "title": "Event BeginPlay",
          "position": { "x": -200, "y": 0 },
          "pins": [
            {
              "pin_id": "then",
              "direction": "output",
              "type": "exec",
              "connected_to": [
                { "node_id": "K2Node_CallFunction_0", "pin_id": "execute" }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

## Graph Fields

| Field              | Type   | Description                                                              |
| ------------------ | ------ | ------------------------------------------------------------------------ |
| `name`             | string | Graph name (e.g. `EventGraph`, `MyFunction`)                             |
| `graph_type`       | string | `UbergraphPage`, `FunctionGraph`, `MacroGraph`, `DelegateSignatureGraph` |
| `schema`           | string | Schema class (e.g. `EdGraphSchema_K2`, `AnimationGraphSchema`)           |
| `node_count`       | int    | Total number of nodes in the graph                                       |
| `truncated`        | bool   | Present and `true` if `max_nodes` was applied                            |
| `nodes[]`          | array  | Array of node objects                                                    |

## Node Fields

| Field        | Type   | Description                                              |
| ------------ | ------ | -------------------------------------------------------- |
| `node_id`    | string | Unique node identifier within the graph                  |
| `node_class` | string | Node class (e.g. `K2Node_Event`, `K2Node_CallFunction`)  |
| `title`      | string | Human-readable title (e.g. `Event BeginPlay`)            |
| `full_title` | string | Extended title (only if different from `title`)           |
| `position`   | object | `{ "x": int, "y": int }` position in the graph editor   |
| `comment`    | string | Node comment text (if any)                               |
| `pins[]`     | array  | Array of pin objects                                     |

## Pin Fields

| Field            | Type   | Description                                                  |
| ---------------- | ------ | ------------------------------------------------------------ |
| `pin_id`         | string | Pin name (unique within a node)                              |
| `direction`      | string | `input` or `output`                                          |
| `type`           | string | Pin type (e.g. `exec`, `bool`, `float`, `FString`, `Actor`) |
| `default_value`  | string | Default value text (if `include_pin_defaults` is true)       |
| `default_object` | string | Default object path (for object-type pins)                   |
| `connected_to[]` | array  | Array of `{ "node_id", "pin_id" }` connection targets       |
