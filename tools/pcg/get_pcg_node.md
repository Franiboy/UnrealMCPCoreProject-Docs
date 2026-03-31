# get_pcg_node

Detailed inspection of a single PCG node within a graph: settings type, seed, enabled/debug status, pins with connections and tooltips, overridable parameters, and reflection-based settings properties.

## Input

```json
{
  "asset_path": "/Game/PCG/PCG_LandscapeScatter",
  "node_name": "SurfaceSampler_0",
  "include_settings": true
}
```

| Parameter          | Required | Description                                                        |
| ------------------ | -------- | ------------------------------------------------------------------ |
| `asset_path`       | **yes**  | Asset path of the PCG graph containing the node                    |
| `node_name`        | **yes**  | Node name (from `get_pcg_graph_info` `nodes[].name`)              |
| `include_settings` | no       | Include reflection-based settings properties (default: `true`)     |

## Output

```json
{
  "name": "SurfaceSampler_0",
  "display_name": "SurfaceSampler",
  "graph": "PCG_LandscapeScatter",
  "position": { "x": 200, "y": 0 },
  "has_inbound_edges": true,
  "inbound_edge_count": 1,
  "is_instance": false,
  "settings": {
    "class": "PCGSurfaceSamplerSettings",
    "type": "Sampler",
    "enabled": true,
    "debug": false,
    "uses_seed": true,
    "seed": 42,
    "additional_info": "",
    "tooltip": "Samples points on a surface",
    "has_dynamic_pins": false,
    "can_be_disabled": true,
    "overridable_params": [
      {
        "label": "PointsPerSquaredMeter",
        "property_path": "PointsPerSquaredMeter",
        "property_class": "PCGSurfaceSamplerSettings"
      }
    ],
    "properties": [
      {
        "name": "PointsPerSquaredMeter",
        "type": "float",
        "value": "0.100000",
        "category": "Settings"
      }
    ]
  },
  "input_pins": [
    {
      "label": "In",
      "allowed_types": "Point|Landscape",
      "connected": true,
      "edge_count": 1,
      "allow_multiple_data": false,
      "allow_multiple_connections": true,
      "is_output": false,
      "pin_status": "Normal",
      "connections": [
        { "node": "InputNode", "pin": "Out" }
      ]
    }
  ],
  "output_pins": [
    {
      "label": "Out",
      "allowed_types": "Point",
      "connected": true,
      "edge_count": 1,
      "allow_multiple_data": true,
      "allow_multiple_connections": true,
      "is_output": true,
      "pin_status": "Normal",
      "connections": [
        { "node": "StaticMeshSpawner_0", "pin": "In" }
      ]
    }
  ]
}
```

### Top-level fields

| Field                | Type    | Description                                            |
| -------------------- | ------- | ------------------------------------------------------ |
| `name`               | string  | Internal node name                                     |
| `display_name`       | string  | Human-readable display name                            |
| `graph`              | string  | Name of the owning graph                               |
| `authored_title`     | string  | User-set title (if any)                                |
| `comment`            | string  | Node comment (if any)                                  |
| `position`           | object  | `{x, y}` editor position                              |
| `has_inbound_edges`  | boolean | Whether the node has incoming edges                    |
| `inbound_edge_count` | number  | Count of inbound edges                                 |
| `is_instance`        | boolean | Whether node uses an instance of settings              |

### Settings fields

| Field                 | Type    | Description                                           |
| --------------------- | ------- | ----------------------------------------------------- |
| `class`               | string  | Settings UClass name                                  |
| `type`                | string  | Node type (Spatial, Sampler, Spawner, Filter, etc.)   |
| `enabled`             | boolean | Whether the node is enabled                           |
| `debug`               | boolean | Whether debug mode is on                              |
| `uses_seed`           | boolean | Whether the node uses a random seed                   |
| `seed`                | number  | Seed value (only if `uses_seed` is true)              |
| `additional_info`     | string  | Extra context (e.g. referenced asset name)            |
| `tooltip`             | string  | Node description tooltip                              |
| `has_dynamic_pins`    | boolean | Whether pins can change based on input                |
| `can_be_disabled`     | boolean | Whether the node can be disabled                      |
| `overridable_params`  | array   | Parameters that can be overridden via graph params    |
| `properties`          | array   | Reflection-based settings properties (subclass-specific) |

### Pin fields (detailed)

| Field                       | Type    | Description                                   |
| --------------------------- | ------- | --------------------------------------------- |
| `label`                     | string  | Pin label/name                                |
| `allowed_types`             | string  | Allowed data types (e.g. `Point\|Landscape`)  |
| `connected`                 | boolean | Whether the pin has connections               |
| `edge_count`                | number  | Number of connected edges                     |
| `allow_multiple_data`       | boolean | Whether pin accepts multiple data items       |
| `allow_multiple_connections`| boolean | Whether pin allows multiple edges             |
| `is_output`                 | boolean | Whether this is an output pin                 |
| `pin_status`                | string  | `Normal`, `Required`, or `Advanced`           |
| `tooltip`                   | string  | Pin tooltip (if set)                          |
| `connections`               | array   | Connected `{node, pin}` pairs (when connected)|

## Examples

### Full node inspection

```json
{"asset_path": "/Game/PCG/MyGraph", "node_name": "SurfaceSampler_0"}
```

### Without settings properties

```json
{"asset_path": "/Game/PCG/MyGraph", "node_name": "SurfaceSampler_0", "include_settings": false}
```

## Errors

| Condition           | `isError` | Message                                    |
| ------------------- | --------- | ------------------------------------------ |
| Missing asset_path  | `true`    | `asset_path is required`                   |
| Missing node_name   | `true`    | `node_name is required`                    |
| Graph not found     | `true`    | `PCG graph not found: <path>`              |
| Node not found      | `true`    | `Node not found: <name>`                   |
