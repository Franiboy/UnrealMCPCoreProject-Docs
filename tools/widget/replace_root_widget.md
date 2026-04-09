# replace_root_widget

Replace the root widget of a Widget Blueprint with a new panel widget. All children of the old root are automatically reparented to the new root. Useful after reparenting a Widget Blueprint to clear conflicting widget trees.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "new_root_class": "CanvasPanel",
  "new_root_name": "RootCanvas"
}
```

| Parameter        | Required | Description                                                                                       |
| ---------------- | -------- | ------------------------------------------------------------------------------------------------- |
| `asset_path`     | **yes**  | Asset path of the Widget Blueprint                                                                |
| `new_root_class` | **yes**  | Widget class for the new root (e.g. `CanvasPanel`, `VerticalBox`, `Overlay`). Must be a panel widget |
| `new_root_name`  | no       | Name for the new root widget (default: `RootPanel`)                                               |

## Output

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "old_root_name": "CanvasPanel_0",
  "old_root_class": "CanvasPanel",
  "new_root_name": "RootCanvas",
  "new_root_class": "CanvasPanel",
  "reparented_children": 3
}
```

| Field                 | Type    | Description                                           |
| --------------------- | ------- | ----------------------------------------------------- |
| `asset_path`          | string  | Full content path to the Widget Blueprint              |
| `old_root_name`       | string  | Name of the previous root widget                       |
| `old_root_class`      | string  | Class of the previous root widget                      |
| `new_root_name`       | string  | Name of the newly created root widget                  |
| `new_root_class`      | string  | Class of the newly created root widget                 |
| `reparented_children` | integer | Number of children moved from old root to new root     |

## Examples

### Replace root with a CanvasPanel

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "new_root_class": "CanvasPanel"
}
```

### Replace root with a VerticalBox and custom name

```json
{
  "asset_path": "/Game/UI/WBP_Settings",
  "new_root_class": "VerticalBox",
  "new_root_name": "MainLayout"
}
```

### Replace root with an Overlay

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "new_root_class": "Overlay",
  "new_root_name": "HUDRoot"
}
```

## Errors

| Condition                            | `isError` | Message                                                                                     |
| ------------------------------------ | --------- | ------------------------------------------------------------------------------------------- |
| Missing required parameter           | `true`    | `Missing required parameter '<param>'`                                                      |
| Widget Blueprint not found           | `true`    | `Widget Blueprint not found at '<asset_path>'`                                              |
| Invalid or non-panel widget class    | `true`    | `'<new_root_class>' is not a valid panel widget class. Use CanvasPanel, VerticalBox, HorizontalBox, Overlay, etc.` |
| Failed to construct widget           | `true`    | `Failed to construct widget of class '<new_root_class>'`                                    |

## Notes

- The new root must be a panel widget (inherits from `UPanelWidget`). Supported classes include `CanvasPanel`, `VerticalBox`, `HorizontalBox`, `Overlay`, `GridPanel`, `UniformGridPanel`, `WidgetSwitcher`, etc.
- If the old root is also a panel, all of its children are automatically reparented to the new root. If the old root is not a panel (i.e. it has no children), `reparented_children` will be `0`.
- The class name can be specified with or without the `U` prefix (e.g. both `CanvasPanel` and `UCanvasPanel` work).
- The `widget_path` parameter is accepted as an alias for `asset_path`.
