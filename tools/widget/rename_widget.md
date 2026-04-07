# rename_widget

Rename a widget within a Widget Blueprint's hierarchy. Automatically updates event bindings (OnClicked, OnPressed, etc.) and animation bindings that reference the old name.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "ControllsButton",
  "new_name": "ControlsButton"
}
```

| Parameter     | Required | Description                              |
| ------------- | -------- | ---------------------------------------- |
| `asset_path`  | **yes**  | Asset path of the Widget Blueprint.      |
| `widget_name` | **yes**  | Current name of the widget to rename.    |
| `new_name`    | **yes**  | New name for the widget.                 |

### Parameter Aliases

| Alias            | Maps to       |
| ---------------- | ------------- |
| `component_name` | `widget_name` |
| `widget_path`    | `asset_path`  |

## Output

```json
{
  "old_name": "ControllsButton",
  "new_name": "ControlsButton",
  "widget_class": "Button",
  "asset_path": "/Game/UI/WBP_MainMenu",
  "event_bindings_updated": 1,
  "animation_bindings_updated": 0
}
```

| Field                       | Type   | Description                                         |
| --------------------------- | ------ | --------------------------------------------------- |
| `old_name`                  | string | Previous widget name                                |
| `new_name`                  | string | New widget name (after rename)                      |
| `widget_class`              | string | UMG class of the widget (e.g. `Button`, `TextBlock`)|
| `asset_path`                | string | Widget Blueprint asset path                         |
| `event_bindings_updated`    | number | Number of delegate editor bindings updated          |
| `animation_bindings_updated`| number | Number of animation bindings updated                |

## Examples

### Fix a typo in a widget name

```json
{
  "asset_path": "/Game/UI/WBP_Settings",
  "widget_name": "ControllsButton",
  "new_name": "ControlsButton"
}
```

### Rename for clarity

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "widget_name": "Button_0",
  "new_name": "StartGameButton"
}
```

## Errors

| Condition              | `isError` | Message                                        |
| ---------------------- | --------- | ---------------------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter 'asset_path'`      |
| Missing `widget_name`  | `true`    | `Missing required parameter 'widget_name'`     |
| Missing `new_name`     | `true`    | `Missing required parameter 'new_name'`        |
| Widget not found       | `true`    | `Widget '...' not found in '...'`              |
| Name already exists    | `true`    | `A widget named '...' already exists in '...'` |
| Asset not found        | `true`    | `WidgetBlueprint not found: '...'`             |

## Notes

- Event bindings (`FDelegateEditorBinding`) that reference the widget by `ObjectName` are automatically updated to the new name.
- Animation bindings (`FWidgetAnimationBinding`) and MovieScene display names are also updated.
- The tool does **not** recompile the Blueprint (to avoid CommonUI crashes). The rename takes effect on next compile or PIE start.
