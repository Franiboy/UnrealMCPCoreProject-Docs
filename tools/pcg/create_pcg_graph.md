# create_pcg_graph

Create a new PCG Graph asset. The graph is saved to disk and registered with the Asset Registry.

## Input

```json
{
  "name": "PCG_MyForest",
  "path": "/Game/PCG"
}
```

| Parameter | Required | Description                                    |
| --------- | -------- | ---------------------------------------------- |
| `name`    | **yes**  | Name for the new PCG graph asset               |
| `path`    | no       | Folder path (default: `/Game`)                 |

## Output

```json
{
  "name": "PCG_MyForest",
  "asset_path": "/Game/PCG/PCG_MyForest",
  "class": "PCGGraph",
  "node_count": 2,
  "hierarchical_generation": false,
  "use_2d_grid": false
}
```

| Field                    | Type    | Description                                          |
| ------------------------ | ------- | ---------------------------------------------------- |
| `name`                   | string  | Asset name                                           |
| `asset_path`             | string  | Full asset path                                      |
| `class`                  | string  | Always `PCGGraph`                                    |
| `node_count`             | number  | Number of nodes in the newly created graph           |
| `hierarchical_generation`| boolean | Whether hierarchical generation is enabled           |
| `use_2d_grid`            | boolean | Whether the graph uses a 2D grid                     |

## Examples

### Create with defaults

```json
{"name": "PCG_Scatter"}
```

### Create in a specific folder

```json
{"name": "PCG_CityBlocks", "path": "/Game/PCG/City"}
```

## Errors

| Condition                 | `isError` | Message                                          |
| ------------------------- | --------- | ------------------------------------------------ |
| Missing/empty name        | `true`    | `name is required`                               |
| Invalid path prefix       | `true`    | `Invalid path '...'. Must start with /Game or /Engine.` |
| Asset already exists      | `true`    | `An asset already exists at '...'`               |
| Package creation failed   | `true`    | `Failed to create package '...'`                 |
