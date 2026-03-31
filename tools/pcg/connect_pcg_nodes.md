# connect_pcg_nodes

Wire two PCG nodes together by connecting an output pin to an input pin.

## Input

```json
{
  "asset_path": "/Game/PCG/MyGraph",
  "from_node": "PCGNode_0",
  "from_pin": "Out",
  "to_node": "PCGNode_1",
  "to_pin": "In"
}
```

| Parameter    | Required | Description                                      |
| ------------ | -------- | ------------------------------------------------ |
| `asset_path` | **yes**  | Asset path of the PCG graph                      |
| `from_node`  | **yes**  | Source node name                                 |
| `from_pin`   | no       | Output pin label on source node (default: `Out`) |
| `to_node`    | **yes**  | Target node name                                 |
| `to_pin`     | no       | Input pin label on target node (default: `In`)   |

## Output

```json
{
  "connected": true,
  "from_node": "PCGNode_0",
  "from_pin": "Out",
  "to_node": "PCGNode_1",
  "to_pin": "In",
  "graph": "MyGraph"
}
```

| Field       | Type    | Description                            |
| ----------- | ------- | -------------------------------------- |
| `connected` | boolean | Whether the connection was established |
| `from_node` | string  | Source node name                       |
| `from_pin`  | string  | Output pin label used                  |
| `to_node`   | string  | Target node name                       |
| `to_pin`    | string  | Input pin label used                   |
| `graph`     | string  | Graph asset name                       |

## Examples

### Connect with default pin names

```json
{"asset_path": "/Game/PCG/MyGraph", "from_node": "PCGNode_0", "to_node": "PCGNode_1"}
```

### Connect specific pins

```json
{"asset_path": "/Game/PCG/MyGraph", "from_node": "InputNode", "from_pin": "Out", "to_node": "PCGNode_0", "to_pin": "In"}
```

## Errors

| Condition           | `isError` | Message                               |
| ------------------- | --------- | ------------------------------------- |
| Missing asset_path  | `true`    | `asset_path is required`              |
| Missing from_node   | `true`    | `from_node is required`               |
| Missing to_node     | `true`    | `to_node is required`                 |
| Graph not found     | `true`    | `PCG graph not found: <path>`         |
| Source node missing | `true`    | `Source node not found: <name>`       |
| Target node missing | `true`    | `Target node not found: <name>`       |
