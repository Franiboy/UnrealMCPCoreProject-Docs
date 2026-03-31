# add_animation_keyframe

Add or update a keyframe on a property track in a widget animation. If a keyframe already exists at the given time, its value and interpolation mode are updated.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "animation_name": "FadeIn",
  "widget_name": "MyImage",
  "property_name": "RenderOpacity",
  "time": 0.0,
  "value": 0.0,
  "interpolation": "linear"
}
```

| Parameter        | Required | Description                                                  |
| ---------------- | -------- | ------------------------------------------------------------ |
| `asset_path`     | **yes**  | Asset path of the Widget Blueprint.                          |
| `animation_name` | **yes**  | Name of the animation.                                       |
| `widget_name`    | **yes**  | Name of the widget.                                          |
| `property_name`  | **yes**  | Property name of the track (e.g. `RenderOpacity`).           |
| `time`           | **yes**  | Time in seconds where the keyframe should be placed.         |
| `value`          | **yes**  | The float value for the keyframe.                            |
| `interpolation`  | no       | Interpolation mode: `linear`, `cubic` (default), `constant`. |

## Output

```json
{
  "animation_name": "FadeIn",
  "widget_name": "MyImage",
  "property_name": "RenderOpacity",
  "time": 0.0,
  "frame": 0,
  "value": 0.0,
  "interpolation": "linear",
  "updated_existing": false,
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field              | Type    | Description                                    |
| ------------------ | ------- | ---------------------------------------------- |
| `animation_name`   | string  | Animation name                                 |
| `widget_name`      | string  | Widget name                                    |
| `property_name`    | string  | Animated property                              |
| `time`             | number  | Time in seconds                                |
| `frame`            | number  | Frame number (tick resolution)                 |
| `value`            | number  | Keyframe value                                 |
| `interpolation`    | string  | Interpolation mode used                        |
| `updated_existing` | boolean | `true` if an existing key was updated          |
| `asset_path`       | string  | Widget Blueprint asset path                    |

## Examples

### Add a fade-in: opacity 0 at start, 1 at 2 seconds

```json
{"asset_path": "/Game/UI/WBP_HUD", "animation_name": "FadeIn", "widget_name": "Overlay", "property_name": "RenderOpacity", "time": 0.0, "value": 0.0, "interpolation": "linear"}
```
```json
{"asset_path": "/Game/UI/WBP_HUD", "animation_name": "FadeIn", "widget_name": "Overlay", "property_name": "RenderOpacity", "time": 2.0, "value": 1.0, "interpolation": "linear"}
```

## Errors

| Condition                | `isError` | Message                                          |
| ------------------------ | --------- | ------------------------------------------------ |
| Missing required param   | `true`    | `Missing required parameter '...'`               |
| Animation not found      | `true`    | `Animation '...' not found in '...'`             |
| No binding for widget    | `true`    | `No binding for widget '...' in animation`       |
| No track for property    | `true`    | `No track for property '...' on widget '...'`    |
| Asset not found          | `true`    | `WidgetBlueprint not found: '...'`               |
