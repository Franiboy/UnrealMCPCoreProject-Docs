# add_event_binding

Create a new event binding for a widget. Generates a Blueprint event handler function bound to the specified widget event (e.g. OnClicked, OnHovered).

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "MyButton",
  "event_name": "OnClicked",
  "handler_function_name": "HandleButtonClick"
}
```

| Parameter               | Required | Description                                                           |
| ----------------------- | -------- | --------------------------------------------------------------------- |
| `asset_path`            | **yes**  | Asset path of the Widget Blueprint.                                   |
| `widget_name`           | **yes**  | Name of the widget to bind an event on.                               |
| `event_name`            | **yes**  | Multicast delegate event name (e.g. `OnClicked`, `OnHovered`).        |
| `handler_function_name` | no       | Custom name for the handler function (default: auto-generated).       |

## Output

```json
{
  "widget_name": "MyButton",
  "event_name": "OnClicked",
  "handler_function": "HandleButtonClick",
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field              | Type   | Description                            |
| ------------------ | ------ | -------------------------------------- |
| `widget_name`      | string | Widget the event was bound on          |
| `event_name`       | string | Delegate event name                    |
| `handler_function` | string | Name of the generated handler function |
| `asset_path`       | string | Widget Blueprint asset path            |

## Examples

### Bind a button click

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "widget_name": "StartButton",
  "event_name": "OnClicked"
}
```

### Bind with custom handler name

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "widget_name": "StartButton",
  "event_name": "OnHovered",
  "handler_function_name": "OnStartHovered"
}
```

## Errors

| Condition                | `isError` | Message                                         |
| ------------------------ | --------- | ----------------------------------------------- |
| Missing `asset_path`     | `true`    | `Missing required parameter 'asset_path'`       |
| Missing `widget_name`    | `true`    | `Missing required parameter 'widget_name'`      |
| Missing `event_name`     | `true`    | `Missing required parameter 'event_name'`       |
| Widget not found         | `true`    | `Widget '...' not found in '...'`               |
| Event not found          | `true`    | `Event '...' not found on widget '...'`         |
| Asset not found          | `true`    | `Widget Blueprint not found at '...'`           |
