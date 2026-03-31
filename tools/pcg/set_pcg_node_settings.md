# set_pcg_node_settings

Set settings/properties on a PCG node via reflection. Use `get_pcg_node` to discover available property names and current values.

## Input

```json
{
  "asset_path": "/Game/PCG/MyGraph",
  "node_name": "PCGNode_0",
  "properties": {
    "PointsPerSquaredMeter": 0.5,
    "Seed": 42,
    "bEnabled": true
  }
}
```

| Parameter    | Required | Description                                                         |
| ------------ | -------- | ------------------------------------------------------------------- |
| `asset_path` | **yes**  | Asset path of the PCG graph                                        |
| `node_name`  | **yes**  | Name of the node to modify                                         |
| `properties` | **yes**  | Object mapping property names to values                            |

### Property name resolution

- Exact property names from `get_pcg_node` (e.g. `PointsPerSquaredMeter`, `bEnabled`, `Seed`)
- Common aliases: `enabled` resolves to `bEnabled`, `debug` resolves to `bDebug`

### Value formats

- **Numbers**: `0.5`, `42`
- **Booleans**: `true`, `false`
- **Strings**: `"MyValue"`
- **Vectors**: `"(X=1,Y=2,Z=3)"` (UE text format)

## Output

```json
{
  "node_name": "PCGNode_0",
  "graph": "MyGraph",
  "settings_class": "PCGSurfaceSamplerSettings",
  "success_count": 2,
  "fail_count": 1,
  "results": [
    { "property": "PointsPerSquaredMeter", "success": true, "value": "0.500000" },
    { "property": "Seed", "success": true, "value": "42" },
    { "property": "NonExistent", "success": false, "error": "Property not found: NonExistent" }
  ]
}
```

| Field            | Type   | Description                                      |
| ---------------- | ------ | ------------------------------------------------ |
| `node_name`      | string | Node name                                        |
| `graph`          | string | Graph asset name                                 |
| `settings_class` | string | Settings class name                              |
| `success_count`  | number | Number of properties set successfully            |
| `fail_count`     | number | Number of properties that failed                 |
| `results`        | array  | Per-property results with success/failure detail |

The tool returns `isError: true` only if **all** properties fail. Partial success is not an error.

## Examples

### Set sampler density

```json
{"asset_path": "/Game/PCG/MyGraph", "node_name": "PCGNode_0", "properties": {"PointsPerSquaredMeter": 0.1}}
```

### Disable a node and set seed

```json
{"asset_path": "/Game/PCG/MyGraph", "node_name": "PCGNode_0", "properties": {"bEnabled": false, "Seed": 12345}}
```

## Errors

| Condition           | `isError` | Message                                                        |
| ------------------- | --------- | -------------------------------------------------------------- |
| Missing asset_path  | `true`    | `asset_path is required`                                       |
| Missing node_name   | `true`    | `node_name is required`                                        |
| Missing properties  | `true`    | `properties is required (object mapping property names to values)` |
| Graph not found     | `true`    | `PCG graph not found: <path>`                                  |
| Node not found      | `true`    | `Node not found: <name>`                                       |
| No settings on node | `true`    | `Node <name> has no settings`                                  |
