# list_widget_types

List available UWidget subclasses that can be added to a Widget Blueprint. Returns class name, category, and parent class for each type.

## Input

```json
{
  "category": "Panel"
}
```

| Parameter  | Required | Description                                                                 |
| ---------- | -------- | --------------------------------------------------------------------------- |
| `search`   | no       | Case-insensitive substring filter on class name.                            |
| `category` | no       | Filter by category (e.g. `Common`, `Panel`, `Input`). Case-insensitive.     |

## Output

```json
{
  "count": 3,
  "widget_types": [
    {
      "class_name": "CanvasPanel",
      "display_name": "CanvasPanel",
      "category": "Panel",
      "parent_class": "PanelWidget",
      "module": "/Script/UMG"
    },
    {
      "class_name": "HorizontalBox",
      "display_name": "HorizontalBox",
      "category": "Panel",
      "parent_class": "PanelWidget",
      "module": "/Script/UMG"
    },
    {
      "class_name": "VerticalBox",
      "display_name": "VerticalBox",
      "category": "Panel",
      "parent_class": "PanelWidget",
      "module": "/Script/UMG"
    }
  ],
  "available_categories": ["Common", "Input", "Panel"],
  "category_filter": "Panel"
}
```

| Field                  | Type             | Description                                          |
| ---------------------- | ---------------- | ---------------------------------------------------- |
| `count`                | number           | Number of widget types returned                      |
| `widget_types`         | array            | Array of widget type objects                         |
| `widget_types[].class_name`   | string   | C++ / Blueprint class name                           |
| `widget_types[].display_name` | string   | Human-readable display name                          |
| `widget_types[].category`     | string   | Category the widget belongs to (optional)            |
| `widget_types[].parent_class` | string   | Immediate parent class                               |
| `widget_types[].module`       | string   | Module the class lives in (e.g. `/Script/UMG`)      |
| `available_categories` | array of strings | All discovered category names for discoverability    |
| `search_filter`        | string           | The search filter applied (present only when set)    |
| `category_filter`      | string           | The category filter applied (present only when set)  |

## Examples

### List all panel widgets

```json
{"category": "Panel"}
```

### Search by class name substring

```json
{"search": "button"}
```

### Combine search and category filters

```json
{
  "search": "box",
  "category": "Panel"
}
```

### List all available widget types (no filters)

```json
{}
```

## Errors

None specific; this tool always succeeds. If no widget types match the given filters, the response returns an empty `widget_types` array with `count` set to `0`.
