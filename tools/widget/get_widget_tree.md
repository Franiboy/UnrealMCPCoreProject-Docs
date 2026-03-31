# get_widget_tree

Get the full widget hierarchy of a Widget Blueprint, returning each widget with its class, name, slot info, and children recursively.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Parameter    | Required | Description                                                        |
| ------------ | -------- | ------------------------------------------------------------------ |
| `asset_path` | **yes**  | Asset path of the Widget Blueprint (e.g. `/Game/UI/WBP_MainMenu`) |

## Output

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "name": "WBP_MainMenu",
  "root_widget": {
    "name": "RootCanvas",
    "class": "CanvasPanel",
    "is_visible": true,
    "child_count": 0
  },
  "total_widget_count": 1
}
```

| Field                | Type   | Description                                                                                                         |
| -------------------- | ------ | ------------------------------------------------------------------------------------------------------------------- |
| `asset_path`         | string | The asset path requested                                                                                            |
| `name`               | string | The Widget Blueprint's name                                                                                         |
| `root_widget`        | object | The root widget node with nested children (see below)                                                               |
| `total_widget_count` | number | Total number of widgets in the tree                                                                                 |

### Widget Node Fields

Each widget node inside `root_widget` (and its recursive `children`) contains:

| Field         | Type    | Description                                                                                          |
| ------------- | ------- | ---------------------------------------------------------------------------------------------------- |
| `name`        | string  | Widget name                                                                                          |
| `class`       | string  | Widget class (e.g. `CanvasPanel`, `TextBlock`)                                                       |
| `is_visible`  | boolean | Whether the widget is visible                                                                        |
| `slot`        | object  | Slot info from parent panel (anchors, offsets, alignment, padding, etc. — varies by slot type)        |
| `children`    | array   | Child widget nodes (recursive)                                                                       |
| `child_count` | number  | Number of children (for panel widgets)                                                               |

## Examples

### Get the widget tree of a Widget Blueprint

```json
{"name": "get_widget_tree", "arguments": {"asset_path": "/Game/UI/WBP_MainMenu"}}
```

## Errors

| Condition              | `isError` | Message                                              |
| ---------------------- | --------- | ---------------------------------------------------- |
| Missing `asset_path`   | `true`    | `Missing required parameter 'asset_path'`            |
| Asset not found        | `true`    | `WidgetBlueprint not found: '/Game/...'`             |
| Wrong asset type       | `true`    | `Asset '...' is a ..., not a WidgetBlueprint.`      |
