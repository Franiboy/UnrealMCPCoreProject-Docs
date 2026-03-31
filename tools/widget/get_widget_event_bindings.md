# get_widget_event_bindings

List event bindings in a Widget Blueprint. Shows which widgets have which events available and whether they are currently bound.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "MyButton"
}
```

| Parameter     | Required | Description                                                 |
| ------------- | -------- | ----------------------------------------------------------- |
| `asset_path`  | **yes**  | Asset path of the Widget Blueprint.                         |
| `widget_name` | no       | Only show bindings for this widget. If omitted, show all.   |

## Output

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_count": 1,
  "widgets": [
    {
      "widget_name": "MyButton",
      "widget_class": "Button",
      "event_count": 3,
      "events": [
        {
          "event_name": "OnClicked",
          "is_bound": true,
          "parameters": []
        },
        {
          "event_name": "OnHovered",
          "is_bound": false,
          "parameters": []
        }
      ]
    }
  ]
}
```

| Field                          | Type    | Description                               |
| ------------------------------ | ------- | ----------------------------------------- |
| `asset_path`                   | string  | Widget Blueprint asset path               |
| `widget_count`                 | number  | Number of widgets with bindable events    |
| `widgets[].widget_name`        | string  | Widget name                               |
| `widgets[].events[].event_name`| string  | Delegate property name                    |
| `widgets[].events[].is_bound`  | boolean | Whether the event is currently bound      |
| `widgets[].events[].parameters`| array   | Delegate signature parameters             |

## Errors

| Condition              | `isError` | Message                           |
| ---------------------- | --------- | --------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter 'asset_path'` |
| Asset not found        | `true`    | `Widget Blueprint not found at '...'`     |
