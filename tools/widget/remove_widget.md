# remove_widget

Remove a widget from a Widget Blueprint's hierarchy. The root widget cannot be removed.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "TitleText"
}
```

| Parameter     | Required | Description                                                               |
| ------------- | -------- | ------------------------------------------------------------------------- |
| `asset_path`  | **yes**  | Full content path to the Widget Blueprint (e.g. `/Game/UI/WBP_MainMenu`) |
| `widget_name` | **yes**  | Name of the widget to remove                                              |

## Output

```json
{
  "removed_widget": "TitleText",
  "removed_class": "TextBlock",
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field            | Type   | Description                                     |
| ---------------- | ------ | ----------------------------------------------- |
| `removed_widget` | string | Name of the widget that was removed             |
| `removed_class`  | string | Class of the widget that was removed            |
| `asset_path`     | string | Full content path to the Widget Blueprint       |

## Examples

### Remove a text widget

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "TitleText"
}
```

### Remove a button

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "widget_name": "StartButton"
}
```

## Errors

| Condition                        | `isError` | Message                                                        |
| -------------------------------- | --------- | -------------------------------------------------------------- |
| Missing required parameter       | `true`    | `Missing required parameter '<param>'`                         |
| Widget not found                 | `true`    | `Widget '<widget_name>' not found in '<asset_path>'`           |
| Cannot remove root widget        | `true`    | `Cannot remove the root widget`                                |
| Widget Blueprint not found       | `true`    | `Widget Blueprint not found at '<asset_path>'`                 |
