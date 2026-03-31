# get_pcg_component_info

Get PCG component information from an actor in the current level. Returns graph reference, generation status, grid size, partition settings, and generation trigger.

## Input

```json
{
  "actor_name": "PCG_Volume_0"
}
```

| Parameter    | Required | Description                                     |
| ------------ | -------- | ----------------------------------------------- |
| `actor_name` | **yes**  | Actor name or label in the current level        |

## Output

```json
{
  "actor_name": "PCG_Volume_0",
  "actor_label": "PCG Volume",
  "actor_class": "APCGVolume",
  "has_pcg_component": true,
  "component": {
    "graph_name": "PCG_LandscapeScatter",
    "graph_path": "/Game/PCG/PCG_LandscapeScatter.PCG_LandscapeScatter",
    "graph_node_count": 8,
    "is_generating": false,
    "is_cleaning_up": false,
    "generation_grid_size": 25600,
    "can_partition": true,
    "generation_trigger": "GenerateOnLoad",
    "generated_output_count": 3
  }
}
```

### Top-level fields

| Field              | Type    | Description                                     |
| ------------------ | ------- | ----------------------------------------------- |
| `actor_name`       | string  | Internal actor name                             |
| `actor_label`      | string  | Editor display label                            |
| `actor_class`      | string  | Actor UClass name                               |
| `has_pcg_component`| boolean | Whether the actor has a PCG component           |

### Component fields (present when `has_pcg_component` is true)

| Field                    | Type   | Description                                          |
| ------------------------ | ------ | ---------------------------------------------------- |
| `graph_name`             | string | Name of the assigned PCG graph (or `"(none)"`)       |
| `graph_path`             | string | Full path of the assigned graph                      |
| `graph_node_count`       | number | Number of nodes in the assigned graph                |
| `graph_interface_class`  | string | Class name (if graph is not a plain UPCGGraph)       |
| `is_generating`          | boolean| Whether the component is currently generating        |
| `is_cleaning_up`         | boolean| Whether the component is currently cleaning up       |
| `generation_grid_size`   | number | Grid size used for partitioned generation            |
| `can_partition`          | boolean| Whether the component supports partitioning          |
| `generation_trigger`     | string | `GenerateOnLoad` or `GenerateOnDemand`               |
| `generated_output_count` | number | Number of tagged data outputs from last generation   |

## Examples

### Inspect a PCG actor

```json
{"actor_name": "PCG_Volume_0"}
```

### Inspect by label

```json
{"actor_name": "My PCG Spawner"}
```

## Errors

| Condition          | `isError` | Message                                    |
| ------------------ | --------- | ------------------------------------------ |
| Missing actor_name | `true`    | `actor_name is required`                   |
| Actor not found    | `true`    | `Actor not found: <name>`                  |
| No editor world    | `true`    | `No editor world available`                |
