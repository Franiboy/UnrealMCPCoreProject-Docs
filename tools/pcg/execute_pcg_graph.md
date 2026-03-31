# execute_pcg_graph

Trigger PCG graph generation on an actor's PCG component. Generation is asynchronous — use `get_pcg_component_info` to poll status.

## Input

```json
{
  "actor_name": "PCGScatterActor",
  "force": true
}
```

| Parameter    | Required | Description                                                           |
| ------------ | -------- | --------------------------------------------------------------------- |
| `actor_name` | **yes**  | Actor name or label in the current level                              |
| `force`      | no       | Force regeneration even if already generated (default: `true`)        |

## Output

```json
{
  "actor_name": "PCGScatterActor",
  "actor_label": "PCGScatterActor",
  "generation_triggered": true,
  "was_already_generating": false,
  "force": true,
  "is_generating": true,
  "graph_name": "PCG_ForestScatter",
  "graph_path": "/Game/PCG/PCG_ForestScatter.PCG_ForestScatter",
  "generation_trigger": "GenerateOnDemand"
}
```

| Field                   | Type    | Description                                                |
| ----------------------- | ------- | ---------------------------------------------------------- |
| `actor_name`            | string  | Internal actor name                                        |
| `actor_label`           | string  | Editor display label                                       |
| `generation_triggered`  | boolean | Always `true` on success                                   |
| `was_already_generating`| boolean | `true` if the component was already generating             |
| `force`                 | boolean | Whether force-regeneration was requested                   |
| `is_generating`         | boolean | Whether the component is generating after the call         |
| `graph_name`            | string  | Graph asset name (if a graph is assigned)                  |
| `graph_path`            | string  | Full graph asset path (if a graph is assigned)             |
| `generation_trigger`    | string  | Generation trigger mode (`GenerateOnLoad`, `GenerateOnDemand`, `GenerateAtRuntime`) |

## Examples

### Trigger generation (default force)

```json
{"actor_name": "MyPCGActor"}
```

### Trigger without forcing

```json
{"actor_name": "MyPCGActor", "force": false}
```

## Errors

| Condition                | `isError` | Message                                   |
| ------------------------ | --------- | ----------------------------------------- |
| Missing/empty actor      | `true`    | `actor_name is required`                  |
| Actor not found          | `true`    | `Actor not found: <name>`                 |
| No PCG component         | `true`    | `Actor '<name>' has no PCG component`     |
| No editor world          | `true`    | `No editor world available`               |
