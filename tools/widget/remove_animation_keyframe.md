# remove_animation_keyframe

Remove a keyframe from a property track in a widget animation at a specific time.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "animation_name": "FadeIn",
  "widget_name": "MyImage",
  "property_name": "RenderOpacity",
  "time": 1.0
}
```

| Parameter        | Required | Description                                              |
| ---------------- | -------- | -------------------------------------------------------- |
| `asset_path`     | **yes**  | Asset path of the Widget Blueprint.                      |
| `animation_name` | **yes**  | Name of the animation.                                   |
| `widget_name`    | **yes**  | Name of the widget.                                      |
| `property_name`  | **yes**  | Property name of the track (e.g. `RenderOpacity`).       |
| `time`           | **yes**  | Time in seconds of the keyframe to remove.               |

## Output

```json
{
  "animation_name": "FadeIn",
  "widget_name": "MyImage",
  "property_name": "RenderOpacity",
  "removed_time": 1.0,
  "removed_frame": 24000,
  "removed_value": 0.5,
  "remaining_keyframes": 2,
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field                 | Type   | Description                            |
| --------------------- | ------ | -------------------------------------- |
| `animation_name`      | string | Animation name                         |
| `widget_name`         | string | Widget name                            |
| `property_name`       | string | Animated property                      |
| `removed_time`        | number | Time in seconds of the removed key     |
| `removed_frame`       | number | Frame number of the removed key        |
| `removed_value`       | number | Value that was at the removed key      |
| `remaining_keyframes` | number | Number of keyframes remaining          |
| `asset_path`          | string | Widget Blueprint asset path            |

## Errors

| Condition                | `isError` | Message                                                   |
| ------------------------ | --------- | --------------------------------------------------------- |
| Missing required param   | `true`    | `Missing required parameter '...'`                        |
| No keyframe at time      | `true`    | `No keyframe at time ...s (frame ...) on track '...'`     |
| Animation not found      | `true`    | `Animation '...' not found in '...'`                      |
| No binding for widget    | `true`    | `No binding for widget '...' in animation`                |
| No track for property    | `true`    | `No track for property '...' on widget '...'`             |
| Asset not found          | `true`    | `WidgetBlueprint not found: '...'`                        |
