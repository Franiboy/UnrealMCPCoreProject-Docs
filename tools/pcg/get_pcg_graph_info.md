# get_pcg_graph_info

Get detailed information about a PCG graph asset including its nodes, pins, connections, hierarchical generation settings, and input/output nodes.

## Input

```json
{
  "asset_path": "/Game/PCG/PCG_LandscapeScatter",
  "include_nodes": true,
  "include_edges": true
}
```

| Parameter       | Required | Description                                                    |
| --------------- | -------- | -------------------------------------------------------------- |
| `asset_path`    | **yes**  | Asset path of the PCG graph (e.g. `/Game/PCG/MyGraph`)         |
| `include_nodes` | no       | Include node details (default: `true`)                         |
| `include_edges` | no       | Include edge/connection details (default: `true`)              |

## Output

```json
{
  "name": "PCG_LandscapeScatter",
  "asset_path": "/Game/PCG/PCG_LandscapeScatter",
  "node_count": 3,
  "hierarchical_generation": false,
  "use_2d_grid": true,
  "input_node": "InputNode",
  "output_node": "OutputNode",
  "nodes": [
    {
      "name": "SurfaceSampler_0",
      "display_name": "SurfaceSampler",
      "settings_class": "PCGSurfaceSamplerSettings",
      "position": { "x": 200, "y": 0 },
      "input_pins": [
        {
          "label": "In",
          "allowed_types": "Point|Landscape",
          "connected": true,
          "edge_count": 1,
          "allow_multiple_data": false,
          "is_output": false
        }
      ],
      "output_pins": [
        {
          "label": "Out",
          "allowed_types": "Point",
          "connected": true,
          "edge_count": 1,
          "allow_multiple_data": true,
          "is_output": true
        }
      ]
    }
  ],
  "edges": [
    {
      "from_node": "InputNode",
      "from_pin": "Out",
      "to_node": "SurfaceSampler_0",
      "to_pin": "In"
    }
  ],
  "edge_count": 1
}
```

### Top-level fields

| Field                      | Type    | Description                                          |
| -------------------------- | ------- | ---------------------------------------------------- |
| `name`                     | string  | Graph asset name                                     |
| `asset_path`               | string  | Asset path as provided                               |
| `node_count`               | number  | Number of user-created nodes (excludes input/output) |
| `hierarchical_generation`  | boolean | Whether hierarchical generation is enabled           |
| `use_2d_grid`              | boolean | Whether the graph uses a 2D grid                     |
| `input_node`               | string  | Name of the graph's input node                       |
| `output_node`              | string  | Name of the graph's output node                      |

### Node fields (when `include_nodes` is true)

| Field              | Type   | Description                                             |
| ------------------ | ------ | ------------------------------------------------------- |
| `name`             | string | Internal node name                                      |
| `display_name`     | string | Human-readable display name                             |
| `settings_class`   | string | Settings UClass name (e.g. `PCGSurfaceSamplerSettings`) |
| `authored_title`   | string | User-set title (if any)                                 |
| `comment`          | string | Node comment (if any)                                   |
| `position`         | object | `{x, y}` editor position                               |
| `input_pins`       | array  | Array of input pin objects                               |
| `output_pins`      | array  | Array of output pin objects                              |

### Pin fields

| Field                | Type    | Description                                          |
| -------------------- | ------- | ---------------------------------------------------- |
| `label`              | string  | Pin label/name                                       |
| `allowed_types`      | string  | Allowed data types (e.g. `Point`, `Point|Landscape`) |
| `connected`          | boolean | Whether the pin has any connections                  |
| `edge_count`         | number  | Number of connected edges                            |
| `allow_multiple_data`| boolean | Whether pin accepts multiple data inputs             |
| `is_output`          | boolean | Whether this is an output pin                        |

### Edge fields (when `include_edges` is true)

| Field       | Type   | Description                    |
| ----------- | ------ | ------------------------------ |
| `from_node` | string | Source node name               |
| `from_pin`  | string | Source pin label               |
| `to_node`   | string | Destination node name          |
| `to_pin`    | string | Destination pin label          |

## Examples

### Full graph inspection

```json
{"asset_path": "/Game/PCG/MyGraph"}
```

### Nodes only (no edges)

```json
{"asset_path": "/Game/PCG/MyGraph", "include_edges": false}
```

### Metadata only (no nodes or edges)

```json
{"asset_path": "/Game/PCG/MyGraph", "include_nodes": false, "include_edges": false}
```

## Errors

| Condition         | `isError` | Message                                    |
| ----------------- | --------- | ------------------------------------------ |
| Missing asset_path| `true`    | `asset_path is required`                   |
| Graph not found   | `true`    | `PCG graph not found: <path>`              |
