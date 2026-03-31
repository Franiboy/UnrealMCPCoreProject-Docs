# get_widget_properties

Get detailed properties of a specific widget inside a Widget Blueprint. Returns all editable UProperty values.

## Input

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "RootCanvas",
  "category": "Behavior"
}
```

| Parameter     | Required | Description                                                                          |
| ------------- | -------- | ------------------------------------------------------------------------------------ |
| `asset_path`  | **yes**  | Asset path of the Widget Blueprint (e.g. `/Game/UI/WBP_MainMenu`)                   |
| `widget_name` | **yes**  | Name of the widget to inspect (e.g. `RootCanvas`, `TextBlock_Title`)                |
| `category`    | no       | Filter properties by category (e.g. `Appearance`, `Content`). Case-insensitive.     |

## Output

```json
{
  "widget_name": "RootCanvas",
  "widget_class": "CanvasPanel",
  "asset_path": "/Game/UI/WBP_MainMenu",
  "slot": null,
  "properties": {
    "Visibility": {
      "type": "ESlateVisibility",
      "value": "Visible",
      "category": "Behavior",
      "display_name": "Visibility"
    }
  },
  "property_count": 15,
  "available_categories": ["Accessibility", "Behavior", "Clipping", "Render Transform"],
  "category_filter": null
}
```

| Field                  | Type             | Description                                                       |
| ---------------------- | ---------------- | ----------------------------------------------------------------- |
| `widget_name`          | string           | Name of the inspected widget                                      |
| `widget_class`         | string           | Class of the widget (e.g. `CanvasPanel`, `TextBlock`)             |
| `asset_path`           | string           | The asset path requested                                          |
| `slot`                 | object \| null   | Slot info from parent panel                                       |
| `properties`           | object           | Map of property_name -> `{type, value, category, display_name}`   |
| `property_count`       | number           | Number of properties returned                                     |
| `available_categories` | array of strings | All discovered categories                                         |
| `category_filter`      | string \| null   | The category filter applied                                       |

## Examples

### Get all properties of a widget

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "RootCanvas"
}
```

### Filter properties by category

```json
{
  "asset_path": "/Game/UI/WBP_MainMenu",
  "widget_name": "TextBlock_Title",
  "category": "Appearance"
}
```

## Errors

| Condition                      | `isError` | Message                                              |
| ------------------------------ | --------- | ---------------------------------------------------- |
| Missing `asset_path`           | `true`    | `Missing required parameter 'asset_path'`            |
| Missing `widget_name`          | `true`    | `Missing required parameter 'widget_name'`           |
| Widget not found               | `true`    | `Widget 'X' not found in 'Y'`                       |
| Blueprint not found            | `true`    | `WidgetBlueprint not found: '/Game/...'`             |
