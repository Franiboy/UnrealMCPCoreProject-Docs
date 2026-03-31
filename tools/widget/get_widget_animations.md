# get_widget_animations

List all animations in a Widget Blueprint. Returns name, length, and bound objects/tracks for each UWidgetAnimation.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Parameter    | Required | Description                            |
| ------------ | -------- | -------------------------------------- |
| `asset_path` | **yes**  | Asset path of the Widget Blueprint.    |

## Output

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "count": 2,
  "animations": [
    {
      "name": "FadeIn",
      "display_name": "FadeIn",
      "length_seconds": 2.0,
      "start_frame": 0,
      "end_frame": 48000,
      "binding_count": 1,
      "track_count": 1,
      "bindings": [
        {
          "name": "MyImage",
          "tracks": [
            {
              "type": "MovieSceneFloatTrack",
              "display_name": "RenderOpacity"
            }
          ]
        }
      ]
    }
  ]
}
```

| Field                   | Type   | Description                                       |
| ----------------------- | ------ | ------------------------------------------------- |
| `asset_path`            | string | Widget Blueprint asset path                       |
| `count`                 | number | Total number of animations                        |
| `animations[].name`     | string | Internal object name of the animation             |
| `animations[].display_name` | string | Human-readable display label                  |
| `animations[].length_seconds` | number | Duration in seconds                         |
| `animations[].bindings` | array  | Objects bound to the animation with their tracks  |

## Examples

### List animations

```json
{"asset_path": "/Game/UI/WBP_HUD"}
```

## Errors

| Condition              | `isError` | Message                           |
| ---------------------- | --------- | --------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter 'asset_path'` |
| Asset not found        | `true`    | `Widget Blueprint not found at '...'`     |
