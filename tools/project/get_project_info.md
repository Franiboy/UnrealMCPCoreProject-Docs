# get_project_info

Get project metadata: project name, file path, engine version, description, category, target platforms, enabled project plugins, project modules, and map assets. No required parameters.

## Input

```json
{}
```

| Parameter         | Required | Default | Description                                              |
| ----------------- | -------- | ------- | -------------------------------------------------------- |
| `include_maps`    | no       | `true`  | Include a list of all map assets under `/Game`.          |
| `include_plugins` | no       | `true`  | Include the list of project-referenced plugins.          |
| `include_modules` | no       | `true`  | Include the list of project modules.                     |

## Output

| Field               | Type    | Description                                              |
| ------------------- | ------- | -------------------------------------------------------- |
| `project_name`      | string  | The project name (e.g. `"MyGame"`)                       |
| `project_file`      | string  | Full path to the `.uproject` file                        |
| `project_dir`       | string  | Project root directory                                   |
| `engine`            | object  | Engine version details (see below)                       |
| `description`       | string  | Project description (if set)                             |
| `category`          | string  | Project category (if set)                                |
| `is_enterprise`     | boolean | Whether this is an enterprise project                    |
| `target_platforms`  | array   | List of target platform names (empty = all supported)    |
| `project_plugins`   | array   | Plugins referenced by the project (name, enabled)        |
| `num_project_plugins` | number | Count of project-referenced plugins                    |
| `project_modules`   | array   | Project modules (name, type, loading_phase, dependencies)|
| `num_project_modules` | number | Count of project modules                               |
| `maps`              | array   | Map assets (name, path)                                  |
| `num_maps`          | number  | Count of map assets                                      |

### Engine object

| Field       | Type   | Description                 |
| ----------- | ------ | --------------------------- |
| `version`   | string | Full version string         |
| `major`     | number | Major version number        |
| `minor`     | number | Minor version number        |
| `patch`     | number | Patch version number        |
| `changelist` | number | Build changelist           |
| `branch`    | string | Source branch               |

## Examples

### Get all project info

```json
{}
```

### Get only basic info (no maps, no plugins, no modules)

```json
{
  "include_maps": false,
  "include_plugins": false,
  "include_modules": false
}
```

## Errors

This tool does not fail — it always returns available project information.
