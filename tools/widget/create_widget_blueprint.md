# create_widget_blueprint

Create a new UMG Widget Blueprint asset.

## Input

```json
{
  "name": "WBP_MainMenu",
  "path": "/Game/UI",
  "parent_class": "UserWidget",
  "size_x": 1920,
  "size_y": 1080
}
```

| Parameter      | Required | Description                                                                 |
| -------------- | -------- | --------------------------------------------------------------------------- |
| `name`         | **yes**  | Widget Blueprint asset name (e.g. `WBP_MainMenu`)                          |
| `path`         | no       | Content folder path (default: `/Game`)                                      |
| `parent_class` | no       | Parent class name (default: `UserWidget`). Must be a UserWidget subclass.   |
| `size_x`       | no       | Design-time width in pixels (default: `1920`)                               |
| `size_y`       | no       | Design-time height in pixels (default: `1080`)                              |

## Output

```json
{
  "name": "WBP_MainMenu",
  "asset_path": "/Game/UI/WBP_MainMenu",
  "class": "WidgetBlueprint",
  "parent_class": "UserWidget",
  "root_widget": "RootCanvas",
  "root_widget_class": "CanvasPanel"
}
```

| Field               | Type   | Description                                    |
| ------------------- | ------ | ---------------------------------------------- |
| `name`              | string | Created Widget Blueprint asset name            |
| `asset_path`        | string | Full content path to the new Widget Blueprint  |
| `class`             | string | Always `WidgetBlueprint`                       |
| `parent_class`      | string | Parent class used                              |
| `root_widget`       | string | Name of the root widget                        |
| `root_widget_class` | string | Class of the root widget (e.g. `CanvasPanel`) |

## Examples

### Create a Widget Blueprint with defaults

```json
{"name": "WBP_HUD"}
```

### Create in a specific folder

```json
{
  "name": "WBP_MainMenu",
  "path": "/Game/UI"
}
```

## Errors

| Condition                      | `isError` | Message                                              |
| ------------------------------ | --------- | ---------------------------------------------------- |
| Missing `name`                 | `true`    | `Missing required parameter 'name'`                  |
| Asset already exists           | `true`    | `An asset already exists at '/Game/...'`             |
| Invalid `path`                 | `true`    | `Invalid path '...'. Must start with /Game or /Engine.` |
| Parent class not found         | `true`    | `Parent class '...' not found or is not a UserWidget subclass.` |
