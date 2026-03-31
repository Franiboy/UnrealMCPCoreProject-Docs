# add_animation_track

Add a property track to a widget animation. Binds a widget property (e.g. RenderOpacity, RenderTransform) to the animation timeline.

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
| `animation_name` | **yes**  | Name of the animation to add the track to.               |
| `widget_name`    | **yes**  | Name of the widget to animate.                           |
| `property_name`  | **yes**  | Property name to animate (e.g. `RenderOpacity`).         |

## Output

```json
{
  "animation_name": "FadeIn",
  "widget_name": "MyImage",
  "property_name": "RenderOpacity",
  "track_type": "FloatTrack",
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field            | Type   | Description                            |
| ---------------- | ------ | -------------------------------------- |
| `animation_name` | string | Animation the track was added to       |
| `widget_name`    | string | Widget bound to the track              |
| `property_name`  | string | Property being animated                |
| `track_type`     | string | Sequencer track type used              |
| `asset_path`     | string | Widget Blueprint asset path            |

## Examples

### Animate opacity

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "animation_name": "FadeIn",
  "widget_name": "OverlayImage",
  "property_name": "RenderOpacity"
}
```

## Errors

| Condition                | `isError` | Message                                          |
| ------------------------ | --------- | ------------------------------------------------ |
| Missing required param   | `true`    | `Missing required parameter '...'`               |
| Animation not found      | `true`    | `Animation '...' not found in '...'`             |
| Widget not found         | `true`    | `Widget '...' not found in '...'`                |
| Property not found       | `true`    | `Property '...' not found on widget '...'`       |
| Asset not found          | `true`    | `Widget Blueprint not found at '...'`            |
