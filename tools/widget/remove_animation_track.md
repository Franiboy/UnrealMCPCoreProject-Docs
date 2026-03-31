# remove_animation_track

Remove a property track from a widget animation. If no tracks remain for the widget binding, the binding is also removed.

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
| `animation_name` | **yes**  | Name of the animation containing the track.              |
| `widget_name`    | **yes**  | Name of the widget whose track to remove.                |
| `property_name`  | **yes**  | Property name of the track to remove (e.g. `RenderOpacity`). |

## Output

```json
{
  "animation_name": "FadeIn",
  "widget_name": "MyImage",
  "property_name": "RenderOpacity",
  "binding_removed": true,
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field             | Type    | Description                                      |
| ----------------- | ------- | ------------------------------------------------ |
| `animation_name`  | string  | Animation the track was removed from             |
| `widget_name`     | string  | Widget the track belonged to                     |
| `property_name`   | string  | Property that was animated                       |
| `binding_removed` | boolean | Whether the widget binding was also removed      |
| `asset_path`      | string  | Widget Blueprint asset path                      |

## Examples

### Remove an opacity track

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
| No binding for widget    | `true`    | `No binding for widget '...' in animation`       |
| No track for property    | `true`    | `No track for property '...' on widget '...'`    |
| Asset not found          | `true`    | `WidgetBlueprint not found: '...'`               |
