# set_widget_slot

Modify slot/layout properties on a widget's slot inside a Widget Blueprint. Slot properties control how a widget is positioned and sized within its parent panel.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "TitleText",
  "slot_properties": {
    "anchors_min_x": 0.5,
    "anchors_min_y": 0.0,
    "anchors_max_x": 0.5,
    "anchors_max_y": 0.0,
    "offset_left": -200,
    "offset_top": 50,
    "offset_right": 200,
    "offset_bottom": 100,
    "alignment_x": 0.5,
    "alignment_y": 0.0,
    "auto_size": false,
    "z_order": 1
  }
}
```

| Parameter         | Required | Description                                                               |
| ----------------- | -------- | ------------------------------------------------------------------------- |
| `asset_path`      | **yes**  | Full content path to the Widget Blueprint (e.g. `/Game/UI/WBP_MainMenu`) |
| `widget_name`     | **yes**  | Name of the target widget whose slot to modify                            |
| `slot_properties` | **yes**  | Object mapping slot property names to their new values                    |

### Supported Slot Properties

#### CanvasPanel Slot

| Property        | Type  | Description                              |
| --------------- | ----- | ---------------------------------------- |
| `anchors_min_x` | float | Minimum anchor X (0.0 – 1.0)            |
| `anchors_min_y` | float | Minimum anchor Y (0.0 – 1.0)            |
| `anchors_max_x` | float | Maximum anchor X (0.0 – 1.0)            |
| `anchors_max_y` | float | Maximum anchor Y (0.0 – 1.0)            |
| `offset_left`   | float | Left offset in pixels                    |
| `offset_top`    | float | Top offset in pixels                     |
| `offset_right`  | float | Right offset in pixels                   |
| `offset_bottom` | float | Bottom offset in pixels                  |
| `alignment_x`   | float | Alignment X (0.0 – 1.0)                 |
| `alignment_y`   | float | Alignment Y (0.0 – 1.0)                 |
| `auto_size`     | bool  | Whether the widget auto-sizes to content |
| `z_order`       | int   | Render order (higher draws on top)       |

#### HorizontalBox / VerticalBox Slot

| Property      | Type   | Description                                                 |
| ------------- | ------ | ----------------------------------------------------------- |
| `size_rule`   | string | `Auto` or `Fill`                                            |
| `size_value`  | float  | Fill ratio when `size_rule` is `Fill`                       |
| `padding_left`   | float | Left padding in pixels                                   |
| `padding_top`    | float | Top padding in pixels                                    |
| `padding_right`  | float | Right padding in pixels                                  |
| `padding_bottom` | float | Bottom padding in pixels                                 |

#### Overlay Slot

| Property         | Type  | Description                  |
| ---------------- | ----- | ---------------------------- |
| `padding_left`   | float | Left padding in pixels       |
| `padding_top`    | float | Top padding in pixels        |
| `padding_right`  | float | Right padding in pixels      |
| `padding_bottom` | float | Bottom padding in pixels     |

## Output

```json
{
  "widget_name": "TitleText",
  "asset_path": "/Game/UI/WBP_MainMenu",
  "slot_class": "CanvasPanelSlot",
  "applied_properties": ["anchors_min_x", "anchors_min_y", "anchors_max_x", "anchors_max_y", "offset_left", "offset_top", "offset_right", "offset_bottom", "alignment_x", "alignment_y", "auto_size", "z_order"],
  "applied_count": 12
}
```

| Field                | Type    | Description                                          |
| -------------------- | ------- | ---------------------------------------------------- |
| `widget_name`        | string  | Name of the widget whose slot was modified           |
| `asset_path`         | string  | Full content path to the Widget Blueprint            |
| `slot_class`         | string  | Class of the slot (e.g. `CanvasPanelSlot`)           |
| `applied_properties` | array   | List of slot property names that were applied        |
| `applied_count`      | integer | Number of slot properties successfully applied       |

## Examples

### Center a widget on a CanvasPanel

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "TitleText",
  "slot_properties": {
    "anchors_min_x": 0.5,
    "anchors_min_y": 0.5,
    "anchors_max_x": 0.5,
    "anchors_max_y": 0.5,
    "alignment_x": 0.5,
    "alignment_y": 0.5
  }
}
```

### Set fill size in a VerticalBox

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "widget_name": "ContentArea",
  "slot_properties": {
    "size_rule": "Fill",
    "size_value": 1.0,
    "padding_top": 10,
    "padding_bottom": 10
  }
}
```

### Set z-order on a CanvasPanel child

```json
{
  "asset_path": "/Game/UI/WBP_Inventory",
  "widget_name": "Tooltip",
  "slot_properties": {
    "z_order": 10
  }
}
```

## Errors

| Condition                        | `isError` | Message                                                        |
| -------------------------------- | --------- | -------------------------------------------------------------- |
| Missing required parameter       | `true`    | `Missing required parameter '<param>'`                         |
| Widget not found                 | `true`    | `Widget '<widget_name>' not found in '<asset_path>'`           |
| Widget has no slot (root widget) | `true`    | `Widget '<widget_name>' is the root widget and has no slot`    |
| Widget Blueprint not found       | `true`    | `Widget Blueprint not found at '<asset_path>'`                 |
