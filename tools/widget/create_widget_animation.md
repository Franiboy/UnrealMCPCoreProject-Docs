# create_widget_animation

Create a new UWidgetAnimation inside a Widget Blueprint. Specify a name and optional length in seconds.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "animation_name": "FadeIn",
  "length": 2.0
}
```

| Parameter        | Required | Description                                        |
| ---------------- | -------- | -------------------------------------------------- |
| `asset_path`     | **yes**  | Asset path of the Widget Blueprint.                |
| `animation_name` | **yes**  | Name for the new animation (e.g. `FadeIn`).        |
| `length`         | no       | Animation length in seconds (default: `1.0`).      |

## Output

```json
{
  "animation_name": "FadeIn",
  "display_name": "FadeIn",
  "length_seconds": 2.0,
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field            | Type   | Description                            |
| ---------------- | ------ | -------------------------------------- |
| `animation_name` | string | Internal object name of the animation  |
| `display_name`   | string | Human-readable display label           |
| `length_seconds` | number | Duration in seconds                    |
| `asset_path`     | string | Widget Blueprint asset path            |

## Examples

### Create a 1-second animation

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "animation_name": "SlideIn"
}
```

### Create with explicit length

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "animation_name": "FadeOut",
  "length": 0.5
}
```

## Errors

| Condition                 | `isError` | Message                                      |
| ------------------------- | --------- | -------------------------------------------- |
| Missing `asset_path`      | `true`    | `Missing required parameter 'asset_path'`    |
| Missing `animation_name`  | `true`    | `Missing required parameter 'animation_name'`|
| Animation already exists  | `true`    | `Animation '...' already exists in '...'`    |
| Asset not found           | `true`    | `Widget Blueprint not found at '...'`        |
