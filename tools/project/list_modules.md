# list_modules

List all modules known to the engine with name, file path, loaded status, and game module flag. Optionally filter by name, loaded status, or game modules only.

## Input

```json
{
  "name_filter": "MCP",
  "loaded_only": true,
  "game_only": false,
  "limit": 50
}
```

| Parameter     | Required | Default | Description                                              |
| ------------- | -------- | ------- | -------------------------------------------------------- |
| `name_filter` | no       | ---     | Case-insensitive substring filter on module name.        |
| `loaded_only` | no       | `false` | If true, only list loaded modules.                       |
| `game_only`   | no       | `false` | If true, only list game modules.                         |
| `limit`       | no       | all     | Maximum number of modules to return.                     |

## Output

| Field           | Type   | Description                                       |
| --------------- | ------ | ------------------------------------------------- |
| `count`         | number | Number of modules returned (after filtering)      |
| `total_modules` | number | Total number of known modules (before filter)     |
| `modules`       | array  | Array of module objects (see below)               |
| `name_filter`   | string | Applied name filter (if any)                      |
| `loaded_only`   | boolean| Whether only loaded modules were listed           |
| `game_only`     | boolean| Whether only game modules were listed             |

### Module object

| Field            | Type    | Description                              |
| ---------------- | ------- | ---------------------------------------- |
| `name`           | string  | Module name                              |
| `is_loaded`      | boolean | Whether the module is currently loaded   |
| `is_game_module` | boolean | Whether this is a game/project module    |
| `file_path`      | string  | Path to the module DLL (if available)    |

## Examples

### List all modules

```json
{}
```

### List only loaded modules

```json
{
  "loaded_only": true
}
```

### List game modules only

```json
{
  "game_only": true
}
```

### Search for a specific module

```json
{
  "name_filter": "UnrealMCPCore"
}
```

## Errors

This tool does not fail — it always returns available module information.
