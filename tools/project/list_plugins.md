# list_plugins

List all discovered plugins (enabled and disabled) with version, description, category, creator, type (Engine/Project), and enabled status. Optionally filter by name, enabled status, or category.

## Input

```json
{
  "name_filter": "MCP",
  "enabled_only": true,
  "category_filter": "Editor",
  "limit": 20
}
```

| Parameter         | Required | Default | Description                                              |
| ----------------- | -------- | ------- | -------------------------------------------------------- |
| `name_filter`     | no       | ---     | Case-insensitive substring filter on plugin name.        |
| `enabled_only`    | no       | `false` | If true, only list enabled plugins.                      |
| `category_filter` | no       | ---     | Case-insensitive substring filter on plugin category.    |
| `limit`           | no       | all     | Maximum number of plugins to return.                     |

## Output

| Field              | Type   | Description                                       |
| ------------------ | ------ | ------------------------------------------------- |
| `count`            | number | Number of plugins returned (after filtering)      |
| `total_discovered` | number | Total number of discovered plugins (before filter)|
| `plugins`          | array  | Array of plugin objects (see below)               |
| `name_filter`      | string | Applied name filter (if any)                      |
| `category_filter`  | string | Applied category filter (if any)                  |
| `enabled_only`     | boolean| Whether only enabled plugins were listed          |

### Plugin object

| Field           | Type    | Description                                     |
| --------------- | ------- | ----------------------------------------------- |
| `name`          | string  | Plugin name                                     |
| `friendly_name` | string  | Display name (if different from name)            |
| `enabled`       | boolean | Whether the plugin is enabled                   |
| `version`       | number  | Numeric version                                 |
| `version_name`  | string  | Display version string (e.g. `"1.0.0"`)        |
| `description`   | string  | Plugin description                              |
| `category`      | string  | Plugin category                                 |
| `created_by`    | string  | Creator name                                    |
| `loaded_from`   | string  | `"Engine"` or `"Project"`                       |
| `has_content`   | boolean | Whether the plugin contains content assets      |
| `beta`          | boolean | Whether this is a beta plugin                   |
| `experimental`  | boolean | Whether this is an experimental plugin          |

## Examples

### List all plugins

```json
{}
```

### List only enabled plugins

```json
{
  "enabled_only": true
}
```

### Search for a specific plugin

```json
{
  "name_filter": "UnrealMCPCore"
}
```

### List plugins in a category

```json
{
  "category_filter": "Gameplay"
}
```

## Errors

This tool does not fail — it always returns available plugin information.
