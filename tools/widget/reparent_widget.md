# reparent_widget

Move a widget to a different parent panel inside a Widget Blueprint. The root widget cannot be reparented.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "TitleText",
  "new_parent_name": "HeaderPanel",
  "index": 0
}
```

| Parameter         | Required | Description                                                               |
| ----------------- | -------- | ------------------------------------------------------------------------- |
| `asset_path`      | **yes**  | Full content path to the Widget Blueprint (e.g. `/Game/UI/WBP_MainMenu`) |
| `widget_name`     | **yes**  | Name of the widget to move                                                |
| `new_parent_name` | **yes**  | Name of the new parent panel widget                                       |
| `index`           | no       | Insertion index among the new parent's children (appended to end if omitted) |

## Output

```json
{
  "widget_name": "TitleText",
  "old_parent": "RootCanvas",
  "new_parent": "HeaderPanel",
  "index": 0,
  "asset_path": "/Game/UI/WBP_MainMenu"
}
```

| Field          | Type    | Description                                        |
| -------------- | ------- | -------------------------------------------------- |
| `widget_name`  | string  | Name of the widget that was moved                  |
| `old_parent`   | string  | Name of the previous parent panel                  |
| `new_parent`   | string  | Name of the new parent panel                       |
| `index`        | integer | Index at which the widget was inserted             |
| `asset_path`   | string  | Full content path to the Widget Blueprint          |

## Examples

### Move a widget to a different panel

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "TitleText",
  "new_parent_name": "HeaderPanel"
}
```

### Move a widget to a specific index

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "widget_name": "HealthBar",
  "new_parent_name": "StatusPanel",
  "index": 0
}
```

### Reorganize widgets between panels

```json
{
  "asset_path": "/Game/UI/WBP_Inventory",
  "widget_name": "ItemDescription",
  "new_parent_name": "DetailsBox",
  "index": 2
}
```

## Errors

| Condition                            | `isError` | Message                                                        |
| ------------------------------------ | --------- | -------------------------------------------------------------- |
| Missing required parameter           | `true`    | `Missing required parameter '<param>'`                         |
| Widget not found                     | `true`    | `Widget '<widget_name>' not found in '<asset_path>'`           |
| Cannot reparent root widget          | `true`    | `Cannot reparent the root widget`                              |
| New parent not found                 | `true`    | `Widget '<new_parent_name>' not found in '<asset_path>'`       |
| New parent is not a panel widget     | `true`    | `Widget '<new_parent_name>' is not a panel widget and cannot have children` |
| Cannot reparent to own descendant    | `true`    | `Cannot reparent '<widget_name>' to its own descendant '<new_parent_name>'` |
| Widget Blueprint not found           | `true`    | `Widget Blueprint not found at '<asset_path>'`                 |
