# remove_event_binding

Remove an event binding from a widget in a Widget Blueprint. Also removes the corresponding handler function node from the EventGraph if present.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "MyButton",
  "event_name": "OnClicked"
}
```

| Parameter     | Required | Description                                      |
| ------------- | -------- | ------------------------------------------------ |
| `asset_path`  | **yes**  | Asset path of the Widget Blueprint.              |
| `widget_name` | **yes**  | Name of the widget to remove the binding from.   |
| `event_name`  | **yes**  | Multicast delegate event to unbind.              |

## Output

```json
{
  "widget_name": "MyButton",
  "event_name": "OnClicked",
  "removed_function": "On_MyButton_OnClicked",
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field              | Type   | Description                            |
| ------------------ | ------ | -------------------------------------- |
| `widget_name`      | string | Widget the binding was removed from    |
| `event_name`       | string | Delegate event name                    |
| `removed_function` | string | Name of the removed handler function   |
| `asset_path`       | string | Widget Blueprint asset path            |

## Errors

| Condition                | `isError` | Message                                         |
| ------------------------ | --------- | ----------------------------------------------- |
| Missing `asset_path`     | `true`    | `Missing required parameter 'asset_path'`       |
| Missing `widget_name`    | `true`    | `Missing required parameter 'widget_name'`      |
| Missing `event_name`     | `true`    | `Missing required parameter 'event_name'`       |
| No binding found         | `true`    | `No binding found for event '...' on widget '...'` |
| Widget not found         | `true`    | `Widget '...' not found in '...'`               |
| Asset not found          | `true`    | `Widget Blueprint not found at '...'`           |
