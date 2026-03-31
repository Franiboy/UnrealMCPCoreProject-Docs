# delete_widget_animation

Delete a UWidgetAnimation from a Widget Blueprint.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "animation_name": "FadeIn"
}
```

| Parameter        | Required | Description                              |
| ---------------- | -------- | ---------------------------------------- |
| `asset_path`     | **yes**  | Asset path of the Widget Blueprint.      |
| `animation_name` | **yes**  | Name of the animation to delete.         |

## Output

```json
{
  "deleted_animation": "FadeIn",
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field               | Type   | Description                            |
| ------------------- | ------ | -------------------------------------- |
| `deleted_animation` | string | Name of the deleted animation          |
| `asset_path`        | string | Widget Blueprint asset path            |

## Examples

### Delete an animation

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "animation_name": "OldFadeIn"
}
```

## Errors

| Condition                | `isError` | Message                                      |
| ------------------------ | --------- | -------------------------------------------- |
| Missing `asset_path`     | `true`    | `Missing required parameter 'asset_path'`    |
| Missing `animation_name` | `true`    | `Missing required parameter 'animation_name'`|
| Animation not found      | `true`    | `Animation '...' not found in '...'`         |
| Asset not found          | `true`    | `Widget Blueprint not found at '...'`        |
