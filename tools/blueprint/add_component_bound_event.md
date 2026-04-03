# add_component_bound_event

Add a component-bound event node (`K2Node_ComponentBoundEvent`) to a Blueprint graph. This creates the event handler node for component delegates like `OnClicked`, `OnComponentBeginOverlap`, `OnComponentEndOverlap`, etc.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "component_name": "BoxCollision",
  "delegate_name": "OnComponentBeginOverlap"
}
```

| Parameter        | Required | Description                                                                        |
| ---------------- | -------- | ---------------------------------------------------------------------------------- |
| `asset_path`     | yes      | Blueprint asset path (e.g. `/Game/Blueprints/BP_MyActor`)                          |
| `component_name` | yes      | Name of the component in the Blueprint (e.g. `BoxCollision`, `StaticMesh`)         |
| `delegate_name`  | yes      | Name of the multicast delegate on the component class (e.g. `OnComponentBeginOverlap`) |
| `graph_name`     | no       | Target graph name (default: `EventGraph`)                                          |
| `position_x`     | no       | X position in the graph editor (default: 0)                                        |
| `position_y`     | no       | Y position in the graph editor (default: 0)                                        |

## Output

```json
{
  "asset_path": "/Game/BP_MyActor",
  "graph_name": "EventGraph",
  "node_id": "K2Node_ComponentBoundEvent_0",
  "node_class": "K2Node_ComponentBoundEvent",
  "component_name": "BoxCollision",
  "delegate_name": "OnComponentBeginOverlap",
  "pins": [
    {"name": "then", "direction": "output", "type": "exec"},
    {"name": "OverlappedComponent", "direction": "output", "type": "UPrimitiveComponent*"},
    {"name": "OtherActor", "direction": "output", "type": "AActor*"}
  ]
}
```

## Errors

| Condition               | Error Message                                                  |
| ----------------------- | -------------------------------------------------------------- |
| Missing required params | Missing required parameters: asset_path, component_name, and delegate_name |
| Blueprint not found     | Blueprint not found at '...'                                   |
| Component not found     | Component property '...' not found on '...'                    |
| Delegate not found      | Delegate '...' not found on component class '...'              |
| Graph not found         | Graph '...' not found in '...'                                 |

## Notes

- The component must exist in the Blueprint's component tree. Use `get_blueprint` to discover component names.
- The delegate must be `BlueprintAssignable` on the component's class.
- Output pins are automatically generated to match the delegate's parameter signature.
- The Blueprint is compiled after adding the node.
- For class-level delegates (not component-bound), use `add_delegate_binding` instead.
