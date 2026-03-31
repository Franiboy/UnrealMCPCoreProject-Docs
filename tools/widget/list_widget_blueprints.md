# list_widget_blueprints

List UMG Widget Blueprint assets in the project, supporting filtering by path and name search.

## Input

```json
{
  "path": "/Game/UI",
  "search": "MainMenu",
  "include_engine_content": false
}
```

| Parameter                | Required | Description                                                                 |
| ------------------------ | -------- | --------------------------------------------------------------------------- |
| `path`                   | no       | Filter by content path prefix (default: `/Game` — project content only)     |
| `search`                 | no       | Case-insensitive substring filter on asset name                             |
| `include_engine_content` | no       | Include engine content (default: `false`)                                   |

## Output

```json
{
  "count": 1,
  "widget_blueprints": [
    {
      "name": "WBP_MainMenu",
      "asset_path": "/Game/UI/WBP_MainMenu.WBP_MainMenu",
      "package_path": "/Game/UI",
      "parent_class": "UserWidget"
    }
  ],
  "path_filter": "/Game",
  "search_filter": "MainMenu"
}
```

| Field               | Type   | Description                                           |
| ------------------- | ------ | ----------------------------------------------------- |
| `count`             | number | Number of widget blueprints returned                  |
| `widget_blueprints` | array  | Array of widget blueprint objects (see below)         |
| `path_filter`       | string | The path filter applied (omitted when not specified)  |
| `search_filter`     | string | The search filter applied (omitted when not specified) |

Each object in `widget_blueprints`:

| Field          | Type   | Description                                    |
| -------------- | ------ | ---------------------------------------------- |
| `name`         | string | Widget Blueprint asset name                    |
| `asset_path`   | string | Full asset path including object name          |
| `package_path` | string | Package path (content folder) of the asset     |
| `parent_class` | string | Parent class of the widget (e.g. `UserWidget`) |

## Examples

### List all widget blueprints in the project

```json
{}
```

### Search by name

```json
{"search": "MainMenu"}
```

### Filter by path

```json
{
  "path": "/Game/UI",
  "search": "HUD"
}
```

### Include engine content

```json
{
  "include_engine_content": true
}
```

## Errors

No tool-specific errors. The tool always succeeds and returns an empty array if no matches are found.
