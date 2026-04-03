# add_delegate_binding

High-level tool that creates a complete delegate binding in one call. Spawns three nodes — `K2Node_AddDelegate` (Bind Event), `K2Node_CreateDelegate`, and `K2Node_CustomEvent` — and wires them together. The custom event's signature automatically matches the delegate's parameters.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "target_class": "Actor",
  "delegate_name": "OnActorBeginOverlap",
  "handler_event_name": "HandleOverlap"
}
```

| Parameter            | Required | Description                                                                              |
| -------------------- | -------- | ---------------------------------------------------------------------------------------- |
| `asset_path`         | yes      | Blueprint asset path (e.g. `/Game/Blueprints/BP_MyActor`)                                |
| `target_class`       | yes      | Class owning the delegate (e.g. `Actor`, `PrimitiveComponent`, or full path `/Script/Engine.Actor`) |
| `delegate_name`      | yes      | Name of the multicast delegate property (e.g. `OnActorBeginOverlap`, `OnDestroyed`)     |
| `handler_event_name` | no       | Custom event name for the handler (default: `On_<delegate_name>`)                        |
| `graph_name`         | no       | Target graph name (default: `EventGraph`)                                                |
| `position_x`         | no       | X position of the node cluster (default: 0)                                              |
| `position_y`         | no       | Y position of the node cluster (default: 0)                                              |

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "graph_name": "EventGraph",
  "target_class": "Actor",
  "delegate_name": "OnActorBeginOverlap",
  "handler_event_name": "HandleOverlap",
  "wired": true,
  "add_delegate_node": {
    "node_id": "K2Node_AddDelegate_0",
    "title": "Bind Event to OnActorBeginOverlap",
    "pins": [...]
  },
  "create_delegate_node": {
    "node_id": "K2Node_CreateDelegate_0",
    "pins": [...]
  },
  "handler_event_node": {
    "node_id": "K2Node_CustomEvent_0",
    "event_name": "HandleOverlap",
    "pins": [...]
  }
}
```

| Field                  | Description                                                                 |
| ---------------------- | --------------------------------------------------------------------------- |
| `wired`                | Whether the CreateDelegate output was successfully wired to AddDelegate's delegate pin |
| `add_delegate_node`    | The "Bind Event to ..." node with its ID, title, and pins                  |
| `create_delegate_node` | The CreateDelegate bridge node with its ID and pins                        |
| `handler_event_node`   | The CustomEvent node whose signature matches the delegate                  |

## Node Layout

The three nodes are positioned relative to `position_x`/`position_y`:

- **AddDelegate** — at `(x+500, y)` — the "Bind Event" node
- **CreateDelegate** — at `(x+200, y)` — bridges the function to the delegate
- **CustomEvent** — at `(x, y+200)` — your handler function with matching parameters

## Errors

| Condition                 | Error Message                                                       |
| ------------------------- | ------------------------------------------------------------------- |
| Missing required params   | Missing required parameters: asset_path, target_class, and delegate_name |
| Class not found           | Target class '...' not found                                       |
| Delegate not found        | Delegate '...' not found on class '...'. Available: ...             |
| Blueprint not found       | Blueprint not found at '...'                                        |
| Graph not found           | Graph '...' not found in '...'                                      |

## Notes

- The delegate must be `BlueprintAssignable`. The error message lists all available delegates on the class if the specified one is not found.
- The custom event's output pins are automatically generated to match the delegate's parameter signature (e.g. `OtherActor` for `OnActorBeginOverlap`).
- For lower-level control, use `add_node` with `K2Node_AddDelegate`, `K2Node_CreateDelegate`, and `K2Node_CustomEvent` individually.
- The Blueprint is marked as modified but not compiled — call `compile_blueprint` separately if needed.
- Undo removes all three nodes.
