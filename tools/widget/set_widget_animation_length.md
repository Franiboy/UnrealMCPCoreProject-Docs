# set_widget_animation_length

Change the duration of a widget animation. Adjusts the playback range and resizes existing track sections.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "animation_name": "FadeIn",
  "length": 3.0
}
```

| Parameter        | Required | Description                                        |
| ---------------- | -------- | -------------------------------------------------- |
| `asset_path`     | **yes**  | Asset path of the Widget Blueprint.                |
| `animation_name` | **yes**  | Name of the animation.                             |
| `length`         | **yes**  | New animation length in seconds (must be > 0).     |

## Output

```json
{
  "animation_name": "FadeIn",
  "old_length": 1.0,
  "new_length": 3.0,
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field            | Type   | Description                            |
| ---------------- | ------ | -------------------------------------- |
| `animation_name` | string | Animation name                         |
| `old_length`     | number | Previous duration in seconds           |
| `new_length`     | number | New duration in seconds                |
| `asset_path`     | string | Widget Blueprint asset path            |

## Examples

### Extend animation to 5 seconds

```json
{"asset_path": "/Game/UI/WBP_HUD", "animation_name": "FadeIn", "length": 5.0}
```

### Shorten to half a second

```json
{"asset_path": "/Game/UI/WBP_HUD", "animation_name": "FadeIn", "length": 0.5}
```

## Errors

| Condition                | `isError` | Message                                      |
| ------------------------ | --------- | -------------------------------------------- |
| Missing `asset_path`     | `true`    | `Missing required parameter 'asset_path'`    |
| Missing `animation_name` | `true`    | `Missing required parameter 'animation_name'`|
| Missing `length`         | `true`    | `Missing required parameter 'length'`        |
| Length <= 0              | `true`    | `Parameter 'length' must be greater than 0`  |
| Animation not found      | `true`    | `Animation '...' not found in '...'`         |
| Asset not found          | `true`    | `WidgetBlueprint not found: '...'`           |
