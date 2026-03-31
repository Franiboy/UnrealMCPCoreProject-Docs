# add_pcg_component

Add a PCG Component to an actor in the current level. If the actor already has a PCG Component, the existing component is updated with the specified graph instead of creating a duplicate.

## Input

```json
{
  "actor_name": "LandscapeActor",
  "graph_path": "/Game/PCG/PCG_ForestScatter",
  "component_name": "ForestPCG",
  "generation_trigger": "GenerateOnLoad"
}
```

| Parameter            | Required | Description                                                       |
| -------------------- | -------- | ----------------------------------------------------------------- |
| `actor_name`         | **yes**  | Actor name or label in the current level                          |
| `graph_path`         | no       | Asset path to a PCG Graph to assign to the component              |
| `component_name`     | no       | Name for the new component (default: `PCGComponent`)              |
| `generation_trigger` | no       | `GenerateOnLoad` or `GenerateOnDemand` (only for new components)  |

## Output

```json
{
  "actor_name": "LandscapeActor",
  "actor_label": "LandscapeActor",
  "component_name": "ForestPCG",
  "created_new": true,
  "updated_existing": false,
  "graph_name": "PCG_ForestScatter",
  "graph_path": "/Game/PCG/PCG_ForestScatter.PCG_ForestScatter",
  "generation_trigger": "GenerateOnLoad"
}
```

| Field                | Type    | Description                                                |
| -------------------- | ------- | ---------------------------------------------------------- |
| `actor_name`         | string  | Internal actor name                                        |
| `actor_label`        | string  | Editor display label                                       |
| `component_name`     | string  | Name of the PCG component                                  |
| `created_new`        | boolean | `true` if a new component was created                      |
| `updated_existing`   | boolean | `true` if an existing component was updated                |
| `graph_name`         | string  | Graph asset name (when a graph is assigned)                |
| `graph_path`         | string  | Full graph asset path (when a graph is assigned)           |
| `generation_trigger` | string  | Generation trigger mode (only for new components)          |

## Examples

### Add component with a graph

```json
{"actor_name": "MyActor", "graph_path": "/Game/PCG/PCG_Scatter"}
```

### Add component without a graph

```json
{"actor_name": "MyActor"}
```

### Update existing component with a new graph

```json
{"actor_name": "MyActor", "graph_path": "/Game/PCG/PCG_NewGraph"}
```

## Errors

| Condition            | `isError` | Message                                |
| -------------------- | --------- | -------------------------------------- |
| Missing/empty actor  | `true`    | `actor_name is required`               |
| Actor not found      | `true`    | `Actor not found: <name>`              |
| Graph not found      | `true`    | `PCG graph not found: <path>`          |
| No editor world      | `true`    | `No editor world available`            |
