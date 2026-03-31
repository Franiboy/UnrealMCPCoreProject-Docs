# add_widget

Add a child widget to a panel inside a Widget Blueprint.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_class": "TextBlock",
  "parent_name": "RootCanvas",
  "widget_name": "TitleText",
  "index": 0
}
```

| Parameter      | Required | Description                                                                 |
| -------------- | -------- | --------------------------------------------------------------------------- |
| `asset_path`   | **yes**  | Full content path to the Widget Blueprint (e.g. `/Game/UI/WBP_MainMenu`)   |
| `widget_class` | **yes**  | UMG widget class to add (e.g. `TextBlock`, `Button`, `Image`, `HorizontalBox`) |
| `parent_name`  | **yes**  | Name of the parent panel widget to add the child to                         |
| `widget_name`  | no       | Name for the new widget (auto-generated if omitted)                         |
| `index`        | no       | Insertion index among the parent's children (appended to end if omitted)    |

## Output

```json
{
  "widget_name": "TitleText",
  "widget_class": "TextBlock",
  "parent_name": "RootCanvas",
  "asset_path": "/Game/UI/WBP_MainMenu",
  "index": 0
}
```

| Field          | Type    | Description                                      |
| -------------- | ------- | ------------------------------------------------ |
| `widget_name`  | string  | Name of the newly added widget                   |
| `widget_class` | string  | Class of the newly added widget                  |
| `parent_name`  | string  | Name of the parent panel it was added to         |
| `asset_path`   | string  | Full content path to the Widget Blueprint        |
| `index`        | integer | Index at which the widget was inserted           |

## Examples

### Add a TextBlock to the root canvas

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_class": "TextBlock",
  "parent_name": "RootCanvas"
}
```

### Add a Button at a specific index with a custom name

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "widget_class": "Button",
  "parent_name": "RootCanvas",
  "widget_name": "StartButton",
  "index": 2
}
```

### Add an Image inside a nested panel

```json
{
  "asset_path": "/Game/UI/WBP_Inventory",
  "widget_class": "Image",
  "parent_name": "ItemsBox"
}
```

## Errors

| Condition                        | `isError` | Message                                                        |
| -------------------------------- | --------- | -------------------------------------------------------------- |
| Missing required parameter       | `true`    | `Missing required parameter '<param>'`                         |
| Parent widget not found          | `true`    | `Widget '<parent_name>' not found in '<asset_path>'`           |
| Parent is not a panel widget     | `true`    | `Widget '<parent_name>' is not a panel widget and cannot have children` |
| Widget class not found           | `true`    | `Widget class '<widget_class>' not found`                      |
| Widget Blueprint not found       | `true`    | `Widget Blueprint not found at '<asset_path>'`                 |
