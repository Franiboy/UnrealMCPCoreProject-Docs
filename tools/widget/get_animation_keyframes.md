# get_animation_keyframes

Get all keyframes of a property track in a widget animation. Returns time, value, and interpolation mode for each keyframe.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "animation_name": "FadeIn",
  "widget_name": "MyImage",
  "property_name": "RenderOpacity"
}
```

| Parameter        | Required | Description                                              |
| ---------------- | -------- | -------------------------------------------------------- |
| `asset_path`     | **yes**  | Asset path of the Widget Blueprint.                      |
| `animation_name` | **yes**  | Name of the animation.                                   |
| `widget_name`    | **yes**  | Name of the widget.                                      |
| `property_name`  | **yes**  | Property name of the track (e.g. `RenderOpacity`).       |

## Output

```json
{
  "animation_name": "FadeIn",
  "widget_name": "MyImage",
  "property_name": "RenderOpacity",
  "count": 2,
  "keyframes": [
    {
      "time": 0.0,
      "frame": 0,
      "value": 0.0,
      "interpolation": "linear"
    },
    {
      "time": 2.0,
      "frame": 48000,
      "value": 1.0,
      "interpolation": "linear"
    }
  ],
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field                          | Type   | Description                            |
| ------------------------------ | ------ | -------------------------------------- |
| `animation_name`               | string | Animation name                         |
| `widget_name`                  | string | Widget name                            |
| `property_name`                | string | Animated property                      |
| `count`                        | number | Number of keyframes                    |
| `keyframes[].time`             | number | Time in seconds                        |
| `keyframes[].frame`            | number | Frame number (tick resolution)         |
| `keyframes[].value`            | number | Float value at this keyframe           |
| `keyframes[].interpolation`    | string | Interpolation mode (`linear`, `cubic`, `constant`) |
| `asset_path`                   | string | Widget Blueprint asset path            |

## Errors

| Condition                | `isError` | Message                                          |
| ------------------------ | --------- | ------------------------------------------------ |
| Missing required param   | `true`    | `Missing required parameter '...'`               |
| Animation not found      | `true`    | `Animation '...' not found in '...'`             |
| No binding for widget    | `true`    | `No binding for widget '...' in animation`       |
| No track for property    | `true`    | `No track for property '...' on widget '...'`    |
| Asset not found          | `true`    | `WidgetBlueprint not found: '...'`               |
