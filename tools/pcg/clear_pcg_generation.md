# clear_pcg_generation

Clear/cleanup all generated content from a PCG component on an actor. Removes spawned meshes, instances, and other generated resources immediately (synchronous).

## Input

```json
{
  "actor_name": "PCGScatterActor",
  "remove_components": true
}
```

| Parameter           | Required | Description                                                    |
| ------------------- | -------- | -------------------------------------------------------------- |
| `actor_name`        | **yes**  | Actor name or label in the current level                       |
| `remove_components` | no       | Remove generated components (default: `true`)                  |

## Output

```json
{
  "actor_name": "PCGScatterActor",
  "actor_label": "PCGScatterActor",
  "cleaned": true,
  "remove_components": true,
  "is_generating": false,
  "is_cleaning_up": false,
  "remaining_output_count": 0
}
```

| Field                    | Type    | Description                                             |
| ------------------------ | ------- | ------------------------------------------------------- |
| `actor_name`             | string  | Internal actor name                                     |
| `actor_label`            | string  | Editor display label                                    |
| `cleaned`                | boolean | Always `true` on success                                |
| `remove_components`      | boolean | Whether generated components were removed               |
| `is_generating`          | boolean | Whether the component is still generating after cleanup |
| `is_cleaning_up`         | boolean | Whether the component is still cleaning up              |
| `remaining_output_count` | number  | Number of remaining tagged data items in graph output   |

## Examples

### Clear all generated content

```json
{"actor_name": "MyPCGActor"}
```

### Clear without removing components

```json
{"actor_name": "MyPCGActor", "remove_components": false}
```

## Errors

| Condition                | `isError` | Message                                   |
| ------------------------ | --------- | ----------------------------------------- |
| Missing/empty actor      | `true`    | `actor_name is required`                  |
| Actor not found          | `true`    | `Actor not found: <name>`                 |
| No PCG component         | `true`    | `Actor '<name>' has no PCG component`     |
| No editor world          | `true`    | `No editor world available`               |
