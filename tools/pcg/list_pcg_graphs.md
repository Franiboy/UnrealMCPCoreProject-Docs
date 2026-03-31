# list_pcg_graphs

List PCG (Procedural Content Generation) graph assets in the project. Returns name, asset path, node count, and generation settings for each graph.

## Input

```json
{
  "path_prefix": "/Game/PCG",
  "name_filter": "Landscape",
  "include_engine": false
}
```

| Parameter        | Required | Description                                                          |
| ---------------- | -------- | -------------------------------------------------------------------- |
| `path_prefix`    | no       | Content path prefix to search. Default: `/Game`.                     |
| `name_filter`    | no       | Case-insensitive substring filter on the graph name.                 |
| `include_engine` | no       | Include engine/plugin graphs (default: `false`).                     |

## Output

```json
{
  "total_count": 2,
  "graphs": [
    {
      "name": "PCG_LandscapeScatter",
      "asset_path": "/Game/PCG/PCG_LandscapeScatter.PCG_LandscapeScatter",
      "node_count": 8,
      "hierarchical_generation": false,
      "use_2d_grid": true
    },
    {
      "name": "PCG_ForestGenerator",
      "asset_path": "/Game/PCG/PCG_ForestGenerator.PCG_ForestGenerator",
      "node_count": 12,
      "hierarchical_generation": true,
      "use_2d_grid": false
    }
  ]
}
```

| Field                      | Type    | Description                                          |
| -------------------------- | ------- | ---------------------------------------------------- |
| `total_count`              | number  | Number of matching graphs                            |
| `graphs[].name`            | string  | Graph asset name                                     |
| `graphs[].asset_path`      | string  | Full asset path                                      |
| `graphs[].node_count`      | number  | Number of nodes in the graph                         |
| `graphs[].hierarchical_generation` | boolean | Whether hierarchical generation is enabled    |
| `graphs[].use_2d_grid`     | boolean | Whether the graph uses a 2D grid                     |

## Examples

### List all PCG graphs in the project

```json
{}
```

### Search by name

```json
{"name_filter": "Forest", "path_prefix": "/Game/PCG"}
```

### Include engine graphs

```json
{"include_engine": true}
```

## Errors

No tool-level errors. Returns `total_count: 0` with an empty `graphs` array when no matches are found.
