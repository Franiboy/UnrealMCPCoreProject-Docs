# rename_widget_animation

Rename a widget animation within a Widget Blueprint.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "animation_name": "FadeIn",
  "new_name": "FadeInSlow"
}
```

| Parameter        | Required | Description                              |
| ---------------- | -------- | ---------------------------------------- |
| `asset_path`     | **yes**  | Asset path of the Widget Blueprint.      |
| `animation_name` | **yes**  | Current name of the animation.           |
| `new_name`       | **yes**  | New name for the animation.              |

## Output

```json
{
  "old_name": "FadeIn",
  "new_name": "FadeInSlow",
  "new_display_name": "FadeInSlow",
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field              | Type   | Description                            |
| ------------------ | ------ | -------------------------------------- |
| `old_name`         | string | Previous animation name                |
| `new_name`         | string | New internal object name               |
| `new_display_name` | string | New human-readable display label       |
| `asset_path`       | string | Widget Blueprint asset path            |

## Examples

### Rename an animation

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "animation_name": "Anim1",
  "new_name": "SlideInFromLeft"
}
```

## Errors

| Condition                | `isError` | Message                                      |
| ------------------------ | --------- | -------------------------------------------- |
| Missing `asset_path`     | `true`    | `Missing required parameter 'asset_path'`    |
| Missing `animation_name` | `true`    | `Missing required parameter 'animation_name'`|
| Missing `new_name`       | `true`    | `Missing required parameter 'new_name'`      |
| Animation not found      | `true`    | `Animation '...' not found in '...'`         |
| Name already exists      | `true`    | `An animation named '...' already exists in '...'` |
| Asset not found          | `true`    | `WidgetBlueprint not found: '...'`           |
