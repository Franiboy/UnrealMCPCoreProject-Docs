# set_widget_property

Modify one or more properties on a widget inside a Widget Blueprint.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "TitleText",
  "properties": {
    "Text": "Welcome",
    "ColorAndOpacity": "(R=1,G=0,B=0,A=1)"
  }
}
```

| Parameter    | Required | Description                                                               |
| ------------ | -------- | ------------------------------------------------------------------------- |
| `asset_path` | **yes**  | Full content path to the Widget Blueprint (e.g. `/Game/UI/WBP_MainMenu`) |
| `widget_name`| **yes**  | Name of the target widget                                                 |
| `properties` | **yes**  | Object mapping property names to their new values                         |

## Output

```json
{
  "widget_name": "TitleText",
  "asset_path": "/Game/UI/WBP_MainMenu",
  "succeeded": ["Text", "ColorAndOpacity"],
  "failed": [],
  "succeeded_count": 2,
  "failed_count": 0
}
```

| Field             | Type    | Description                                        |
| ----------------- | ------- | -------------------------------------------------- |
| `widget_name`     | string  | Name of the modified widget                        |
| `asset_path`      | string  | Full content path to the Widget Blueprint          |
| `succeeded`       | array   | List of property names that were set successfully  |
| `failed`          | array   | List of property names that failed to set          |
| `succeeded_count` | integer | Number of properties set successfully              |
| `failed_count`    | integer | Number of properties that failed to set            |

## Examples

### Set text on a TextBlock

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "TitleText",
  "properties": {
    "Text": "Main Menu"
  }
}
```

### Set multiple properties on a Button

```json
{
  "asset_path": "/Game/UI/WBP_HUD",
  "widget_name": "StartButton",
  "properties": {
    "IsEnabled": true,
    "Visibility": "Visible"
  }
}
```

### Set properties with partial failure

```json
{
  "asset_path": "/Game/UI/WBP_Inventory",
  "widget_name": "ItemIcon",
  "properties": {
    "Brush": "/Game/Textures/T_Sword",
    "NonExistentProp": "value"
  }
}
```

Response when some properties fail:

```json
{
  "widget_name": "ItemIcon",
  "asset_path": "/Game/UI/WBP_Inventory",
  "succeeded": ["Brush"],
  "failed": ["NonExistentProp"],
  "succeeded_count": 1,
  "failed_count": 1
}
```

> **Note:** If *all* properties fail to set, the response will have `isError: true`.

## Errors

| Condition                        | `isError` | Message                                                        |
| -------------------------------- | --------- | -------------------------------------------------------------- |
| Missing required parameter       | `true`    | `Missing required parameter '<param>'`                         |
| Widget not found                 | `true`    | `Widget '<widget_name>' not found in '<asset_path>'`           |
| All properties failed            | `true`    | `All properties failed to set on '<widget_name>'`              |
| Widget Blueprint not found       | `true`    | `Widget Blueprint not found at '<asset_path>'`                 |
