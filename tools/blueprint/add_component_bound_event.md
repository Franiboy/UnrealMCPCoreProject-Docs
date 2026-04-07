# add_component_bound_event

Add an event handler for a component or widget delegate in a Blueprint graph.

- **Actor Blueprints**: Creates a `K2Node_ComponentBoundEvent` node — the standard event node UE generates when you bind a component delegate like `OnComponentBeginOverlap`, `OnClicked`, etc.
- **Widget Blueprints**: Automatically uses the `FDelegateEditorBinding` approach — creates a `K2Node_CustomEvent` bound to the widget event (e.g. `OnClicked`, `OnHovered` on a Button). No extra steps needed; just pass the widget name as `component_name`.

## Input

```json
{
  "asset_path": "/Game/BP_MyActor",
  "component_name": "BoxCollision",
  "delegate_name": "OnComponentBeginOverlap"
}
```

**Widget Blueprint example:**

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "component_name": "PlayButton",
  "delegate_name": "OnClicked"
}
```

| Parameter        | Required | Description                                                                        |
| ---------------- | -------- | ---------------------------------------------------------------------------------- |
| `asset_path`     | yes      | Blueprint asset path (e.g. `/Game/Blueprints/BP_MyActor`)                          |
| `component_name` | yes      | Name of the component or widget (e.g. `BoxCollision`, `PlayButton`)                |
| `delegate_name`  | yes      | Name of the multicast delegate (e.g. `OnComponentBeginOverlap`, `OnClicked`)       |
| `graph_name`     | no       | Target graph name (default: `EventGraph`)                                          |
| `position_x`     | no       | X position in the graph editor (default: 0)                                        |
| `position_y`     | no       | Y position in the graph editor (default: 0)                                        |

**Parameter aliases:** `widget_name` → `component_name`, `event_name` → `delegate_name`

## Output (Actor Blueprint)

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

## Output (Widget Blueprint)

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "graph_name": "EventGraph",
  "node_id": "K2Node_CustomEvent_0",
  "node_class": "K2Node_CustomEvent",
  "widget_name": "PlayButton",
  "delegate_name": "OnClicked",
  "handler_function": "On_PlayButton_OnClicked",
  "pins": [
    {"name": "then", "direction": "output", "type": "exec"}
  ]
}
```

## Errors

| Condition               | Error Message                                                  |
| ----------------------- | -------------------------------------------------------------- |
| Missing required params | Missing required parameters: asset_path, component_name, and delegate_name |
| Blueprint not found     | Blueprint not found at '...'                                   |
| Component not found     | Component property '...' not found on '...'                    |
| Widget not found        | Widget '...' not found in Widget Blueprint '...'               |
| Delegate not found      | Delegate '...' not found on component/widget class '...'       |
| Graph not found         | Graph '...' not found in '...'                                 |
| Duplicate binding       | A binding for '...' already exists                             |

## Notes

- For **Actor Blueprints**: the component must exist in the Blueprint's component tree. Use `get_blueprint` to discover component names. The delegate must be `BlueprintAssignable`.
- For **Widget Blueprints**: the widget must exist in the widget tree. Use `get_widget_event_bindings` to discover available widgets and their events. The handler function is auto-generated as `On_{widget}_{event}`.
- Output pins are automatically generated to match the delegate's parameter signature.
- For class-level delegates (not component-bound), use `add_delegate_binding` instead.
