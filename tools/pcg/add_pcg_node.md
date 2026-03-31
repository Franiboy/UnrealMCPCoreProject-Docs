# add_pcg_node

Add a node to a PCG Graph by settings class name. Returns the new node name, type, and available pins.

## Input

```json
{
  "asset_path": "/Game/PCG/MyGraph",
  "settings_class": "PCGSurfaceSamplerSettings",
  "node_title": "TerrainSampler",
  "position_x": 300,
  "position_y": 100,
  "comment": "Samples terrain surface"
}
```

| Parameter        | Required | Description                                                                  |
| ---------------- | -------- | ---------------------------------------------------------------------------- |
| `asset_path`     | **yes**  | Asset path of the PCG graph                                                  |
| `settings_class` | **yes**  | Settings class name (e.g. `PCGSurfaceSamplerSettings` or short `SurfaceSampler`) |
| `node_title`     | no       | Custom node title                                                            |
| `position_x`     | no       | Editor X position                                                            |
| `position_y`     | no       | Editor Y position                                                            |
| `comment`        | no       | Node comment                                                                 |

### Class name resolution

The `settings_class` parameter supports flexible naming:

- **Full name**: `PCGSurfaceSamplerSettings`
- **With U prefix**: `UPCGSurfaceSamplerSettings`
- **Short name**: `SurfaceSampler` (automatically maps to `PCGSurfaceSamplerSettings`)

## Output

```json
{
  "node_name": "PCGNode_0",
  "settings_class": "PCGSurfaceSamplerSettings",
  "graph": "MyGraph",
  "asset_path": "/Game/PCG/MyGraph",
  "node_type": "Sampler",
  "input_pins": [
    { "label": "In" }
  ],
  "output_pins": [
    { "label": "Out" }
  ]
}
```

| Field            | Type   | Description                                    |
| ---------------- | ------ | ---------------------------------------------- |
| `node_name`      | string | Auto-generated node name (use for other tools) |
| `settings_class` | string | Resolved settings class name                   |
| `graph`          | string | Graph asset name                               |
| `asset_path`     | string | Graph asset path                               |
| `node_type`      | string | Node type (Sampler, Spawner, Filter, etc.)     |
| `input_pins`     | array  | Available input pins with labels               |
| `output_pins`    | array  | Available output pins with labels              |

## Examples

### Add a surface sampler

```json
{"asset_path": "/Game/PCG/MyGraph", "settings_class": "SurfaceSampler"}
```

### Add a static mesh spawner with position

```json
{"asset_path": "/Game/PCG/MyGraph", "settings_class": "PCGStaticMeshSpawnerSettings", "position_x": 500, "position_y": 0}
```

## Errors

| Condition              | `isError` | Message                                         |
| ---------------------- | --------- | ----------------------------------------------- |
| Missing asset_path     | `true`    | `asset_path is required`                        |
| Missing settings_class | `true`    | `settings_class is required`                    |
| Graph not found        | `true`    | `PCG graph not found: <path>`                   |
| Class not found        | `true`    | `PCG settings class not found: <name>`          |
